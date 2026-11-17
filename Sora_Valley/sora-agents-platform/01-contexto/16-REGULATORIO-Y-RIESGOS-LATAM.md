# 16 · Marco regulatorio LatAm (a verificar) y riesgos semilla

> **ADVERTENCIA:** este documento es un **mapa de puntos a verificar**, no asesoría legal. Todo lo que contiene lleva `[LEGAL]` y/o `[VERIFICAR]` implícito. WP06 lo convierte en una matriz de cumplimiento verificada con fuentes oficiales y una lista de preguntas para abogados de cada país. La regulación cambia con frecuencia, y varias leyes de datos de la región se reformaron entre 2024 y 2026.

---

## 1. Dimensiones regulatorias que afectan a la plataforma

| # | Dimensión | Por qué importa | Agentes afectados |
|---|---|---|---|
| D1 | Protección de datos personales | Sora Valley trata datos personales y financieros de los usuarios finales en nombre del tenant (como encargado o procesador); hay transferencias internacionales por la nube y la inferencia | Todos |
| D2 | Cobranza extrajudicial | Horarios, frecuencias, trato digno, terceros, identificación, registros | AG-COB, AG-RTR |
| D3 | Protección al consumidor financiero | Atención de quejas, información clara, rutas formales de reclamo | AG-ATC |
| D4 | Publicidad de crédito | Costo total del crédito, leyendas obligatorias, claims prohibidos | AG-CON |
| D5 | Telemarketing y listas de exclusión | Registros de "no llamar" y de exclusión de publicidad financiera | AG-COB (voz), AG-CON (campañas) |
| D6 | Regulación del tenant como entidad financiera | Requisitos de tercerización (outsourcing) y de nube cuando el tenant es una entidad supervisada | Plataforma |
| D7 | PLD/AML y KYC | Fase 2 (AG-KYC) | AG-KYC |
| D8 | Grabación de llamadas y consentimiento | Aviso de grabación, retención | AG-COB y AG-ATC por voz |
| D9 | IA | Transparencia, decisiones automatizadas, regulaciones emergentes de IA en la región | Todos |
| D10 | Políticas de plataformas | WhatsApp Business (opt-in, plantillas, políticas sobre IA), políticas de Meta para publicidad y contenido financiero | Todos los de WhatsApp y AG-CON |

## 2. Matriz por país (punto de partida para WP06)

> Normas citadas de memoria como **pistas de investigación**. WP06 verifica vigencia, número exacto, reglamentos y autoridad.

| País | D1 Datos personales | D2 Cobranza | D3/D4 Consumidor y publicidad | D5 Listas de exclusión | D6 Supervisión del tenant |
|---|---|---|---|---|---|
| **MX** | Nueva LFPDPPP (publicada en marzo de 2025); la autoridad cambió tras la extinción del INAI | Disposiciones de CONDUSEF sobre despachos de cobranza; registro REDECO; ventanas horarias de contacto (se cita 7:00–22:00) | CONDUSEF; Ley de Transparencia y Ordenamiento de los Servicios Financieros (CAT en publicidad); UNE para reclamos | REUS (CONDUSEF), REPEP (PROFECO) | CNBV (Ley Fintech, IFPE, SOFOM E.R.); SOFOM E.N.R. bajo CONDUSEF |
| **CO** | Ley 1581 de 2012 (habeas data) y Ley 1266 de 2008 (habeas data financiero); autoridad: SIC | Ley 2300 de 2023 ("Dejen de fregar"), que limita horarios, días y frecuencia de contacto de cobranza | Superintendencia Financiera (SFC) para entidades vigiladas; SIC para no vigiladas; Estatuto del Consumidor | Registro de exclusión (verificar mecanismo vigente) | SFC (vigiladas); no vigiladas bajo la SIC |
| **BR** | LGPD (Lei 13.709/2018); autoridad: ANPD; regulación de transferencias internacionales (cláusulas estándar) | CDC art. 42 (cobro abusivo); Ley de Superendeudamiento (14.181/2021) | CDC; normas del Banco Central sobre transparencia; Procon | "Não Me Perturbe"; reglas de ANATEL para llamadas masivas (prefijo para telemarketing) | Banco Central (SCD, SEP, instituciones de pago); outsourcing y nube regulados por el BCB para entidades autorizadas |
| **PE** | Ley 29733 de Protección de Datos Personales y su reglamento (actualizado recientemente); autoridad: ANPD (MINJUSDH) | Código de Protección y Defensa del Consumidor (métodos abusivos de cobranza); INDECOPI | INDECOPI; SBS para entidades supervisadas; transparencia de costos | Registro "Gracias… No Insista" (INDECOPI) | SBS (supervisadas) |
| **CL** | Ley 19.628 y la **nueva Ley 21.719 (2024)** que crea la Agencia de Protección de Datos (vigencia escalonada; verificar la fecha en que entra en vigor) | Ley 19.496 del consumidor (art. 37, cobranza extrajudicial: horarios, frecuencia, prohibiciones) | SERNAC; CMF; Ley Fintec 21.521 | Verificar mecanismo de "No Molestar" | CMF |
| **AR** | Ley 25.326 de Protección de Datos Personales; autoridad: AAIP; proyecto de reforma en curso | Ley 24.240 de Defensa del Consumidor (art. 8 bis, trato digno, aplicado a la cobranza) | Defensa del Consumidor; normas del BCRA sobre transparencia | Registro Nacional "No Llame" (Ley 26.951) | BCRA (proveedores no financieros de crédito, registro) |
| **LATAM-OTROS** | Revisión caso por caso | Revisión caso por caso | Revisión caso por caso | Revisión caso por caso | Revisión caso por caso |

## 3. Temas transversales críticos

1. **Rol legal de Sora Valley.** Es probablemente **encargado o procesador** de datos por cuenta del tenant, que es el **responsable o controlador**. Hace falta un contrato de tratamiento de datos (DPA) y una lista de subencargados: AWS, Meta (WhatsApp), proveedores de telefonía y, eventualmente, de verificación. `[LEGAL]`
2. **¿Cobrar a nombre del tenant convierte a Sora Valley en "despacho" o "agencia de cobranza"?** En algunos países las agencias de cobranza tienen registros u obligaciones propias, como REDECO en México. **Es una pregunta legal prioritaria por país.** `[LEGAL]` `[RIESGO R-12]`
3. **Transferencias internacionales por la inferencia.** Si la inferencia del LLM ocurre fuera del país del usuario (cross-region), hay transferencia internacional. Cada ley exige mecanismos distintos (consentimiento, cláusulas, adecuación, aviso de privacidad). `[LEGAL]`
4. **Transparencia de IA y decisiones automatizadas.** Varias leyes de datos dan derechos frente a decisiones automatizadas. La plataforma debe permitir pedir una persona y explicar las decisiones; esto ya está en `[ADR-13]`. `[LEGAL]`
5. **Políticas de WhatsApp Business.** Hay opt-in obligatorio para mensajes iniciados por la empresa, categorías de plantillas, calificación de calidad y límites de mensajería. Además, la política de Meta vigente desde enero de 2026 restringe a los proveedores de **asistentes de IA de propósito general** en la WhatsApp Business API. Los asistentes de atención específicos de un negocio siguen permitidos, pero hay que **verificar cómo encaja el modelo de Sora**, que opera agentes a nombre de cada fintech `[VERIFICAR]` `[RIESGO R-09]`.
6. **PCI DSS.** Evitarlo por diseño: el agente **nunca** recibe ni procesa datos de tarjeta; los pagos se hacen siempre por link o referencia del PSP del tenant.
7. **Grabación de llamadas.** Aviso al inicio, propósito, retención y acceso, según el país. `[LEGAL]`
8. **Clientes vulnerables.** Buenas prácticas internacionales (y en algunos países, normas) exigen un trato especial. Ya está incorporado como protocolo en AG-RTR y AG-COB.
9. **Retención y borrado.** Los plazos varían por país y por tipo de dato (gestiones de cobranza, quejas, grabaciones). Las políticas de retención se configuran por país `[S-39]`.
10. **Facturación a clientes.** La facturación electrónica es obligatoria en varios países (por ejemplo, CFDI en México, factura electrónica DIAN en Colombia, NF-e/NFS-e en Brasil). Afecta a C-31 y WP11 `[VERIFICAR]`.

## 4. Registro de riesgos semilla

| ID | Riesgo | Prob. | Impacto | Mitigación inicial | WP |
|---|---|---|---|---|---|
| R-01 | El agente "alucina" montos, fechas u ofertas | M | A | `[ADR-04]`: cifras solo desde herramientas; validación determinista; evals de grounding | WP04, WP07 |
| R-02 | Prompt injection desde mensajes del usuario, documentos de la KB o resultados de herramientas | A | A | Guardrails, separación de instrucciones y datos, mínimo privilegio de herramientas, red teaming | WP04, WP06 |
| R-03 | Fuga de datos entre tenants (memoria, KB, logs, caché) | B | Crítico | ABAC, KMS por tenant, namespaces, pruebas automáticas de aislamiento | WP03, WP06 |
| R-04 | Bloqueo del número o caída de la calificación de calidad de WhatsApp por exceso de mensajes o reportes | M | A | Opt-in, frecuencias conservadoras, monitoreo de la calidad, números de respaldo | WP05 |
| R-05 | Queja regulatoria por hostigamiento o cobranza indebida | M | A | Ventanas y frecuencias en el motor de políticas por país; bitácora; auditorías | WP04, WP06 |
| R-06 | Sobrecosto de tokens por loops o conversaciones largas | M | M | Presupuestos por conversación, límites de turnos, alertas de costo por tenant | WP04, WP08 |
| R-07 | Disponibilidad limitada de modelos o servicios en las regiones de LatAm | A | M | Inferencia cross-region; matriz de disponibilidad; diseño por celdas | WP03 |
| R-08 | Latencia de voz inaceptable | M | A | Motor speech-to-speech, presupuesto de latencia, piloto acotado | WP05 |
| R-09 | Cambios en las políticas de Meta (IA, precios, categorías de plantillas) | M | A | Seguimiento de políticas, multi-canal, diseño desacoplado del proveedor | WP05 |
| R-10 | Daño reputacional por contenido generado (AG-CON) | M | A | Aprobación humana obligatoria, revisión de cumplimiento y marca | WP04 |
| R-11 | Retrasos en integraciones por la calidad de las APIs del tenant | A | A | Contrato Mínimo de Integración, conectores por lotes como plan B, sandbox | WP05, WP10 |
| R-12 | Sora Valley clasificada como agencia de cobranza con obligaciones de registro en algún país | M | A | Consulta legal temprana por país; estructura contractual | WP06 |
| R-13 | Account takeover vía SIM swap o números reciclados | M | A | Niveles L1/L2; OTP para acciones sensibles; detección de cambios de número | WP06 |
| R-14 | Mala calidad en variantes locales (voseo, modismos) o en portugués | M | M | Evals por país e idioma; diseñadores conversacionales locales | WP07 |
| R-15 | Dependencia de un solo proveedor de modelo | B | M | Capa de acceso agnóstica (C-15); evals de modelos alternativos | WP04 |
| R-16 | Transferencias internacionales no permitidas o sin el mecanismo legal correcto | M | A | Análisis por país; celdas regionales; avisos y DPA | WP06, WP03 |
| R-17 | Costo de cumplimiento multi-país mayor al esperado (abogados, adaptaciones) | A | M | Priorizar países (`[OD-11]`); plantillas legales reutilizables | WP11, WP12 |
| R-18 | Volatilidad cambiaria (por ejemplo, ARS) que erosiona el margen | M | M | Precios en USD; cláusulas de ajuste | WP09 |
