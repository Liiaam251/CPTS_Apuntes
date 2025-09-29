# Apuntes: Enumeración Web

## 1. Introducción
- Los servidores web suelen encontrarse en los puertos **80 (HTTP)** y **443 (HTTPS)**.
- Alojan aplicaciones web que representan una **superficie de ataque crítica**.
- La **enumeración web** es esencial cuando hay pocos servicios expuestos o cuando los servicios principales están parcheados.

---

## 2. Gobuster
Herramienta para **descubrir directorios, archivos ocultos y subdominios**.

### 2.1 Enumeración de directorios/archivos
Ejemplo de uso:
```bash
gobuster dir -u http://10.10.10.121/ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

Salida de ejemplo:
```
/.hta (Status: 403)
/.htpasswd (Status: 403)
/.htaccess (Status: 403)
/index.php (Status: 200)
/server-status (Status: 403)
/wordpress (Status: 301)
```

- **200 OK** → Solicitud exitosa.
- **403 Forbidden** → Recurso denegado.
- **301 Moved Permanently** → Redirección.

El análisis reveló un **WordPress** instalado en `/wordpress`, aún en configuración, con posibilidad de **RCE (Remote Code Execution)**.

---

### 2.2 Enumeración de subdominios DNS
Posibles paneles de administración o servicios adicionales.

Instalación de **SecLists**:
```bash
git clone https://github.com/danielmiessler/SecLists
sudo apt install seclists -y
```

Ejemplo de enumeración de subdominios:
```bash
gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```

Salida de ejemplo:
```
Found: blog.inlanefreight.com
Found: customer.inlanefreight.com
Found: my.inlanefreight.com
Found: ns1.inlanefreight.com
Found: ns2.inlanefreight.com
Found: ns3.inlanefreight.com
```

---

## 3. Captura de banners y encabezados
Revelan información sobre frameworks, autenticación y configuraciones de seguridad.

Ejemplo con **cURL**:
```bash
curl -IL https://www.inlanefreight.com
```

Salida de ejemplo:
```
HTTP/1.1 200 OK
Date: Fri, 18 Dec 2020 22:24:05 GMT
Server: Apache/2.4.29 (Ubuntu)
Link: <https://www.inlanefreight.com/index.php/wp-json/>; rel="https://api.w.org/"
Link: <https://www.inlanefreight.com/>; rel=shortlink
Content-Type: text/html; charset=UTF-8
```

---

## 4. EyeWitness
- Herramienta para tomar **capturas de pantalla de aplicaciones web**.
- Ayuda a identificar visualmente las aplicaciones y credenciales predeterminadas.

---

## 5. WhatWeb
Extrae información sobre **versiones de servidores, frameworks y aplicaciones web**.

Ejemplo básico:
```bash
whatweb 10.10.10.121
```

Salida de ejemplo:
```
http://10.10.10.121 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], Email[license@php.net], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.121], Title[PHP 7.4.3 - phpinfo()]
```

Escaneo de toda la red:
```bash
whatweb --no-errors 10.10.10.0/24
```

Salida de ejemplo:
```
http://10.10.10.11 [200 OK] HTTPServer[nginx/1.14.1], Title[Test Page for the Nginx HTTP Server]
http://10.10.10.100 [200 OK] HTTPServer[Apache/2.4.41 (Ubuntu)], Title[File Sharing Service]
http://10.10.10.247 [200 OK] Bootstrap, HTTPServer[OpenBSD httpd], Title[Fine Wines], PHP[7.4.12]
```

---

## 6. Certificados SSL/TLS
- Pueden revelar datos como **correo electrónico, nombre de la empresa y ubicación**.
- Útiles para ataques de **phishing** o reconocimiento adicional.

Ejemplo: inspeccionar el certificado de `https://10.10.10.121/`.

---

## 7. Robots.txt
- Indica a los motores de búsqueda qué rutas no indexar.
- Puede revelar **directorios sensibles**.

Ejemplo: el archivo `robots.txt` incluye:
```
Disallow: /private
Disallow: /uploaded_files
```

Al navegar `http://10.10.10.121/private` se accede a una **página de login de administrador**.

---

## 8. Revisión del código fuente
- Abrir con **CTRL+U** en el navegador.
- Buscar **comentarios de desarrolladores** que pueden contener **credenciales** o pistas.

Ejemplo: credenciales de una cuenta de prueba comentadas en el código.

---

## 9. Herramientas clave
- **Gobuster / ffuf** → Enumeración de directorios, archivos y subdominios.
- **cURL** → Revisión de encabezados de servidor.
- **WhatWeb** → Identificación de frameworks y tecnologías.
- **EyeWitness** → Capturas de pantallas de aplicaciones.
- **SecLists** → Listas de palabras para fuerza bruta.
