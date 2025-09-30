# Descubrimiento de Host con Nmap

El **descubrimiento de host** permite identificar los sistemas **activos** en una red antes de realizar un escaneo de puertos.  
Es una fase esencial en pruebas de penetración internas para **mapear la red** y priorizar los objetivos.

---

## 1. Opciones Clave de Nmap

- `-sn` → Desactiva el escaneo de puertos y solo descubre hosts activos.
- `-oA <nombre>` → Guarda los resultados en **todos los formatos** (normal, XML, grepable).
- `-iL <archivo>` → Usa una **lista de IPs** como entrada.
- `-PE` → Usa **ICMP Echo Requests** para descubrir hosts.
- `--packet-trace` → Muestra los **paquetes enviados y recibidos**.
- `--reason` → Indica el **motivo por el que un host fue marcado como activo**.
- `--disable-arp-ping` → Deshabilita el ping ARP (usa solo ICMP).

---

## 2. Escanear un Rango de Red

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet
```

- `10.129.2.0/24` → Rango de red a escanear.
- Ideal para redes internas para listar todos los hosts activos.

---

## 3. Escanear Usando una Lista de IPs

Crear un archivo con las IPs:

```bash
cat hosts.lst
10.129.2.4
10.129.2.10
10.129.2.18
```

Ejecutar el escaneo:

```bash
sudo nmap -sn -oA tnet -iL hosts.lst
```

---

## 4. Escanear Varias o Rango de IPs Específicas

```bash
# Varias IPs
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20

# Rango consecutivo
sudo nmap -sn -oA tnet 10.129.2.18-20
```

---

## 5. Escanear una Sola IP

```bash
sudo nmap 10.129.2.18 -sn -oA host
```

---

## 6. Forzar ICMP Echo Request y Ver Paquetes

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace
```

- `-PE` → Usa **solicitudes ICMP Echo**.
- `--packet-trace` → Muestra los paquetes enviados/recibidos.

---

## 7. Mostrar Razón del Descubrimiento

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --reason
```

- Útil para entender si el host fue detectado por **ARP** o **ICMP**.

---

## 8. Deshabilitar ARP Ping

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping
```

- Evita el ping ARP y usa solo **ICMP Echo Requests**.

---

## 📝 Resumen

- Usa `-sn` para **solo descubrir hosts activos**.
- Guarda siempre los resultados con `-oA` para **documentar y comparar escaneos**.
- Combina opciones como `-PE`, `--reason` y `--packet-trace` para **diagnóstico avanzado**.
- El descubrimiento de host es el **primer paso para un mapeo de red efectivo**.
