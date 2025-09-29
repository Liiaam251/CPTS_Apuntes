# Términos Comunes en Pentesting e Infosec

Las **pruebas de penetración** y el **hacking ético** abarcan una gran variedad de tecnologías y conceptos. A continuación, se explican los términos y componentes más frecuentes que todo profesional debe comprender para avanzar en los módulos fundamentales y laboratorios iniciales (por ejemplo, Hack The Box).

---

## 1. Shell

Un **shell** es un programa que actúa como interfaz entre el usuario y el sistema operativo, permitiendo ejecutar comandos.  
En sistemas Linux, el shell más común es **Bash (Bourne Again Shell)**, evolución del shell original de Unix (`sh`). Otros shells incluyen **Zsh**, **Ksh**, **Tcsh** y **Fish Shell**.  
En Windows, sus equivalentes son **cmd.exe** y **PowerShell**.

### Obtener un Shell
Cuando hablamos de “obtener un shell en un sistema objetivo”, nos referimos a **ganar acceso remoto interactivo** a dicho host, con capacidad de ejecutar comandos. Esto puede lograrse mediante:
- Explotación de vulnerabilidades en aplicaciones web o servicios de red.
- Uso de credenciales válidas para autenticarse remotamente.

### Tipos principales de Shell
| Tipo             | Descripción                                                                                     |
|------------------|-------------------------------------------------------------------------------------------------|
| **Reverse Shell**| El host comprometido inicia una conexión hacia el equipo atacante (listener).                    |
| **Bind Shell**   | El host comprometido abre un puerto en escucha para que el atacante se conecte a él.             |
| **Web Shell**    | Permite ejecutar comandos del sistema operativo a través de un navegador web, normalmente de forma no interactiva o semiinteractiva. |

Los shells pueden implementarse en diversos lenguajes (**Python, Perl, Bash, PHP, Go, Java, awk, etc.**) y pueden ser simples scripts o herramientas avanzadas.

---

## 2. Puertos

Un **puerto** es un punto lógico de comunicación donde inician y finalizan las conexiones de red en un sistema.  
Puede imaginarse como **“puertas o ventanas”** a través de las cuales los servicios reciben y envían datos.

Los puertos son gestionados por el sistema operativo y permiten diferenciar los tipos de tráfico:
- **HTTP:** suele usar el puerto **80/TCP**.
- **HTTPS:** suele usar el puerto **443/TCP**.
- **SSH:** puerto **22/TCP**.
- Otros servicios pueden configurarse en puertos no estándar.

### Categorías de Puertos
- **TCP (Transmission Control Protocol):** orientado a la conexión, requiere establecer un enlace entre cliente y servidor.
- **UDP (User Datagram Protocol):** sin conexión, sin garantía de entrega; más rápido pero menos fiable.

Hay **65.535 puertos TCP** y **65.535 puertos UDP** numerados del 1 al 65.535.  
Conocer los puertos comunes es esencial para los pentesters, ya que facilita la **enumeración y priorización de objetivos**.

| Puerto(s)        | Protocolo / Servicio                        |
|------------------|----------------------------------------------|
| 20/21 (TCP)      | FTP                                          |
| 22 (TCP)         | SSH                                          |
| 23 (TCP)         | Telnet                                       |
| 25 (TCP)         | SMTP                                         |
| 80 (TCP)         | HTTP                                         |
| 161 (TCP/UDP)    | SNMP                                         |
| 389 (TCP/UDP)    | LDAP                                         |
| 443 (TCP)        | SSL/TLS (HTTPS)                              |
| 445 (TCP)        | SMB                                          |
| 3389 (TCP)       | RDP                                          |

> Memorizar los puertos más usados (por ejemplo, **21 → FTP**, **80 → HTTP**, **88 → Kerberos**) acelera el trabajo en auditorías y facilita la interpretación de resultados de escaneos (por ejemplo, con **Nmap**).

---

## 3. Servidor Web

Un **servidor web** es una aplicación en el **back-end** que gestiona el tráfico **HTTP/HTTPS** entre los navegadores de los clientes y las páginas o recursos solicitados.  
Opera generalmente en los puertos **80/TCP** (HTTP) o **443/TCP** (HTTPS).

Las aplicaciones web suelen estar **expuestas a internet** y, por tanto, representan un objetivo atractivo.  
Las vulnerabilidades en estas aplicaciones pueden comprometer tanto el servidor web como la red interna de la organización.

---

## 4. OWASP Top 10: Vulnerabilidades Comunes en Aplicaciones Web

El **OWASP Top 10** es un estándar reconocido que lista las principales categorías de vulnerabilidades en aplicaciones web:

| Nº  | Categoría                               | Descripción breve                                                                    |
|-----|-----------------------------------------|--------------------------------------------------------------------------------------|
| 1   | **Control de acceso roto**              | Permite acceso indebido a datos, funciones o cuentas de otros usuarios.              |
| 2   | **Fallos criptográficos**               | Uso incorrecto o débil de algoritmos criptográficos que expone datos sensibles.      |
| 3   | **Inyección**                            | Falta de validación de entradas (SQLi, Command Injection, LDAPi, etc.).              |
| 4   | **Diseño inseguro**                      | Arquitectura de la aplicación sin considerar principios de seguridad.                 |
| 5   | **Mala configuración de seguridad**     | Configuraciones predeterminadas, almacenamiento inseguro, mensajes de error excesivos.|
| 6   | **Componentes vulnerables/obsoletos**   | Uso de librerías, frameworks o módulos sin soporte o con fallos conocidos.           |
| 7   | **Fallos de identificación/autenticación** | Gestión inadecuada de credenciales, sesiones y procesos de autenticación.            |
| 8   | **Fallos de integridad de software y datos** | Uso de dependencias no confiables o sin verificación de integridad.                 |
| 9   | **Fallos de registro y monitoreo**      | Ausencia de logs y alertas, impidiendo detectar y responder a incidentes.            |
| 10  | **Falsificación de solicitudes del lado del servidor (SSRF)** | La aplicación procesa peticiones manipuladas hacia destinos internos. |

Estas categorías sirven como **punto de partida** para metodologías de auditoría de seguridad web.

---


