# Nibbles - Huella web

## 1. Identificación inicial con `whatweb`

```bash
whatweb 10.129.42.190
```

**Salida:**
- Servidor: Apache/2.4.18 (Ubuntu)
- Mensaje en el navegador: “¡Hola mundo!”

Revisando el **código fuente** de la página aparece un comentario:
```html
<!-- /nibbleblog/ directory. Nothing interesting here! -->
```

Esto indica la presencia del directorio **`/nibbleblog/`**.

---

## 2. Confirmación del directorio

```bash
whatweb http://10.129.42.190/nibbleblog
```

- Detecta **Nibbleblog** (motor de blogs en PHP)
- Tecnologías: **HTML5**, **jQuery**, **PHP**
- Cookies: `PHPSESSID`

---

## 3. Enumeración con `gobuster`

```bash
gobuster dir -u http://10.129.42.190/nibbleblog/ --wordlist /usr/share/seclists/Discovery/Web-Content/common.txt
```

**Resultados importantes:**
- `/admin` → redirige a `/admin.php`
- `/content`
- `/languages`
- `/plugins`
- `/README`
- `/themes`

---

## 4. Verificación de versión

```bash
curl http://10.129.42.190/nibbleblog/README
```

**Resultado:**
- Nibbleblog **v4.0.3** (probablemente vulnerable a **File Upload Exploit**)

---

## 5. Exploración de directorios

- **`/themes`** → listado de plantillas
- **`/content/private/users.xml`** → revela el usuario **admin**
- **`/content/private/config.xml`** → configuración del sitio
- Confirmación de que el listado de directorios está **habilitado**

---

## 6. Hallazgos clave

- Portal de administración en `/nibbleblog/admin.php`
- Usuario válido: **admin**
- La IP se bloquea tras varios intentos de login fallidos → **Blacklist Protection**
- Pistas en el sitio: nombre **“nibbles”** usado también como posible contraseña
- Nibbleblog v4.0.3 vulnerable a **File Upload Exploit**

---

## 7. Conclusiones

1. Comenzamos con un escaneo básico (`nmap`) que reveló puertos abiertos.
2. Identificamos Nibbleblog con `whatweb`.
3. Enumeramos directorios con `gobuster`.
4. Hallamos credenciales parciales y confirmamos el usuario administrador.
5. Descubrimos protecciones de seguridad y potenciales vectores de ataque.
6. La enumeración exhaustiva fue clave para reunir piezas críticas del rompecabezas.

> **Nota:** La metodología sistemática y la toma de notas son esenciales para no perder información útil durante el proceso de enumeración.
