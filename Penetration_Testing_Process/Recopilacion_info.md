#  Recopilación de Información en Pruebas de Penetración

## 1. Etapas del Proceso de Pentesting
1. **Pre-compromiso**
2. **Recopilación de información**
3. **Evaluación de vulnerabilidades**
4. **Explotación**
5. **Post-explotación**
6. **Movimiento lateral**
7. **Prueba de concepto (PoC)**
8. **Post-compromiso**

>  La recopilación de información es **la piedra angular** de cualquier prueba de penetración: todo ataque se basa en los datos obtenidos en esta fase.

---

## 2. Objetivo
- Obtener la **máxima información posible** sobre la organización, sus empleados, su infraestructura y servicios.  
- Cuanta más información, más fácil será identificar y explotar vulnerabilidades.  
- La información se recopila tanto desde el exterior (externo) como desde el interior (interno) de la red.

---

## 3. Categorías de Recopilación
Se deben realizar **todas las categorías** en cada pentest:

1. **OSINT (Open Source Intelligence)**  
2. **Enumeración de infraestructura**  
3. **Enumeración de servicios**  
4. **Enumeración de hosts**

---

## 4. 1 OSINT – Inteligencia de Fuentes Abiertas
- Uso de información **pública y accesible** (código abierto).  
- Objetivo: identificar **eventos**, **dependencias**, **conexiones**, credenciales filtradas, configuraciones inseguras, etc.  
- Fuentes: redes sociales, foros, ofertas de empleo, repositorios de código (GitHub, GitLab), StackOverflow, etc.
- Riesgo: es frecuente hallar **contraseñas, hashes, tokens o claves SSH** expuestas accidentalmente.

 Si se detectan datos sensibles (por ej. credenciales activas), debe activarse el protocolo de **Gestión e Informe de Incidentes (RoE)** para su notificación inmediata al cliente.

---

## 5. 2️ Enumeración de Infraestructura
- Busca un **mapa completo** de los recursos del cliente:  
  - Servidores DNS, de correo, web, instancias en la nube, rangos IP.  
- Permite conocer medidas de seguridad: firewalls, WAF, segmentación, etc.
- Se realiza con:
  - OSINT
  - Primeros escaneos activos (ej.: DNS, Nmap, etc.)
- Incluye hosts internos y externos:
  - **Externo:** visión desde fuera de la red corporativa.
  - **Interno:** visión desde dentro (útil para Password Spraying).

---

## 6. 3️ Enumeración de Servicios
- Identificación de los **servicios activos** en los hosts detectados.  
- Se analiza:
  - Versiones instaladas (para detectar vulnerabilidades conocidas).
  - Configuraciones inseguras o desactualizadas.
- Permite elegir los **vectores de ataque** más efectivos.

> ⚠️ Muchos administradores evitan actualizar versiones críticas para “no romper” sistemas, lo que deja abiertas brechas de seguridad.

---

## 7. 4️ Enumeración de Hosts
- Se analiza cada host dentro del alcance:  
  - **Sistema operativo**, **servicios activos**, **versiones**, **puertos**.  
  - Función del host y su comunicación con otros componentes.  
- Desde perspectiva **externa** e **interna**:
  - Internamente se descubren servicios no accesibles desde internet pero con frecuentes configuraciones inseguras.  
- Se recopila info útil para fases posteriores (Explotación, Escalada de Privilegios).

---

## 8.  Expoliación (Pillaging)
- Se realiza **después de explotar un host** (post-explotación).  
- Consiste en **saquear información local sensible**:  
  - Nombres de empleados, datos de clientes, archivos confidenciales, credenciales, scripts de seguridad, etc.
- Ayuda a:
  - Escalar privilegios.
  - Moverse lateralmente por la red.
- No es una fase aparte: se integra en **Recopilación de Información** y **Escalada de Privilegios**.

---

## 9.  Herramientas y Técnicas Frecuentes
- **OSINT:** buscadores avanzados, redes sociales, repositorios de código, filtraciones públicas.  
- **Infraestructura y Servicios:** Nmap, DNSenum, Shodan, Censys, escáneres de vulnerabilidades.  
- **Interno:** Password Spraying, explotación de servicios no actualizados.  

---

## 10 Puntos Clave
- La información **es el recurso principal** para un pentester.  
- Se debe documentar y comparar todo con el **alcance aprobado (RoE)**.  
- Las configuraciones erróneas internas suelen ser el mayor vector de ataque.  
- Mantener siempre los **protocolos de notificación de incidentes** ante hallazgos críticos.
