# Apuntes: Escaneo de Servicios y Enumeración de Red

## 1. Conceptos básicos
- **Servicio**: Aplicación que se ejecuta en un equipo para ofrecer funciones a otros usuarios o máquinas (servidores).
- **Dirección IP**: Identifica de forma única a un dispositivo en la red.
- **Puertos**:
  - Rango total: **1–65.535**.
  - **1–1.023**: Puertos conocidos y privilegiados.
  - **0**: Puerto reservado en TCP/IP (no se usa en mensajes TCP/UDP).
  - Un servicio se accede por **IP + puerto** y debe hablar el protocolo adecuado.
- **Objetivo del escaneo**: Localizar servicios mal configurados o con vulnerabilidades para explotarlos.

---

## 2. Herramienta principal: Nmap (Network Mapper)
- Escanea rangos de puertos para descubrir servicios activos.
- Por defecto analiza **1.000 puertos más comunes** con:

```bash
nmap <IP>
```

- Muestra:
  - **PORT**: Puerto y protocolo (por defecto TCP).
  - **STATE**: Estado del puerto (`open`, `closed`, `filtered`).
  - **SERVICE**: Servicio asociado al puerto (basado en base de datos interna).

Ejemplo de salida:
```
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

---

## 3. Escaneos avanzados
- Parámetros clave:
  - `-p-` → Escanea **todos los puertos (1–65.535)**.
  - `-sV` → **Detección de versiones** de servicios.
  - `-sC` → **Ejecuta scripts** predeterminados de Nmap.
  - `-A` → Escaneo agresivo (detector de SO + traceroute + scripts + versiones).

Ejemplo:
```bash
nmap -sV -sC -p- <IP>
```

- Obtención de datos adicionales:
  - Versión de servicios (ej. `vsftpd 3.0.3`, `OpenSSH 8.2p1 Ubuntu`).
  - Detección aproximada del **SO** (ej. Ubuntu 20.04 Focal Fossa).
  - Información de encabezados web (`http-server-header`, `http-title`).

---

## 4. Scripts de Nmap
- Permiten auditar vulnerabilidades específicas.

Ejemplo (Citrix):
```bash
nmap --script <script_name> -p<puerto> <IP>
```

Localización de scripts:
```bash
locate scripts/citrix
```

- Scripts comunes:
  - `ftp-anon` → Comprueba acceso anónimo a FTP.
  - `smb-os-discovery` → Enumera SO a través de SMB.
  - `banner` → Captura banners de servicios.

---

## 5. Captura de banners
- Técnica para **identificar rápidamente versiones de servicios**.

Con Nmap:
```bash
nmap -sV --script=banner -p<puerto> <IP>
```

Con Netcat (manual):
```bash
nc -nv <IP> <puerto>
```

Ejemplo de salida:
```
220 (vsFTPd 3.0.3)
```

---

## 6. Protocolos comunes y enumeración

### 6.1 FTP (Puerto 21)
- Permite transferencia de archivos.
- Puede habilitar **login anónimo**.

Ejemplo de escaneo:
```bash
nmap -sC -sV -p21 <IP>
```

Conexión:
```bash
ftp -p <IP>
Name: anonymous
```

Comandos útiles:
- `ls` → Lista archivos.
- `cd` → Cambiar de directorio.
- `get <archivo>` → Descargar archivo.

Ejemplo de credenciales encontradas: `login.txt → admin:ftp@dmin123`.

---

### 6.2 SMB (Puertos 139 y 445)
- **Protocolo de compartición de archivos en Windows**.
- Puede exponer recursos compartidos y credenciales.

Escaneo:
```bash
nmap --script smb-os-discovery.nse -p445 <IP>
```

Enumeración de recursos compartidos:
```bash
smbclient -N -L \\<IP>
```

Conexión a recurso compartido:
```bash
smbclient -U <usuario> \\<IP>\<recurso>
```

Comandos:
- `ls` → Lista contenido.
- `cd <dir>` → Cambia de directorio.
- `get <archivo>` → Descarga archivo.

Ejemplo: acceso a `users\bob\passwords.txt`.

---

### 6.3 SNMP (Puertos 161/162)
- Proporciona **información de dispositivos de red**.
- **v1/v2c**: control mediante **cadenas de comunidad** (`public` / `private`).
- **v3**: añade cifrado y autenticación.

Enumeración:
```bash
snmpwalk -v 2c -c public <IP> 1.3.6.1.2.1.1.5.0
```

Fuerza bruta de cadenas de comunidad:
```bash
onesixtyone -c dict.txt <IP>
```

---

## 7. Ejemplos prácticos de escaneo

Escaneo básico:
```bash
nmap <IP>
```

Escaneo completo con versiones y scripts:
```bash
nmap -sV -sC -p- <IP>
```

Escaneo de SMB:
```bash
nmap -A -p445 <IP>
```

Enumeración de recursos SMB:
```bash
smbclient -N -L \\<IP>
```

Acceso a FTP anónimo:
```bash
ftp -p <IP>
```

Captura de banners:
```bash
nmap -sV --script=banner -p21 <IP>
```

---

## 8. Buenas prácticas
- **Escanear puertos completos** si no se encuentra el servicio deseado en el escaneo básico.
- **Identificar el SO** mediante la combinación de información de puertos y banners.
- **Enumerar servicios con scripts específicos** para ahorrar tiempo.
- **Buscar credenciales** en archivos compartidos o accesos anónimos.
- **Cuidado con falsos positivos**: versiones de servicios no siempre indican la versión exacta del SO.

---

## 9. Conclusiones
- El **escaneo y enumeración de servicios** es esencial para la fase de reconocimiento y para descubrir puntos débiles en un sistema.
- Herramientas principales: **Nmap**, **Netcat**, **ftp**, **smbclient**, **snmpwalk**, **onesixtyone**.
- La información recolectada (banners, versiones, credenciales) guía los siguientes pasos de explotación.
