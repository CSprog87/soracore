# WP06 · Datos, seguridad, privacidad y cumplimiento

| Campo | Valor |
|---|---|
| Oleada | 1 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, en especial 14, 15 y 16 |
| Insumos de otros WPs | Ninguno (oleada 1) |
| Archivo de salida | `03-entregables/WP06-seguridad-y-cumplimiento.md` |
| Acceso web | Recomendado: textos oficiales de las leyes y las autoridades por país, documentación de seguridad de AWS y Bedrock, OWASP |
| Extensión orientativa | 7,000–10,000 palabras más tablas |
| Dueño de | Matriz de cumplimiento por país; análisis de residencia y transferencias (insumo de `[OD-06]`); óptica regulatoria de `[OD-11]`; threat model; preguntas legales |

## Rol
Eres arquitecto de seguridad cloud y especialista en privacidad y cumplimiento para fintech en LatAm, con conocimiento de seguridad de aplicaciones con LLMs.

## Objetivo
Diseñar el **modelo de seguridad, privacidad y cumplimiento** de la plataforma: qué datos se tratan, dónde y bajo qué controles; cómo se protege cada capa (incluidos los riesgos propios de agentes con LLM); qué exige cada país núcleo, y qué preguntas deben responder los abogados **antes** del piloto.

> **Regla:** ningún punto legal se da por cumplido. Todo lleva `[LEGAL]`. Tu trabajo es dejar el mapa completo y las preguntas precisas, con fuentes oficiales.

## Secciones obligatorias

### 2.1 Inventario y clasificación de datos
| Dato | Ejemplo | Clasificación (público, interno, confidencial, personal, personal sensible, financiero) | Origen | Dónde se almacena (C-xx) | Retención por país | Base de tratamiento `[LEGAL]` | Quién accede |
Incluye datos de conversación, metadatos, grabaciones, datos de cobranza, contenido de marketing, credenciales del tenant, logs y trazas, y, para la fase 2, biométricos de KYC.

### 2.2 Flujos de datos y fronteras de confianza
Diagrama con las fronteras: usuario, Meta, telefonía, Sora (celda y plano de control), Bedrock (incluida la inferencia cross-region), tenant, Sora Core, PSP. Señala dónde se cruza una **frontera de país**.

### 2.3 Residencia y transferencias internacionales (insumo de `[OD-06]`)
- Por país núcleo: requisitos de residencia (si los hay), mecanismos de transferencia internacional (consentimiento, cláusulas, adecuación, aviso de privacidad), y requisitos adicionales si el tenant es una entidad supervisada (nube y outsourcing).
- Impacto de cada opción de celda (`sa-east-1`, `mx-central-1`, `us-east-1`) y de la **inferencia cross-region** sobre cada país.
- Recomendación desde el cumplimiento, para que WP03 y el equipo la integren.

### 2.4 Roles legales y contratos
Responsable o controlador (tenant), encargado o procesador (Sora), subencargados (AWS, Meta, telefonía y otros). Cláusulas mínimas del DPA. Lista de subencargados y su ubicación. Cómo se comunica al usuario final (aviso de privacidad del tenant: qué debe decir sobre IA, Sora y las transferencias).

### 2.5 Controles de seguridad por capa
Identidad (IAM Identity Center, Cognito con MFA para la consola, sin credenciales de larga duración), autorización (RBAC en la consola, ABAC por tenant en AWS), cifrado (KMS por tenant, TLS 1.2+, cifrado de backups), secretos (rotación), red (endpoints privados, egress controlado, WAF y Shield en la consola y en el web chat), logging (CloudTrail de la organización, log archive inmutable), detección (GuardDuty, Security Hub, Macie sobre S3, Inspector), gestión de vulnerabilidades, SDLC seguro (SAST, DAST, SCA, revisión de IaC) y backups. Mapea cada control a su C-xx.

### 2.6 Seguridad específica de LLMs y agentes
- Mapea el **OWASP Top 10 para aplicaciones LLM** (versión vigente) y las amenazas de **IA agéntica** de OWASP `[VERIFICAR versión]` a mitigaciones concretas y a componentes.
- Autenticación del usuario final (L0/L1/L2) y prevención de **account takeover** por SIM swap, números reciclados e ingeniería social conversacional.
- **Redacción de PII** en logs, trazas y datasets de evals; uso de datos reales en evals (anonimización).
- Aislamiento entre tenants en memoria, KB, caché de prompts y trazas.
- Revisión humana de las acciones W-alto según la política.
- Configuración de Bedrock en cuanto a uso de datos y retención del proveedor `[VERIFICAR]`.

### 2.7 Threat model (STRIDE)
Tabla STRIDE para al menos **10 componentes críticos**: C-01, C-06, C-07, C-08, C-10, C-13, C-14, C-19, C-23 y C-25. Cada fila con amenaza, escenario, mitigación y riesgo residual.

### 2.8 Retención, borrado y derechos de los titulares
- Política de retención por tipo de dato y **por país**, e implementación (TTL de DynamoDB, lifecycle de S3, Object Lock solo para la auditoría).
- **Workflow W-08** (opt-out y derechos de los titulares) con las variantes por país (plazos de respuesta `[LEGAL]`), en un diagrama.
- Borrado y devolución de datos al terminar el contrato con el tenant.

### 2.9 Bitácora de auditoría
Eventos auditables (cada acción de agente, cada cambio de configuración o prompt, cada acceso a datos desde la consola), esquema, inmutabilidad, retención y **acceso del tenant a su auditoría** (exportación).

### 2.10 Matriz de cumplimiento por país
Para cada país núcleo, y por dimensión D1–D10 de `16 §1`:
| País | Dimensión | Norma o fuente (con enlace oficial) | Requisito concreto | Control o diseño que lo cubre (C-xx o regla de C-13) | Evidencia | Responsable (Sora o tenant) | Estado `[LEGAL]` |
Debe incluir explícitamente las **reglas de cobranza por país** (ventanas horarias, días, frecuencias, terceros, identificación) en formato utilizable por WP04 para codificarlas en C-13, y las **reglas de publicidad de crédito por país** para AG-CON.

### 2.11 Óptica regulatoria de `[OD-11]`
Ranking de países núcleo por complejidad regulatoria para el piloto: cobranza, datos, transferencias, registro como agencia de cobranza e IA. Se combina con el ranking de mercado de WP01.

### 2.12 Preguntas legales críticas
Lista priorizada por país. Para cada pregunta: país, pregunta exacta, por qué importa, qué cambia en el diseño según la respuesta y si bloquea el piloto. Debe incluir como mínimo: registro de Sora como agencia de cobranza; transferencias por inferencia fuera del país; disclosure de IA; grabación de llamadas; responsabilidad por errores del agente; uso de datos para mejorar los modelos de Sora Core; requisitos de outsourcing si el tenant está supervisado.

### 2.13 Programa de certificaciones y due diligence de clientes
Roadmap de SOC 2 Type I → II e ISO 27001 (tiempos y costos aproximados `[VERIFICAR]`). Cuestionario de seguridad típico que pedirán las fintechs, con borradores de respuesta para las 20 preguntas más comunes.

### 2.14 Respuesta a incidentes
Severidades, runbook, notificación al tenant y, cuando corresponda, a la autoridad y a los titulares, con plazos por país `[LEGAL]`. Incidentes propios de IA (por ejemplo, el agente comunicó un dato incorrecto a muchos usuarios).

### 2.15 Gobierno de IA
Política de uso de IA, ficha por agente (propósito, datos, límites, riesgos), proceso de cambio de prompts y modelos con aprobación, supervisión humana y registro de incidentes de IA.

## Diagramas obligatorios (mínimo 5)
1. Flujos de datos con fronteras de confianza y de país. 2. Modelo de identidad y acceso (operadores, servicios, tenants). 3. W-08 (derechos de los titulares y opt-out). 4. Respuesta a incidentes. 5. Cifrado y llaves por tenant.

## Criterios de aceptación
- [ ] Inventario de datos completo y clasificado.
- [ ] Matriz de cumplimiento por los 6 países núcleo y las 10 dimensiones, con fuentes oficiales o `[VERIFICAR]`.
- [ ] Reglas de cobranza y de publicidad por país en formato utilizable por C-13.
- [ ] STRIDE de ≥ 10 componentes.
- [ ] Mapeo OWASP para LLM y agentes a controles.
- [ ] Análisis de residencia y transferencias por país, con recomendación.
- [ ] Preguntas legales priorizadas por país, con las bloqueantes identificadas.
- [ ] Ningún cumplimiento afirmado sin `[LEGAL]`.

## Fuera de alcance
Implementación detallada de la infraestructura (WP03) y lógica de agentes (WP04). Aquí se definen los requisitos y controles que ellos deben cumplir.
