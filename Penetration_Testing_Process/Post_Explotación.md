# Post-explotación (Post-Exploitation)

## 1. Contexto en el proceso de Pentesting
1) Pre-compromiso → 2) Recopilación de información → 3) Evaluación de vulnerabilidad → 4) Explotación → **5) Post-explotación** → 6) Movimiento lateral → 7) Prueba de concepto (PoC) → 8) Post-compromiso

La post-explotación comienza una vez que se ha obtenido acceso al sistema objetivo.  
Su propósito es **recopilar información sensible desde una perspectiva interna, elevar privilegios y mantener el acceso** para alcanzar los objetivos definidos en el alcance de la prueba.

Componentes principales:
- Pruebas evasivas
- Recopilación de información local
- Expoliación (Pillaging)
- Evaluación de vulnerabilidades desde dentro
- Escalada de privilegios
- Persistencia
- Exfiltración de datos

## 2. Pruebas evasivas
- Después de la explotación, es más difícil permanecer indetectable; cada comando puede activar alertas.
- Un administrador o un sistema EDR/EDP puede detectar la actividad y:
  - Cuarentenar el host
  - Bloquear la cuenta comprometida
  - Forzar un cambio de contraseña
- Aunque la detección puede considerarse un “fallo”, también evidencia que los controles defensivos funcionan.
- Las pruebas evasivas permiten identificar **puntos ciegos** en la monitorización del cliente.

Modos:
- **Evasive:** se prioriza pasar desapercibido.
- **Hybrid-evasive:** se combinan acciones evasivas con ataques visibles para evaluar defensas específicas.
- **Non-evasive:** pruebas directas y visibles, generalmente cuando el cliente busca la cobertura más amplia posible.

## 3. Recopilación de información local
- Tras el acceso inicial se obtiene una **nueva perspectiva interna** de la red y sistemas.
- Se repiten las fases de **Information Gathering** y **Vulnerability Assessment**, pero ahora desde el interior.
- Se enumeran recursos locales:
  - Impresoras, bases de datos, servicios de virtualización, shares corporativos, etc.
- Esta exploración interna suele descubrir configuraciones inseguras y datos útiles para etapas posteriores.

## 4. Expoliación (Pillaging)
- Busca comprender el **rol del host** dentro de la red corporativa:
  - Interfaces de red, rutas, DNS, ARP, VPN, subredes, tráfico, credenciales reutilizables.
- Permite mapear relaciones con otros sistemas, posibles subdominios y dependencias.
- Se buscan datos sensibles:
  - Contraseñas en archivos de configuración, scripts, vaults, documentos (Excel, Word, txt) y correos electrónicos.
- Objetivos:
  - Evidenciar el impacto de la explotación
  - Encontrar credenciales que habiliten escalada de privilegios o movimiento lateral

## 5. Persistencia
- Se establece un **mecanismo para mantener el acceso** al host comprometido en caso de que la sesión inicial se pierda.
- Es recomendable configurarla **antes de realizar más acciones** que puedan inestabilizar el sistema.
- Cada host y sistema operativo requiere métodos de persistencia específicos; el enfoque debe adaptarse al entorno.

## 6. Evaluación de vulnerabilidades desde el interior
- Con la visión local se repite la **Vulnerability Assessment**.
- Se identifican servicios y configuraciones internas no visibles desde el exterior.
- Se priorizan nuevas debilidades para preparar la fase de escalada de privilegios.

## 7. Escalada de privilegios
- Objetivo: obtener los máximos privilegios disponibles:
  - Linux: usuario root
  - Windows: administrador local, administrador de dominio, SYSTEM
- La escalada también puede lograrse usando credenciales capturadas de otros usuarios con privilegios superiores.
- Es una fase crítica que permite ampliar el alcance dentro de la red.

## 8. Exfiltración de datos
- Consiste en transferir datos fuera de la red del cliente de forma controlada y con autorización previa.
- Se prueban los mecanismos de seguridad contra fuga de datos:
  - DLP (Data Loss Prevention)
  - EDR (Endpoint Detection and Response)
  - Monitoreo de red y cifrado de discos
- En muchos casos se utiliza **información falsa o de prueba** (por ejemplo, números ficticios de tarjetas) para validar los controles sin comprometer datos reales.
- La exfiltración debe ajustarse a las **regulaciones de protección de datos** aplicables:
  - PCI-DSS (tarjetas de pago)
  - HIPAA (datos médicos)
  - GLBA (banca)
  - FISMA, GDPR, ISO/IEC 27001, NIST, entre otros
- Se debe grabar pantalla y capturas que evidencien:
  - Host, usuario, ruta y prueba de extracción
- El cliente debe ser notificado de inmediato ante hallazgos críticos.

## 9. Buenas prácticas
- Registrar todas las actividades de post-explotación para informes y trazabilidad.
- Consultar con el cliente antes de acciones que puedan poner en riesgo la continuidad del servicio.
- Adaptar las técnicas al entorno concreto y minimizar el impacto operacional.
- Mantener copias de seguridad y planes de reversión ante errores.
- Respetar las Reglas de Compromiso (RoE) y regulaciones de datos.

## 10. Salida de la fase
Resultados esperados:
- Inventario de activos descubiertos internamente
- Mapa de la red y relaciones entre sistemas
- Listado de credenciales y datos sensibles encontrados
- Persistencia establecida y documentada
- Evidencias de escalada de privilegios y exfiltración (si fue autorizada)
- Insumos para la fase de **Movimiento lateral**

