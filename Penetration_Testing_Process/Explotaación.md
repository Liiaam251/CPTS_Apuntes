# Explotación (Exploitation)

## 1. Contexto en el proceso de Pentesting
1) Pre-compromiso → 2) Recopilación de información → 3) Evaluación de vulnerabilidad → **4) Explotación** → 5) Post-explotación → 6) Movimiento lateral → 7) Prueba de concepto (PoC) → 8) Post-compromiso

La fase de explotación busca **aprovechar las vulnerabilidades identificadas** para obtener el objetivo deseado (punto de apoyo, escalada de privilegios, acceso persistente, etc.).  
Incluye la **adaptación de exploits o PoC** al contexto del objetivo.

## 2. Objetivo
- Transformar debilidades en acceso controlado.  
- Modificar PoC para ejecutar código que permita obtener acceso (por ejemplo, un shell inverso) con conexión cifrada hacia el host del atacante.  
- Garantizar que la explotación sea **segura y controlada**, minimizando el riesgo de dañar el sistema objetivo.

## 3. Relación con otras fases
- Aunque las fases están definidas, **se solapan y se retroalimentan**.  
- Es importante llevar registro de los pasos realizados, especialmente en pruebas largas con múltiples sistemas.

## 4. Priorización de ataques
Se debe decidir qué ataque ejecutar primero basándose en:

1. **Probabilidad de éxito:** estimar la viabilidad técnica (por ejemplo, usando puntuación CVSS y calculadora NVD).  
2. **Complejidad:** tiempo, esfuerzo y experiencia necesarios.  
   - Fácil, media o difícil según la investigación requerida para adaptar el exploit.  
3. **Probabilidad de daño:** riesgo de interrupciones o inestabilidad del sistema.  
   - Se evita realizar ataques DoS salvo solicitud explícita del cliente.  

### Ejemplo de priorización con sistema de puntos

| Factor                          | Inclusión remota de archivos | Desbordamiento de búfer |
|----------------------------------|------------------------------|-------------------------|
| 1. Probabilidad de éxito         | 10                           | 8                       |
| 2. Complejidad - Fácil           | 5                            | 0                       |
| 3. Complejidad - Media           | 3                            | 3                       |
| 4. Complejidad - Difícil         | 1                            | 0                       |
| 5. Probabilidad de daño          | 0                            | -5                      |
| **Total**                        | **14**                        | **6**                    |

En este ejemplo se prioriza el ataque de **Inclusión remota de archivos** por su mayor puntuación y menor riesgo.

## 5. Preparación de exploits
- Si el PoC disponible no es fiable o completo, **replicar el entorno objetivo** en una máquina virtual local con la misma versión de sistemas y servicios.  
- Ajustar, compilar y probar el exploit en este entorno controlado antes de usarlo contra el objetivo real.  
- Confirmar que el exploit no produce efectos colaterales significativos.  
- En casos de configuraciones o vulnerabilidades conocidas, usar herramientas fiables que ya se consideren estables y seguras.

## 6. Comunicación y validación con el cliente
- Ante dudas sobre el riesgo de un ataque, **consultar siempre con el cliente** antes de ejecutarlo.  
- Proporcionar detalles técnicos y posibles impactos para que el cliente tome una decisión informada.  
- Si el cliente decide no autorizar la explotación, documentar el hallazgo como **no confirmado activamente** pero marcarlo como problema a corregir.  
- La transparencia y la trazabilidad son esenciales para evitar interrupciones imprevistas.

## 7. Buenas prácticas
- Mantener un **registro detallado de todas las acciones** realizadas (para informes y trazabilidad).  
- Priorizar la **seguridad y estabilidad** de los sistemas del cliente.  
- Utilizar conexiones cifradas para shells inversos y evitar exposición innecesaria.  
- Evitar ataques destructivos o que provoquen denegación de servicio.  
- Verificar exploits en entornos controlados antes de lanzarlos en producción.  

## 8. Transición a las siguientes fases
Una vez logrado el acceso inicial:
- Confirmar y documentar la explotación exitosa.  
- Pasar a **Post-explotación** (para recopilar más información y elevar privilegios).  
- Continuar con **Movimiento lateral** si el objetivo lo requiere.  
