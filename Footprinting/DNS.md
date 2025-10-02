# 📝 Apuntes de DNS para CTF

## 1. ¿Qué es DNS?
- **DNS (Domain Name System)** traduce **nombres de dominio** (por ej. `www.inlanefreight.htb`) en **direcciones IP** (por ej. `10.129.10.5`).
- Se comporta como una **“guía telefónica” de Internet**.
- En un entorno corporativo o en los **CTF**, el DNS suele contener **información interna** que **no siempre es visible en la web**.

👉 Para un atacante, **enumerar DNS** sirve para descubrir:
- **Subdominios** (p. ej.: `mail.inlanefreight.htb`, `dev.inlanefreight.htb`).
- **Servicios internos** ocultos.
- **Registros TXT** (a menudo usados para flags en los CTF).
- **Infraestructura interna** (por ej., servidores de correo, app servers, etc.).

---

## 2. Tipos de registros DNS importantes
| Registro | Descripción | Relevancia en CTF |
|----------|-------------|-------------------|
| **A**    | Asocia un dominio a una **IP IPv4**. | Permite encontrar hosts y servicios. |
| **NS**   | Indica los **Name Servers** (servidores DNS). | Sirve para descubrir otros servidores autoritativos. |
| **MX**   | Servidores de correo del dominio. | A veces exponen hosts internos. |
| **TXT**  | Contiene texto libre. | Muchos **CTF esconden las flags aquí**. |
| **CNAME**| Alias de otro nombre de dominio. | Útil para encontrar dominios alternativos. |
| **PTR**  | Búsqueda inversa (IP → nombre). | Útil para mapear redes internas. |
| **SOA**  | Información administrativa del dominio. | Sirve para confirmar autoridad del DNS. |

---

## 3. Por qué el DNS es clave en los CTF
- Los **admins de CTF suelen esconder pistas** o **flags** en el DNS porque:
  - Es **fácil de pasar por alto**.
  - Permite mostrar **infraestructura interna realista** (por ejemplo: `internal.inlanefreight.htb`).
- Muchas veces, **enumerar DNS → descubrir subdominios → encontrar servicios/flags**.

---

## 4. Comandos básicos con `dig`

### 4.1 Consultar un registro A
```bash
dig @<IP_DNS> <dominio> +short
```
- Traduce el dominio a su IP.

---

### 4.2 Ver los servidores NS
```bash
dig @<IP_DNS> NS <dominio>
```
- Muestra los servidores de nombres autoritativos.

---

### 4.3 Consultar **cualquier tipo** de registro
```bash
dig @<IP_DNS> ANY <dominio>
```
- A veces muestra **TXT, A, MX, SOA…** todo lo que el servidor permita.

---

### 4.4 Transferencia de zona (**AXFR**)
```bash
dig @<IP_DNS> AXFR <dominio>
```
- 🔑 **El más importante en CTF**: descarga **todos los registros del dominio** si el servidor no está bien configurado.
- Es la forma más rápida de encontrar:
  - **Subdominios ocultos**
  - **Registros TXT con flags**
  - IPs internas.

---

### 4.5 Buscar registros TXT (flag típica)
```bash
dig @<IP_DNS> TXT <dominio>
```
- Útil cuando ya sabes que hay un TXT (por ejemplo, la flag `HTB{...}`).

---

### 4.6 Buscar registros específicos (ejemplo: MX, SOA)
```bash
dig @<IP_DNS> MX <dominio>
dig @<IP_DNS> SOA <dominio>
```

---

## 5. Herramienta de enumeración DNS: `dnsenum`
```bash
dnsenum --dnsserver 10.129.210.242 --enum -p 0 -s 0 -o subdomains.txt -f subdomains-top1million-110000.txt inlanefreight.htb
```
- **`--dnsserver`** → IP del servidor DNS objetivo.  
- **`--enum`** → realiza una enumeración completa.  
- **`-p 0` y `-s 0`** → controla la profundidad de la enumeración.  
- **`-o subdomains.txt`** → guarda los resultados en un archivo.  
- **`-f subdomains-top1million-110000.txt`** → usa una lista de palabras para fuerza bruta de subdominios.  

🔑 Sirve para **enumerar automáticamente subdominios y registros**. Muy útil cuando el servidor no permite AXFR o para descubrir subdominios adicionales.

---

## 6. Metodología práctica para CTF

1. **Identificar el servicio DNS** con `nmap`:
```bash
nmap -p53 -sV <IP_OBJETIVO>
```

2. **Resolver el dominio**:
```bash
dig @<IP_DNS> <dominio>
```

3. **Descubrir los NS**:
```bash
dig @<IP_DNS> NS <dominio>
```

4. **Intentar transferencia de zona**:
```bash
dig @<IP_DNS> AXFR <dominio>
```

5. **Revisar subdominios descubiertos**:
```bash
dig @<IP_DNS> AXFR <subdominio.dominio>
```

6. **Buscar flags** en los registros TXT:
```bash
dig @<IP_DNS> TXT <dominio/subdominio>
```

---

## 7. Conceptos clave
- **SOA**: confirma que el servidor es autoritativo.  
- **NS**: te dice a qué servidor preguntar.  
- **AXFR**: transferencia de zona → si está habilitada es casi siempre **win**.  
- **TXT**: suelen esconder las flags.  
- **ANY**: te puede dar todo de golpe (pero a menudo está restringido).  
- **CTF tip**: si no ves la flag en el dominio principal, prueba subdominios (`internal.`, `dev.`, etc.).

---

## 8. Resumen ultra-corto de comandos
```bash
# Resolver dominio a IP
dig @IP_DNS dominio +short

# Ver Name Servers
dig @IP_DNS NS dominio

# Ver cualquier registro disponible
dig @IP_DNS ANY dominio

# Intentar transferencia de zona
dig @IP_DNS AXFR dominio

# Ver TXT (para flags)
dig @IP_DNS TXT dominio

# Transferencia en subdominios
dig @IP_DNS AXFR sub.dominio

# Enumeración con fuerza bruta de subdominios
dnsenum --dnsserver <IP_DNS> --enum -p 0 -s 0 -o subdomains.txt -f subdomains-top1million-110000.txt <dominio>
```

---

## 9. Por qué el DNS es importante en CTF
- Permite **mapear infraestructura oculta**.  
- Muchas veces contiene **subdominios sensibles** no visibles en la web.  
- Los **registros TXT** suelen guardar las **flags**.  
- Si el **AXFR está abierto**, puedes ver **toda la base de datos DNS del dominio**: es como tener un mapa completo del objetivo.

