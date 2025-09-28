# Evaluación de Vulnerabilidad (Vulnerability Assessment)

## 1. Contexto en el proceso de Pentesting
1) Pre-compromiso → 2) Recopilación de información → 3) Evaluación de vulnerabilidad → 4) Explotación → 5) Post-explotación → 6) Movimiento lateral → 7) Prueba de concepto (PoC) → 8) Post-compromiso

La evaluación de vulnerabilidades es un proceso analítico que utiliza la información recopilada para identificar debilidades, priorizarlas y preparar su explotación de forma controlada.

## 2. Definición de análisis
Examen detallado de hallazgos para entender origen, impacto y acciones que permitan provocar o prevenir eventos futuros.

Tipos de análisis aplicables:
- **Descriptivo:** describe datos, versiones, puertos y ayuda a detectar errores y valores atípicos.
- **Diagnóstico:** identifica causas, efectos e interdependencias de los hallazgos.
- **Predictivo:** proyecta la probabilidad de eventos futuros a partir de datos históricos y actuales.
- **Prescriptivo:** define acciones para mitigar, prevenir o explotar un hallazgo de forma segura.

## 3. Flujo práctico de la evaluación
1. Normalizar los hallazgos de OSINT, infraestructura, servicios y hosts.
2. Enriquecer cada elemento con versión, configuración, exposición y dependencias.
3. Investigar CVEs y exploits disponibles.
4. Modelar riesgos (probabilidad × impacto) y priorizar.
5. Elaborar un plan de pruebas, detallando metodología, nivel de evasión y criterios de éxito.
6. Preparar PoC (adaptada al entorno específico).
7. Volver a la fase de recopilación si faltan datos o hipótesis explotables.

## 4. Ejemplo práctico
Hallazgo: puerto TCP/2121 abierto sin banner.

- ¿Es un puerto estándar? No (los puertos estándar van de 0 a 1023).
- ¿Puede estar relacionado con FTP (21/tcp)? Es posible que se utilice un puerto alternativo.
- Acción: probar con netcat o un cliente FTP. La conexión tarda 15 segundos, lo que indica que podría ser necesario ajustar parámetros de Nmap (`--min-rtt-timeout 15s`) para un escaneo más preciso.
- Conclusión: confirmar la existencia de un servicio FTP y continuar el análisis de versión, autenticación y CVEs asociados.

## 5. Investigación de vulnerabilidades
Basada en los datos de la fase de recopilación:

Fuentes recomendadas:
- CVE/NVD (NIST)
- Bases de datos de exploits
- Avisos de fabricantes y CERT
- Listas de correo y changelogs
- Documentación de configuraciones inseguras frecuentes

Es fundamental entender el código PoC para adaptarlo a las condiciones del entorno objetivo (rutas, roles, configuraciones específicas).

## 6. Evaluación de vectores de ataque
- Combinar datos históricos (vulnerabilidades conocidas) con el estado actual (exposición real).
- Ajustarse al nivel de evasión definido en las Reglas de Compromiso (RoE).  
- Si se requiere sigilo, replicar el servicio o sistema objetivo localmente para validar PoC sin generar alertas.
- Priorizar vectores según facilidad de explotación, impacto y riesgo de detección.

## 7. Priorización de riesgos
Criterios generales:

- **Crítico:** RCE no autenticada, SQLi con exfiltración, bypass de autenticación.
- **Alto:** escalada local de privilegios, exposición de credenciales, deserialización insegura.
- **Medio:** versiones obsoletas con mitigaciones, fugas de información útil para encadenar ataques.
- **Bajo:** configuraciones menores o hallazgos informativos sin impacto directo.

Factores adicionales: complejidad de explotación, necesidad de interacción, alcance (interno o externo), activos afectados y cumplimiento normativo.

## 8. Iteración cuando no se encuentran vulnerabilidades
- Volver a la recopilación para obtener datos más precisos sobre versiones, parches, configuraciones o vectores alternativos (subdominios, VPN, redes internas).
- Tener en cuenta que, a diferencia de los CTF, en entornos reales importa la cobertura y calidad de los hallazgos más que la velocidad.

## 9. Evidencia mínima por hallazgo
Cada hallazgo debe documentarse con:
- Descripción del servicio, versión o configuración detectada.
- Método utilizado para su identificación (comando o traza reproducible).
- Relevancia y referencia a CVE u otra debilidad.
- Prueba de concepto controlada (en entorno espejo o durante ventana acordada).
- Medidas de mitigación recomendadas.
- Riesgo residual si no se aplica corrección inmediata.

## 10. Buenas prácticas
- Ajustar tiempos de espera y técnicas para reducir falsos negativos.
- No asumir que la falta de banner implica ausencia de riesgo; usar scripts y validaciones adicionales.
- Documentar supuestos y dependencias entre hallazgos.
- Cumplir los límites definidos en el alcance (RoE) y coordinar ventanas de prueba con el cliente.
- Disponer de un plan de reversión en caso de interrupciones del servicio.

## 11. Resultados esperados
- Matriz de riesgos priorizada.
- Mapa de vectores de ataque con pre-requisitos.
- Pruebas de concepto validadas y reproducibles.
- Plan de explotación organizado por impacto y restricciones del negocio.
- Recomendaciones prescriptivas para correcciones tempranas.
