# Descripción General de la Seguridad de la Información (Infosec)

## 1. Introducción
La **seguridad de la información (Infosec)** es el conjunto de prácticas, políticas y tecnologías destinadas a **proteger los datos frente a accesos no autorizados, alteraciones, uso indebido, interrupciones o destrucción**.  
El objetivo es garantizar la **Confidencialidad, Integridad y Disponibilidad (triada CIA)** de la información, ya sea electrónica, física, tangible (por ejemplo, planos de diseño) o intangible (conocimiento).

El campo de Infosec ha evolucionado de forma significativa en los últimos años y se compone de múltiples especializaciones.

## 2. Principales áreas de especialización
- **Seguridad de redes e infraestructuras:** protección de routers, switches, firewalls, VPNs y entornos cloud.
- **Seguridad de aplicaciones:** auditoría y protección del ciclo de vida de software (SDLC seguro).
- **Pruebas de seguridad:** pentesting, red teaming y evaluaciones de resistencia.
- **Auditoría de sistemas:** revisión de cumplimiento normativo, configuración y políticas.
- **Planificación de la continuidad del negocio (BCP):** estrategias para recuperación ante desastres y alta disponibilidad.
- **Análisis forense digital:** investigación post-incidente y preservación de evidencias.
- **Detección y respuesta a incidentes (IR):** monitorización activa, contención y erradicación de amenazas.

## 3. Proceso de gestión de riesgos (Risk Management)
El objetivo de la gestión de riesgos en Infosec es implementar **controles eficaces sin afectar negativamente a la operativa y productividad del negocio**.  
El proceso consta de cinco fases principales:

| Paso                 | Descripción                                                                                           |
|----------------------|-------------------------------------------------------------------------------------------------------|
| **Identifying the Risk** | Identificar riesgos legales, regulatorios, ambientales, técnicos, de mercado, etc.                    |
| **Analyze the Risk**     | Analizar el impacto y la probabilidad, correlacionando los riesgos con procesos y políticas existentes. |
| **Evaluate the Risk**    | Evaluar, clasificar y priorizar los riesgos para decidir si **aceptar**, **evitar**, **mitigar** o **transferir**. |
| **Dealing with Risk**    | Implementar medidas para eliminar o contener el riesgo, coordinándose con los responsables de cada área. |
| **Monitoring Risk**      | Monitorear constantemente los riesgos para detectar cambios que puedan modificar su nivel de impacto. |

El principio fundamental sigue siendo **mantener la triada CIA**, independientemente del origen del incidente (fallo técnico, desastre natural, ataque malicioso, etc.).

## 4. Equipos de seguridad: Red Team vs. Blue Team
En ciberseguridad se utilizan los términos:

- **Red Team (Equipo Rojo):** adopta el rol del atacante. Simula amenazas reales para identificar debilidades explotables. Actividades frecuentes:
  - Pruebas de penetración internas y externas.
  - Ingeniería social.
  - Simulación de ataques dirigidos (APT).
- **Blue Team (Equipo Azul):** se enfoca en la defensa:
  - Monitoriza, detecta y responde a incidentes.
  - Fortalece políticas y configuraciones de seguridad.
  - Implementa controles técnicos y procedimientos preventivos.

Ambos roles son complementarios: el Red Team descubre las brechas y el Blue Team trabaja para mitigarlas.

## 5. Rol de los evaluadores de penetración
Los **penetration testers (pentesters)** ayudan a las organizaciones a **identificar y demostrar riesgos en redes, aplicaciones y configuraciones**, incluyendo:
- Vulnerabilidades técnicas.
- Exposición de datos confidenciales.
- Configuraciones incorrectas.
- Riesgos que afectan a la reputación o continuidad del negocio.

Un buen pentester debe:
- Comprender el **entorno empresarial y regulatorio** del cliente.
- Proporcionar **evidencias reproducibles** de los hallazgos.
- Sugerir medidas de **mitigación o remediación** de forma comprensible y priorizada.

## 6. Tipos de evaluaciones
- **Pentesting de caja blanca, gris o negra:** con diferentes niveles de conocimiento previo del entorno.
- **Evaluaciones de phishing y concienciación:** miden el riesgo humano.
- **Evaluaciones de Red Team:** simulan actores de amenaza reales con objetivos definidos.
- **Pruebas de resistencia de aplicaciones web y APIs.**

## 7. Importancia de la visión de riesgo
Una evaluación efectiva exige comprender:
- El **contexto de negocio y los activos críticos**.
- El **panorama de amenazas** y vulnerabilidades existentes.
- Las prioridades de mitigación según **impacto y probabilidad**.

El conocimiento profundo del **proceso de gestión de riesgos** es esencial para evaluar adecuadamente los hallazgos técnicos y traducirlos en recomendaciones alineadas con los objetivos de la organización.

## 8. Enfoque práctico del aprendizaje
Para iniciarse en Infosec y Pentesting:
- Practicar con **distribuciones especializadas de pentesting** (por ejemplo, Kali Linux, Parrot OS).
- Familiarizarse con **tecnologías de red, servicios y protocolos comunes**.
- Aprender el **ciclo de vida de las pruebas de penetración** (reconocimiento, explotación, post-explotación, etc.).
- Usar **plataformas de entrenamiento** (como Hack The Box, TryHackMe) y laboratorios controlados.
- Desarrollar habilidades en **documentación, reporting y comunicación** además de la parte técnica.

