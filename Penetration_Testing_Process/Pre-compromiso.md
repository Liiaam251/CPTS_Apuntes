#  Pre-compromiso en Pruebas de Penetración

## 1. Etapas del Proceso de Pentesting
1. **Pre-compromiso**
2. **Recopilación de información**
3. **Evaluación de vulnerabilidades**
4. **Explotación**
5. **Post-explotación**
6. **Movimiento lateral**
7. **Prueba de concepto (PoC)**
8. **Post-compromiso**

---

## 2. Objetivo del Pre-compromiso
- Definir **alcance**, **objetivos** y **condiciones legales** antes de iniciar las pruebas.
- Garantizar que todas las partes estén de acuerdo y alineadas.
- Evitar problemas legales (cumplimiento de la Ley de Uso Indebido de Computadoras).

---

## 3. Componentes del Pre-compromiso
1. **Cuestionario de alcance (Scoping Questionnaire)**  
   - Se envía tras el primer contacto.
   - Identifica necesidades del cliente, tipo de prueba y recursos requeridos.
2. **Reunión previa al compromiso (Pre-Engagement Meeting)**  
   - Discusión detallada de objetivos, riesgos, metodologías y recursos.
   - Base para el **Penetration Testing Proposal / Scope of Work (SoW)**.
3. **Reunión de lanzamiento (Kick-off Meeting)**  
   - Se celebra tras firmar contratos.
   - Participan el equipo del cliente (Auditoría, TI, Seguridad, etc.) y el equipo de pentesting.
   - Se explica el proceso, riesgos y protocolos de notificación.

---

## 4. Acuerdo de Confidencialidad (NDA)
**Requisito previo** para compartir cualquier información sensible.  
Tipos:
-  **Unilateral NDA:** Solo una parte guarda la confidencialidad.  
-  **Bilateral NDA:** Ambas partes protegen la información (más habitual).  
-  **Multilateral NDA:** Entre más de dos partes (p. ej., redes cooperativas).

---

## 5. Autoridad para Contratar
Solo personal autorizado puede solicitar la prueba:
- CEO, CTO, CISO, CSO, CIO, CRO  
- VP / Director de Auditoría, TI o Seguridad  
- Gerente de Auditoría, etc.  

También se designan **puntos de contacto principales y secundarios**, soporte técnico y contacto de emergencias.

---

## 6. Documentación Necesaria
| Documento | Momento de Creación |
|-----------|---------------------|
| 1. NDA | Tras contacto inicial |
| 2. Scoping Questionnaire | Antes de la reunión previa |
| 3. Scoping Document | Durante la reunión previa |
| 4. Penetration Testing Proposal / SoW | Durante la reunión previa |
| 5. Rules of Engagement (RoE) | Antes de la reunión de lanzamiento |
| 6. Contractors Agreement (si pruebas físicas) | Antes de la reunión de lanzamiento |
| 7. Reports | Durante y después de las pruebas |

>  Todos los documentos deben revisarse legalmente.

---

## 7. Cuestionario de Alcance (Ejemplos)
- Tipo de evaluación:  
  - Vulnerabilidad interna/externa  
  - Pentest interno/externo  
  - Seguridad inalámbrica, aplicaciones web/móviles  
  - Ingeniería social (phishing, vishing), Red Team, etc.
- Detalles a recopilar: Nº de IP/rangos, dominios, SSID, roles de usuario, ubicaciones físicas, objetivos de phishing, etc.
- Definir tipo de prueba: **Caja negra, gris o blanca.**
- Nivel de evasión: no invasivo, híbrido-evasivo, totalmente evasivo.

---

## 8. Lista de Verificación del Contrato
- ✅ NDA  
- ✅ Objetivos (Goals)  
- ✅ Alcance (Scope)  
- ✅ Tipo y localización de pruebas  
- ✅ Metodologías (OSSTMM, OWASP, PTES…)  
- ✅ Estimación de tiempo y horarios  
- ✅ Riesgos y limitaciones de alcance  
- ✅ Manejo de información (HIPAA, PCI, etc.)  
- ✅ Terceros involucrados (nube, ISP, etc.)  
- ✅ Líneas de comunicación y contactos  
- ✅ Reporting y términos de pago  

---

## 9. Reglas de Compromiso (RoE)
Incluyen:
- Introducción, propósito, objetivos
- Alcance técnico (IP, dominios, URLs)
- Metodología y horarios de prueba
- Manejo de evidencias y backups
- Manejo de incidentes, reporting y retesting
- Limitaciones de responsabilidad
- **Permiso para probar** (firma obligatoria)

---

## 10 Reunión de Lanzamiento (Kick-off)
- Confirma detalles finales de la prueba.
- Incluye personal técnico, gerencial y de soporte.
- Se acuerdan protocolos de notificación para vulnerabilidades críticas (ej. RCE, SQLi).
- Se advierte sobre posibles impactos (logs, alarmas, bloqueos accidentales).

---

## 11. Acuerdo de Contratistas (Pruebas Físicas)
Requerido si hay intrusión física.  
Contiene:
- Datos de ubicaciones, salas, pisos, componentes físicos
- Cronograma y permisos notarizados
- Funciona como “**Get Out of Jail Free Card**” en caso de intervención policial.

---

## 12. Configuración
Una vez completado el pre-compromiso:
- Preparar entornos: máquinas virtuales, VPS, herramientas y sistemas.
- Ajustar todo según el alcance acordado.
