# Apuntes sobre SMB y smbclient

## 1. Introducción a SMB
- **SMB (Server Message Block)** es un protocolo cliente-servidor que permite el acceso a:
  - Archivos compartidos
  - Directorios
  - Impresoras y otros recursos de red
- Es usado principalmente en sistemas **Windows**, pero con **Samba** también en **Linux/Unix**.
- Permite comunicación entre cliente y servidor para compartir datos y recursos.

## 2. Versiones de SMB
| Versión  | Compatibilidad               | Características principales                          |
|----------|------------------------------|----------------------------------------------------|
| CIFS     | Windows NT 4.0                | Uso de NetBIOS                                     |
| SMB 1.0  | Windows 2000                  | Conexión directa a través de TCP                   |
| SMB 2.0  | Vista / Server 2008           | Mejor rendimiento y firma de mensajes              |
| SMB 2.1  | Windows 7 / Server 2008 R2    | Mecanismos de bloqueo                              |
| SMB 3.0+ | Windows 8/10 / Server 2012+   | Multicanal, cifrado de extremo a extremo           |

> ⚠️ **SMB1 está obsoleto** pero aún puede encontrarse en sistemas antiguos.

## 3. Samba
- Implementación de SMB en **Linux/Unix**.
- Archivos de configuración: `/etc/samba/smb.conf`
- Servicios principales:
  - `smbd`: servidor SMB
  - `nmbd`: compatibilidad con NetBIOS
- Para reiniciar Samba:
```bash
sudo systemctl restart smbd
```

## 4. Comandos smbclient
### Conexión anónima
```bash
smbclient -N -L //<IP o Host>
```
- `-N`: null session (sin usuario/contraseña)
- `-L`: lista los recursos compartidos

### Conexión autenticada
```bash
smbclient //<IP>/<share> -U <usuario>
```

### Exploración y descarga
Una vez conectado:
```bash
help        # Lista de comandos disponibles
ls          # Muestra archivos y directorios
cd <dir>    # Cambia de directorio
get <file>  # Descarga un archivo
mget *      # Descarga múltiples archivos
put <file>  # Sube un archivo
!ls         # Ejecuta un comando local en tu máquina
!cat <file> # Lee un archivo local
```

## 5. Ejemplo de conexión y descarga
```bash
smbclient -N -L //10.129.14.128

Sharename       Type      Comment
---------       ----      -------
print$          Disk      Printer Drivers
notes           Disk      CheckIT
IPC$            IPC       IPC Service

# Conexión al recurso "notes"
smbclient //10.129.14.128/notes

# Descargar archivo
get prep-prod.txt

# Ver archivo descargado en el sistema local
!ls
!cat prep-prod.txt
```

## 6. Enumeración con rpcclient
```bash
rpcclient -U "" <IP>

# Comandos útiles dentro de rpcclient
srvinfo                 # Información del servidor
enumdomains             # Lista de dominios
querydominfo            # Info de dominio
netshareenumall         # Lista de recursos compartidos
enumdomusers            # Enumera usuarios del dominio
queryuser <RID>         # Información detallada de un usuario
```

## 7. Enumeración con otras herramientas
### Nmap
```bash
sudo nmap -sV -sC -p139,445 <IP>
```

### SMBMap
```bash
smbmap -H <IP>
```

### CrackMapExec
```bash
crackmapexec smb <IP> --shares -u '' -p ''
```

### Enum4Linux-ng
```bash
git clone https://github.com/cddmp/enum4linux-ng.git
cd enum4linux-ng
pip3 install -r requirements.txt
./enum4linux-ng.py <IP> -A
```

---
**Nota:** La enumeración SMB puede revelar usuarios, recursos compartidos y configuraciones inseguras que pueden explotarse en auditorías de seguridad.
