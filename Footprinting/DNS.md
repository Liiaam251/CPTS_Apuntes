# Apuntes: DNS (Domain Name System)

## 1) Conceptos clave
- **DNS** traduce nombres de dominio ↔ direcciones IP (sin base de datos central; arquitectura distribuida).
- Permite ubicar servicios (web, correo, etc.) asociados a un dominio.
- El tráfico DNS **normalmente no va cifrado**. Alternativas: **DoT**, **DoH** y **DNSCrypt**.

---

## 2) Tipos de servidores DNS
| Tipo | Descripción breve |
|-----|--------------------|
| **Root Server** | Gestionan TLDs. 13 identidades lógicas coordinadas por **ICANN**. Último recurso en la recursión. |
| **Authoritative** | Autoridad de una **zona**. Respuestas vinculantes solo de su ámbito. |
| **Non‑authoritative** | No tienen autoridad; responden usando caché o consultas recursivas/iterativas. |
| **Caching** | Guardan respuestas durante el **TTL** indicado por el autoritativo. |
| **Forwarder** | Reenvía consultas a otros DNS. |
| **Resolver** | Realiza la resolución **local** (equipo/routers). |

---

## 3) Jerarquía y nomenclatura
- Raíz `.` → **TLDs** (`.com`, `.org`, `.io`, …) → **2º nivel** (`inlanefreight.com`) → **subdominios** (`dev.inlanefreight.com`) → **hosts** (`ws01.dev.inlanefreight.com`).

---

## 4) Registros DNS fundamentales
| Registro | ¿Para qué sirve? |
|---------|-------------------|
| **A** | IPv4 de un nombre. |
| **AAAA** | IPv6 de un nombre. |
| **MX** | Servidores de correo para el dominio. |
| **NS** | Nameservers autoritativos de la zona. |
| **TXT** | Información arbitraria (SPF, DMARC, validaciones, etc.). |
| **CNAME** | Alias de otro nombre (p. ej. `www` → `@`). |
| **PTR** | Búsqueda inversa: IP → nombre. |
| **SOA** | Datos de la zona: responsable, serie, timers (refresh/retry/expire/minimum). |

**Nota SOA:** en el correo del responsable el punto `.` se lee como `@`.  
Ej.: `awsdns-hostmaster.amazon.com.` ⇒ `awsdns-hostmaster@amazon.com`.

---

## 5) Herramientas de consulta rápidas
```bash
# SOA de un nombre
dig soa www.inlanefreight.com

# Nameservers de una zona preguntando a un DNS concreto
dig ns inlanefreight.htb @10.129.14.128

# Ver (cuando el servidor lo permite) registros variados
dig any inlanefreight.htb @10.129.14.128

# Consultar versión (si expuesta) - clase CHAOS
dig CH TXT version.bind 10.129.120.85
```

---

## 6) Bind9 – archivos y estructura
**Ficheros locales típicos**
- `/etc/bind/named.conf.local`
- `/etc/bind/named.conf.options`
- `/etc/bind/named.conf.log` (opcional)

**Ejemplo `named.conf.local`**
```conf
zone "domain.com" {
    type master;
    file "/etc/bind/db.domain.com";
    allow-update { key rndc-key; };
};
```

### 6.1) Archivo de **zona directa** (forward)
Debe contener **1 SOA**, ≥ **1 NS** y los RR necesarios.
```zone
$ORIGIN domain.com.
$TTL 86400
@   IN SOA dns1.domain.com. hostmaster.domain.com. (
        2001062501  ; serial
        21600       ; refresh
        3600        ; retry
        604800      ; expire
        86400 )     ; minimum

    IN NS  ns1.domain.com.
    IN NS  ns2.domain.com.
    IN MX  10 mx.domain.com.
    IN MX  20 mx2.domain.com.
    IN A   10.129.14.5

server1  IN A 10.129.14.5
server2  IN A 10.129.14.7
ns1      IN A 10.129.14.2
ns2      IN A 10.129.14.3
ftp      IN CNAME server1
mx       IN CNAME server1
mx2      IN CNAME server2
www      IN CNAME server2
```

### 6.2) **Zona inversa** (reverse) – PTR
Traduce IP → FQDN.
```zone
$ORIGIN 14.129.10.in-addr.arpa.
$TTL 86400
@   IN SOA dns1.domain.com. hostmaster.domain.com. (
        2001062501 21600 3600 604800 86400 )
    IN NS ns1.domain.com.
    IN NS ns2.domain.com.

5   IN PTR server1.domain.com.
```

---

## 7) Opciones críticas y superficie de ataque
| Opción (Bind) | Riesgo / uso |
|---------------|--------------|
| `allow-query` | Define quién puede consultar. |
| `allow-recursion` | Limita recursión (evita resolvers abiertos). |
| `allow-transfer` | **Controla AXFR**; restringir por IP/TSIG. |
| `zone-statistics` | Métricas de zona. |

**Buenas prácticas**
- Restringir **recursión** a redes internas.
- Bloquear **AXFR** salvo IPs de secundarios y/o usar **TSIG**.
- No exponer `version.bind` ni `ANY` con respuestas completas.
- Usar **DoT/DoH** cuando proceda y registrar auditorías.

---

## 8) Huella y enumeración
### 8.1) Puertos y servicios
- UDP/TCP **53** (DNS)
- Descubrimiento básico + NSE para RPC/NFS (contexto comparativo):
```bash
sudo nmap -sU -sT -p53 -sV 10.129.14.128
```

### 8.2) Transferencia de zona (AXFR) – *solo si está mal configurado*
```bash
dig axfr inlanefreight.htb @10.129.14.128
dig axfr internal.inlanefreight.htb @10.129.14.128
```

### 8.3) Fuerza bruta de subdominios
```bash
for sub in $(cat /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt); do
  dig +short "$sub.inlanefreight.htb" @10.129.14.128 | sed -r '/^\s*$/d'   && echo "$sub" >> subdomains.txt
done
# Alternativa con dnsenum
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0   -o subdomains.txt   -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt   inlanefreight.htb
```

---

## 9) Resumen operativo
1. **Identifica** tipo de consulta y servidor (autoridad, recursivo o caché).  
2. **Consulta** registros clave (NS, SOA, MX, A/AAAA, TXT).  
3. **Enumera** subdominios y prueba **AXFR** solo si está permitido.  
4. **Endurece** Bind9: `allow-{query,recursion,transfer}`, TSIG y mínima exposición.  
5. **Cifra** (DoT/DoH) cuando aplique y monitoriza logs.

> DNS es crítico y sensible a errores de configuración. Prioriza **seguridad** (recursión restringida, AXFR controlado, mínima información expuesta) sin perder la **consistencia** entre primario/secundario.
