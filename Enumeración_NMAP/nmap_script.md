# Motor de Scripts de Nmap (NSE)

El **Nmap Scripting Engine (NSE)** permite extender las capacidades de Nmap usando **scripts en Lua** para interactuar con servicios, descubrir vulnerabilidades y recolectar más información.

---

## 1. Categorías de Scripts

| Categoría      | Descripción                                                        |
|----------------|--------------------------------------------------------------------|
| **auth**       | Descubrimiento de credenciales de autenticación.                   |
| **broadcast**  | Descubrimiento de hosts mediante difusión (broadcast).             |
| **brute**      | Fuerza bruta para inicio de sesión en servicios.                    |
| **default**    | Scripts predeterminados que se ejecutan con la opción `-sC`.        |
| **discovery**  | Enumeración de servicios accesibles.                                |
| **dos**        | Pruebas de denegación de servicio (no recomendadas en producción). |
| **exploit**    | Intento de explotación de vulnerabilidades conocidas.              |
| **external**   | Usa servicios externos para análisis adicionales.                  |
| **fuzzer**     | Envío de datos anómalos para detectar fallos o vulnerabilidades.    |
| **intrusive**  | Scripts intrusivos que pueden afectar al objetivo.                  |
| **malware**    | Comprobación de infecciones de malware.                             |
| **safe**       | Scripts seguros, no intrusivos.                                     |
| **version**    | Extienden la detección de versiones de servicios.                   |
| **vuln**       | Identificación de vulnerabilidades conocidas.                       |

---

## 2. Uso Básico de Scripts

### Ejecutar scripts predeterminados
```bash
sudo nmap <objetivo> -sC
```

### Ejecutar scripts de una categoría específica
```bash
sudo nmap <objetivo> --script <categoría>
```

### Ejecutar scripts individuales
```bash
sudo nmap <objetivo> --script <script1>,<script2>,<script3>
```

---

## 3. Ejemplos Prácticos

### Escaneo de puerto SMTP con scripts específicos
```bash
sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
```
- **`banner`** → Muestra el banner del servicio (puede revelar el SO o aplicación).
- **`smtp-commands`** → Enumera los comandos soportados por el servidor SMTP.

---

### Escaneo Agresivo (-A)
```bash
sudo nmap 10.129.2.28 -p 80 -A
```
Incluye:
- **Detección de servicios:** `-sV`
- **Detección de SO:** `-O`
- **Traceroute:** `--traceroute`
- **Scripts NSE predeterminados:** `-sC`

Permite descubrir:
- Servidor web → `Apache 2.4.29`
- Aplicación web → `WordPress 5.3.4`
- Título de la web → `blog.inlanefreight.com`
- Probable SO → `Linux`

---

### Evaluación de Vulnerabilidades (categoría vuln)
```bash
sudo nmap 10.129.2.28 -p 80 -sV --script vuln
```
- **`-sV`** → Detecta versiones de servicios.
- **`--script vuln`** → Ejecuta todos los scripts de la categoría `vuln`.

Ejemplo de hallazgos:
- Páginas de WordPress (`/wp-login.php`, `/readme.html`)
- Enumeración de usuarios de WordPress.
- Vulnerabilidades conocidas (ej.: **CVE-2019-0211**, **CVE-2018-1312**).

---

## 4. Resumen de Opciones Clave

| Opción                | Descripción                                             |
|-----------------------|---------------------------------------------------------|
| `-sC`                 | Ejecuta scripts NSE predeterminados.                     |
| `--script <cat>`      | Ejecuta todos los scripts de la categoría indicada.      |
| `--script <s1,s2>`    | Ejecuta scripts específicos por nombre.                  |
| `-A`                  | Escaneo agresivo: servicios, SO, traceroute y scripts.   |
| `-sV`                 | Detecta versiones de servicios.                          |
