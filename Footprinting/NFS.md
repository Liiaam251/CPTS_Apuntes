# Apuntes: Sistema Operativo NFS

## Introducción
**Network File System (NFS)** es un sistema de archivos de red desarrollado por **Sun Microsystems** con el objetivo de permitir acceder a sistemas de archivos remotos a través de la red **como si fueran locales**.  
Su funcionamiento es similar a SMB, pero utiliza un protocolo distinto y se usa principalmente entre sistemas **Linux y Unix**.

- Los clientes NFS no pueden comunicarse directamente con los servidores SMB.
- NFS es un **estándar de Internet** para sistemas de archivos distribuidos.
- A partir de **NFSv4**, la autenticación requiere usuario (no solo cliente).

---

## Versiones de NFS
| Versión | Características principales |
|---------|-----------------------------|
| **NFSv2** | Versión más antigua. Utilizaba UDP y sigue siendo compatible con muchos sistemas. |
| **NFSv3** | Soporte para tamaño de archivo variable, mejores informes de errores y más funciones. No es 100% compatible con clientes NFSv2. |
| **NFSv4** | Soporta **Kerberos**, funciona con **firewalls** e Internet, **usa un solo puerto (2049)**, soporta ACL, alto rendimiento y seguridad. Primera versión con protocolo **con estado**. |
| **NFSv4.1** | Añade **pNFS** para acceso paralelo a archivos distribuidos y **multiruta NFS**. Optimiza implementaciones en clúster. |

**Ventaja NFSv4:** Solo usa el **puerto 2049 (UDP/TCP)**, lo que facilita su configuración en cortafuegos.

---

## Funcionamiento
NFS se basa en el protocolo **ONC-RPC/SUN-RPC** y usa **XDR** para intercambio de datos independiente de la plataforma.

- Autenticación predeterminada mediante **UID/GID** de UNIX.
- Debe usarse en **redes de confianza**, ya que servidor y cliente no siempre comparten las mismas asignaciones UID/GID.

---

## Configuración básica
El archivo principal es **`/etc/exports`**, donde se definen los directorios a compartir y los permisos:

```bash
# Ejemplo de configuración NFSv3
/srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)

# Ejemplo de configuración NFSv4
/srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
/srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
```

### Opciones comunes
| Opción | Descripción |
|--------|-------------|
| **rw** | Permisos de lectura y escritura. |
| **ro** | Solo lectura. |
| **sync** | Transferencia síncrona (más lenta, pero más segura). |
| **async** | Transferencia asíncrona (más rápida). |
| **secure** | Solo permite puertos < 1024. |
| **insecure** | Permite puertos > 1024. |
| **no_subtree_check** | Deshabilita la verificación de subdirectorios. |
| **root_squash** | Restringe privilegios de root, asignándolo a un UID/GID anónimo. |

Ejemplo para compartir carpeta `/mnt/nfs` con toda la subred:
```bash
echo '/mnt/nfs  10.129.14.0/24(sync,no_subtree_check)' >> /etc/exports
systemctl restart nfs-kernel-server
exportfs
```

---

## Configuraciones peligrosas
| Opción | Riesgo |
|--------|--------|
| **rw** | Permite modificar archivos compartidos. |
| **insecure** | Permite puertos altos (>1024), menos seguro. |
| **nohide** | Exporta subdirectorios montados debajo de un directorio exportado. |
| **no_root_squash** | Permite que root mantenga UID/GID 0, lo que da acceso completo. |

---

## Seguridad y huella del servicio
- Puertos principales: **111 (rpcbind)** y **2049 (NFS)**.
- Podemos detectar NFS con herramientas como **nmap**:

```bash
sudo nmap 10.129.14.128 -p111,2049 -sV -sC
sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049
```

- El script **rpcinfo** permite listar servicios RPC activos.
- El script **nfs-ls** muestra contenidos de los recursos compartidos.

---

## Montaje de un recurso NFS
```bash
showmount -e 10.129.14.128
mkdir target-NFS
sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
cd target-NFS
tree .
```

Desmontaje:
```bash
cd ..
sudo umount ./target-NFS
```

---

## Inspección de permisos y UID/GID
```bash
ls -l mnt/nfs/
ls -n mnt/nfs/
```

- Podemos visualizar nombres de usuario, grupos y permisos.
- Si la opción **root_squash** está habilitada, ni siquiera root podrá modificar ciertos archivos.
