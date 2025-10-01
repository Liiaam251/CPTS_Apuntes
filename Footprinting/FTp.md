# Resumen: FTP y TFTP

## FTP (File Transfer Protocol)
- Protocolo antiguo de transferencia de archivos en la **capa de aplicación** (usa TCP).
- Puertos principales:
  - **21/TCP:** canal de control (envío de comandos).
  - **20/TCP:** canal de datos (transferencia de archivos).
- Requiere credenciales, pero a veces permite **acceso anónimo**.
- **FTP activo:** el cliente indica al servidor el puerto de datos (problemas con firewalls).
- **FTP pasivo:** el servidor indica el puerto de datos (supera bloqueos de firewall).
- Es un **protocolo en texto claro** (no cifrado), susceptible de interceptación.
- Permite:
  - Subir (`put`) y descargar (`get`) archivos.
  - Listar directorios (`ls`, `ls -R` para listar de forma recursiva).
  - Conexión anónima si está habilitada (`anonymous`).
- Se puede interactuar con el servidor vía:
  - `ftp <ip>`
  - `nc -nv <ip> 21` o `telnet <ip> 21` para pruebas básicas.
  - `openssl s_client -connect <ip>:21 -starttls ftp` para conexiones TLS/SSL.
- Ejemplo de conexión anónima:
  ```bash
  ftp 10.129.14.136
  Name: anonymous
  Password: (en blanco o cualquier valor)
  ```

## TFTP (Trivial FTP)
- Más simple que FTP.
- Usa **UDP** en lugar de TCP.
- **No requiere autenticación**, solo opera en redes internas y carpetas compartidas.
- Comandos principales:
  - `connect <host>` → conectar a servidor.
  - `get <archivo>` → descargar archivo.
  - `put <archivo>` → subir archivo.
  - `status` → ver estado de la conexión.
  - `quit` → salir.

## Configuración de vsFTPd
Archivo principal: `/etc/vsftpd.conf`  
Algunas opciones comunes:
- `anonymous_enable=YES` → habilitar acceso anónimo.
- `local_enable=YES` → permitir inicio de sesión de usuarios locales.
- `write_enable=YES` → permitir escritura (subir archivos).
- `chroot_local_user=YES` → restringir usuarios locales a su directorio.
- `hide_ids=YES` → oculta nombres de usuario/grupo, mostrando todo como "ftp".
- `ls_recurse_enable=YES` → permite listar carpetas recursivamente.

Archivo `/etc/ftpusers` → define usuarios **denegados** para el servicio FTP.

## Interacción y enumeración con Nmap
- Escaneo básico del puerto FTP:
  ```bash
  sudo nmap -sV -p21 -sC -A <ip>
  ```
- Scripts útiles de Nmap para FTP:
  - `ftp-anon` → comprueba acceso anónimo y muestra contenido.
  - `ftp-syst` → muestra el estado y versión del servidor FTP.
  - `ftp-brute` → prueba de fuerza bruta (requiere precaución).
- Actualizar scripts NSE:
  ```bash
  sudo nmap --script-updatedb
  ```
- Seguimiento de scripts:
  ```bash
  sudo nmap -sV -p21 -sC -A <ip> --script-trace
  ```

## Seguridad y buenas prácticas
- FTP **no es seguro** por defecto → usar FTPS (TLS/SSL) o SFTP.
- Deshabilitar acceso **anónimo** cuando no sea necesario.
- Limitar permisos de escritura a usuarios autorizados.
- Monitorizar registros para detectar actividades sospechosas.

---
**Resumen práctico:**  
FTP permite la transferencia de archivos pero expone credenciales en texto claro y puede permitir accesos inseguros (por ejemplo, con cuentas anónimas). TFTP es aún menos seguro y solo debe usarse en entornos controlados. Durante la enumeración, Nmap y clientes como `ftp`, `nc`, y `openssl` son esenciales para identificar configuraciones, versiones y posibles vectores de ataque.
