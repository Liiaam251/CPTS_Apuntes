# Prueba de Concepto (PoC)

## 1. Contexto en el proceso de Pentesting
1) Pre-compromiso → 2) Recopilación de información → 3) Evaluación de vulnerabilidad → 4) Explotación → 5) Post-explotación → 6) Movimiento lateral → **7) Prueba de concepto (PoC)** → 8) Post-compromiso

La **Proof of Concept (PoC)** es la etapa en la que se **demuestra de forma controlada y verificable la existencia y el impacto de las vulnerabilidades detectadas**.  
Su propósito es proporcionar evidencias que permitan a desarrolladores, administradores y responsables de seguridad **reproducir, validar y comprender el riesgo**, sentando las bases para la remediación.

## 2. Objetivos principales
- Verificar técnicamente la **explotabilidad** de las vulnerabilidades encontradas.
- **Demostrar el impacto** de los fallos de seguridad en entornos reales.
- Proporcionar una base para la **toma de decisiones** sobre mitigaciones y priorización.
- **Minimizar riesgos** al confirmar el problema antes de implementar correcciones.
- Permitir a los responsables de sistemas **reproducir el fallo y validar su resolución**.

## 3. Aplicación en seguridad informática
- Originalmente usada en desarrollo de software para validar prototipos.
- En pentesting, se emplea para:
  - Confirmar vulnerabilidades en **sistemas operativos, servicios o aplicaciones**.
  - Mostrar el posible **camino de ataque** seguido por un atacante.
  - Ejemplo común: ejecución de `calc.exe` en Windows como prueba mínima de ejecución remota de código.

## 4. Tipos de PoC
- **Documentación técnica:** Descripciones detalladas de la vulnerabilidad, vectores de ataque y pasos reproducibles.
- **Script/Code PoC:** Código que explota automáticamente la vulnerabilidad para evidenciar el fallo.
- **Cadena de ataque (Kill Chain):** Representación de cómo varios fallos se combinan para lograr un objetivo (p. ej., compromiso de dominio).

## 5. Riesgos y consideraciones
- Un **script PoC** demuestra una forma específica de explotación, pero no siempre cubre todas las variantes del fallo.
- Existe el riesgo de que el cliente se limite a **“parchear contra el script”** sin solucionar la vulnerabilidad raíz.
- Ejemplo:
  - Un administrador cambia la contraseña débil `Password123`, pero no corrige la **política de contraseñas insegura**, dejando abierta la posibilidad de futuras credenciales débiles.
- Es fundamental recalcar que la **vulnerabilidad reside en la política o configuración**, no únicamente en el vector utilizado por el PoC.

## 6. Buenas prácticas en PoC
- **Proporcionar claridad:** Entregar PoCs claros, reproducibles y seguros para pruebas internas.
- **Explicar la raíz del problema:** Acompañar la PoC con recomendaciones que ataquen la causa subyacente, no solo el síntoma.
- **Cadena de ataque documentada:** Mostrar gráficamente o paso a paso cómo se combinan las vulnerabilidades para alcanzar el objetivo.
- **Informes detallados:** El reporte debe incluir:
  - Descripción de la vulnerabilidad
  - Riesgo e impacto potencial
  - Pasos para reproducirla
  - Evidencias (capturas, logs, demostraciones controladas)
  - Recomendaciones para mitigación y estándares de seguridad
- **Revisión con el cliente:** Destacar que resolver una parte de la cadena no elimina el riesgo si persisten otros fallos.

## 7. Valor añadido para el cliente
- Permite **validar la explotación de forma segura**.
- Facilita priorizar la **remediación efectiva** de los fallos.
- Muestra que **múltiples debilidades pueden encadenarse**, ayudando a identificar y corregir las causas raíz.
- Refuerza la necesidad de políticas sólidas (p. ej., **política de contraseñas robustas**) más allá de parches aislados.

## 8. Resultado esperado
- Evidencia reproducible de cada vulnerabilidad confirmada.
- Scripts PoC entregados de forma controlada (cuando proceda).
- Informe claro que destaque:
  - Impacto, probabilidad y riesgos residuales.
  - Medidas recomendadas que aborden tanto los fallos específicos como los sistémicos.
- Base sólida para la **fase de Post-compromiso** y para el plan de remediación del cliente.
