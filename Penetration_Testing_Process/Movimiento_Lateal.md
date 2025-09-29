# Movimiento Lateral (Lateral Movement)

## 1. Contexto en el proceso de Pentesting
1) Pre-compromiso → 2) Recopilación de información → 3) Evaluación de vulnerabilidad → 4) Explotación → 5) Post-explotación → **6) Movimiento lateral** → 7) Prueba de concepto (PoC) → 8) Post-compromiso

El movimiento lateral busca **determinar hasta dónde puede avanzar un atacante dentro de la red interna** tras comprometer un primer sistema.  
Su objetivo es simular escenarios de ataque reales (por ejemplo, propagación de ransomware) que podrían afectar a toda la organización.

Esta fase evalúa:
- La facilidad de desplazamiento a través de la red interna
- La reutilización de credenciales y permisos
- La efectividad de los controles defensivos (segmentación, detección y respuesta)

## 2. Objetivos principales
- Analizar rutas de acceso entre sistemas internos
- Evaluar vulnerabilidades y configuraciones inseguras en la red interna
- Determinar el impacto potencial de un ataque propagado (p. ej., ransomware)
- Poner a prueba la respuesta de los sistemas defensivos (IPS, IDS, EDR, segmentación)

## 3. Fases dentro del movimiento lateral
1. **Pivoteo (Pivoting / Tunneling)**
2. Pruebas evasivas
3. Recopilación de información interna
4. Evaluación de vulnerabilidades
5. Explotación (de privilegios)
6. Post-explotación en nuevos sistemas comprometidos

## 4. Pivoteo
- Permite utilizar el sistema comprometido como **puente o proxy** para acceder a redes internas no enrutables desde el exterior.
- El tráfico generado por el pentester se envía a través del host comprometido.
- Ejemplo conceptual:
  - Una impresora interna inaccesible desde Internet puede ser alcanzada si un host interno ya comprometido actúa como intermediario.
- Objetivo: ampliar la visibilidad y el alcance del análisis a segmentos internos de la red.

## 5. Pruebas evasivas
- Evalúan la **capacidad de las defensas internas** para detectar actividad sospechosa durante el desplazamiento lateral.
- Permiten ajustar el comportamiento del ataque para:
  - Pasar desapercibido (Evasive)
  - Simular ataques visibles (Non-evasive)
  - Combinar ambos métodos según los requisitos del cliente (Hybrid-evasive)
- Requiere comprender los controles de seguridad presentes:
  - Segmentación de red (micro-segmentation)
  - Sistemas de prevención/detección de intrusiones (IPS/IDS)
  - EDR (Endpoint Detection and Response)

## 6. Recopilación de información interna
- Se repite la fase de **Information Gathering**, pero ahora desde la **nueva posición interna** alcanzada.
- Objetivo:
  - Enumerar nuevos hosts y servidores
  - Identificar servicios expuestos solo en la red interna
- Se obtiene una visión más amplia que la lograda en la post-explotación inicial.

## 7. Evaluación de vulnerabilidades interna
- En redes internas suelen existir **más errores de configuración** que en los sistemas expuestos a Internet.
- Es crucial identificar:
  - Grupos de usuarios y asignaciones de permisos
  - Recursos compartidos y dependencias internas
- Ejemplo:
  - Una cuenta comprometida de un desarrollador puede proporcionar acceso a repositorios de código, servidores internos o documentación sensible.

## 8. Explotación (de privilegios)
- Uso de vulnerabilidades, configuraciones inseguras o credenciales capturadas para **comprometer otros sistemas**.
- Técnicas frecuentes:
  - Descifrado de contraseñas y hashes
  - Reutilización de credenciales capturadas (p. ej., pass-the-hash)
  - Uso de herramientas como **Responder** para interceptar hashes NTLMv2
- Permite expandir el acceso y avanzar a hosts críticos dentro de la red.

## 9. Post-explotación en nuevos sistemas
- Cada nuevo host comprometido debe someterse nuevamente a la fase de **Post-Exploitation**:
  - Recopilación de datos locales
  - Identificación de credenciales y configuraciones
  - Evaluación de información sensible
- Toda acción debe cumplir las **Reglas de Compromiso (RoE)** establecidas con el cliente.

## 10. Buenas prácticas
- Mantener registros detallados de cada pivote, host comprometido y ruta utilizada.
- Coordinar acciones con el cliente, especialmente si se detectan datos sensibles o actividad crítica.
- Respetar los límites del alcance y minimizar el riesgo operacional.
- Validar la efectividad de las defensas internas y reportar hallazgos para la mejora continua de la seguridad.

## 11. Resultado esperado
- Mapa de la red interna alcanzada mediante pivoteo
- Listado de vulnerabilidades internas y configuraciones inseguras
- Evidencias de escalada y rutas de propagación
- Información relevante para la fase de **Prueba de concepto (PoC)** y para la remediación por parte del cliente
