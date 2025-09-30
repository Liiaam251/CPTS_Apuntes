# Evasión de Firewalls e IDS/IPS con Nmap

Nmap ofrece diversas técnicas para **evadir firewalls y sistemas IDS/IPS** durante los escaneos.  
Las más utilizadas incluyen **escaneos ACK**, **uso de señuelos (decoys)**, **cambio de IP de origen** y **escaneos desde puertos de confianza (como el 53 DNS)**.

---

## 1. Conceptos Clave

- **Firewall:** Filtra conexiones entrantes/salientes según reglas predefinidas.  
  - *dropped:* paquetes ignorados (sin respuesta).  
  - *rejected:* genera respuesta explícita (RST o ICMP error).

- **IDS (Intrusion Detection System):** Detecta patrones sospechosos en el tráfico.  
- **IPS (Intrusion Prevention System):** Bloquea automáticamente conexiones sospechosas detectadas por el IDS.

---

## 2. Técnicas de Evasión

### Escaneo ACK `-sA`
- Envía paquetes TCP con la bandera **ACK** en vez de **SYN**, más difícil de filtrar.
- Útil para descubrir puertos filtrados.

```bash
sudo nmap <IP> -p 21,22,25 -sA -Pn -n --disable-arp-ping
```

---

### Señuelos (Decoys) `-D`
- Inserta **IPs falsas** en los encabezados de los paquetes para ocultar el origen real.
- Ejemplo con 5 señuelos aleatorios:

```bash
sudo nmap <IP> -p 80 -sS -Pn -n -D RND:5
```

---

### Cambio de IP de Origen `-S`
- Fuerza el uso de otra IP (útil para probar reglas de firewall).

```bash
sudo nmap <IP> -p 445 -O -S <IP_falsa> -e tun0
```

---

### Escaneo desde Puerto DNS `--source-port 53`
- Aprovecha que el puerto **53 (DNS)** suele estar permitido por el firewall.

```bash
sudo nmap <IP> -p 50000 -sS -Pn -n --source-port 53
```

---

## 3. Proxy DNS y Resolución
- Nmap permite definir servidores DNS específicos con `--dns-server <ns>` para mejorar eludir controles en redes corporativas o DMZ.

---

## 4. Buenas Prácticas
- Usar **múltiples VPS** para evitar bloqueos de IP.  
- Reducir velocidad y agresividad del escaneo para no activar alertas.  
- Combinar técnicas de evasión con `--packet-trace` para depuración.

---

## 5. Ejemplo de Conexión con Netcat
Si el firewall permite el puerto 53, podemos usarlo como **puerto de origen** para conectarnos:

```bash
ncat -nv --source-port 53 <IP_objetivo> 50000
```

---

## 6. Resumen de Opciones Clave

| Opción                  | Descripción                                         |
|-------------------------|-----------------------------------------------------|
| `-sA`                   | Escaneo ACK                                         |
| `-D RND:<n>`            | Generar <n> IPs señuelo                              |
| `-S <IP>`               | Cambiar IP de origen                                |
| `--source-port <port>`  | Usar puerto de origen específico                     |
| `--disable-arp-ping`    | Deshabilitar ping ARP                               |
| `-Pn`                   | Deshabilitar ping ICMP                              |
| `--dns-server <ns>`     | Usar servidor DNS específico                        |

