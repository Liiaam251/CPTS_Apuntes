# Nmap: Actuación y Rendimiento del Escaneo

El rendimiento del escaneo es crucial en redes grandes o con bajo ancho de banda.  
Nmap permite optimizar la **velocidad**, **tiempos de espera**, **reintentos** y **paralelismo**.

---

## 1. Parámetros Clave

- `-T <0-5>` → Plantillas de tiempo (de más lento/paranoico a más rápido/insano).  
- `--initial-rtt-timeout <time>` → Tiempo de espera inicial (RTT).  
- `--max-rtt-timeout <time>` → Tiempo de espera máximo.  
- `--max-retries <n>` → Número máximo de reintentos por puerto.  
- `--min-rate <n>` → Número mínimo de paquetes por segundo.  
- `--min-parallelism <n>` → Número mínimo de tareas simultáneas.

---

## 2. Tiempos de Espera (RTT)

El **RTT** es el tiempo que tarda en recibir respuesta un paquete enviado.

```bash
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

- `-F` → Escanea los **100 puertos más comunes**.  
- Ajustar los tiempos puede **acelerar** el escaneo, pero demasiado bajo puede omitir hosts.

---

## 3. Máximo de Reintentos

Reduce el tiempo dejando de reenviar paquetes sin respuesta:

```bash
sudo nmap 10.129.2.0/24 -F --max-retries 0
```

- Valor por defecto: `10`.  
- Con `0`, no reintenta → **más rápido**, pero puede perder puertos abiertos.

---

## 4. Velocidad de Envío (min-rate)

Permite definir cuántos paquetes enviar por segundo:

```bash
sudo nmap 10.129.2.0/24 -F --min-rate 300
```

- Acelera significativamente los escaneos.  
- Útil en **pruebas de caja blanca** con lista blanca en firewalls.

---

## 5. Plantillas de Tiempo (-T)

Controlan la agresividad del escaneo:

- `-T0` → **Paranoid** (muy lento, evasivo).
- `-T1` → **Sneaky** (lento, sigiloso).
- `-T2` → **Polite** (moderado, menos carga de red).
- `-T3` → **Normal** (valor **por defecto**).
- `-T4` → **Aggressive** (más rápido, más tráfico).
- `-T5` → **Insane** (máxima velocidad, más detectable).

Ejemplo:

```bash
sudo nmap 10.129.2.0/24 -F -T5 -oN tnet.T5
```

- `-oN tnet.T5` → Guarda resultados en formato normal.


