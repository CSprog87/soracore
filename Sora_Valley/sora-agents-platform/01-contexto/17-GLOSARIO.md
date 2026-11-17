# 17 · Glosario

## Negocio y crédito

| Término | Definición |
|---|---|
| BNPL | *Buy now, pay later*: compra a plazos, normalmente en cuotas cortas y sin tarjeta. |
| Microcrédito | Crédito de monto bajo, a corto plazo, frecuentemente para personas sub-bancarizadas. |
| Cartera | Conjunto de créditos vigentes de una fintech. "Gestionar cartera" incluye servicing y cobranza. |
| Tenant | Cliente de Sora Valley: una fintech (o grupo) que usa la plataforma. |
| Operación de país | Instancia de un tenant en un país, con su regulación, moneda, idioma, número de WhatsApp e integraciones. |
| DPD | *Days past due*: días de atraso de una cuota. |
| Mora temprana / media / tardía | Tramos de atraso. En este paquete: 1–30, 31–90 y más de 90 días (configurables). |
| Roll rate | Porcentaje de cuentas que pasa de un tramo de mora al siguiente. |
| Promesa de pago | Compromiso del deudor de pagar un monto en una fecha. Se mide si se cumple. |
| Plan de pagos / reestructura | Nuevo calendario de pagos acordado, dentro de las políticas del tenant. |
| Holdout | Grupo de control al que no se aplica la intervención, para medir el impacto incremental. |
| Costo total del crédito | Indicador obligatorio en la publicidad de crédito en varios países (por ejemplo, CAT en México, CET en Brasil y Argentina, TCEA en Perú, CAE en Chile). |
| PSP | Proveedor de servicios de pago. |
| Medios de pago locales | Ejemplos: SPEI, CoDi/DiMo y efectivo en tiendas (MX); PIX y boleto (BR); PSE, Nequi y Bre-B (CO); Yape, Plin y PagoEfectivo (PE); transferencias y Webpay (CL); transferencias, Mercado Pago y efectivo (AR). `[VERIFICAR]` |
| Sora Core | El núcleo de modelos de Sora Valley: Origin, Limit, Pulse, Collect y Audience. |
| Next-best-action | La siguiente acción óptima de cobranza que define Sora Collect: canal, hora, tono y oferta. |

## Canales

| Término | Definición |
|---|---|
| WABA | *WhatsApp Business Account*: cuenta de WhatsApp Business Platform de un negocio. |
| BSP | *Business Solution Provider*: intermediario autorizado por Meta para la WhatsApp Business API. |
| Tech Provider | Rol de Meta para plataformas que dan onboarding a la WABA de sus clientes (embedded signup). |
| Plantilla (template) | Mensaje preaprobado por Meta, obligatorio para iniciar conversaciones fuera de la ventana de servicio. Categorías: marketing, utility y authentication. |
| Ventana de servicio de 24 h | Periodo tras el último mensaje del usuario en que el negocio puede responder con texto libre. |
| Opt-in / opt-out | Consentimiento del usuario para recibir mensajes de la empresa, y su revocación. |
| WhatsApp Flows | Formularios interactivos dentro de WhatsApp. |
| DID | Número telefónico directo para llamadas entrantes y salientes. |
| Barge-in | Capacidad del usuario de interrumpir al agente de voz mientras habla. |
| Speech-to-speech (S2S) | Modelo de voz que procesa audio y genera audio sin un pipeline separado de STT y TTS. |
| STT / TTS | *Speech-to-text* y *text-to-speech*. |

## IA y agentes

| Término | Definición |
|---|---|
| Agente | Sistema que usa un LLM para decidir qué herramientas invocar y cómo responder, en un loop, hasta resolver una tarea. |
| Workflow determinista | Proceso con pasos definidos en código o máquina de estados. Puede invocar agentes en pasos puntuales. |
| Router | Componente que clasifica el mensaje y elige al agente responsable. |
| Handoff | Transferencia de la conversación a una persona, con contexto. |
| Handover | Transferencia entre agentes de IA. |
| RAG | *Retrieval-augmented generation*: el agente busca en una base de conocimiento antes de responder y cita la fuente. |
| Guardrails | Filtros y validaciones de la entrada y la salida del LLM: temas, PII, toxicidad, grounding. |
| Grounding | Que la respuesta esté sustentada en datos de herramientas o fuentes, no inventada. |
| MCP | *Model Context Protocol*: protocolo estándar para exponer herramientas a los agentes. |
| Prompt caching | Reutilización del prefijo estable del prompt entre llamadas para reducir costo y latencia. |
| Effort | Parámetro que controla cuánto razona el modelo; hace un trade-off entre calidad, costo y latencia. |
| Batch | Procesamiento asíncrono por lotes con descuento, para tareas no interactivas. |
| LLM-as-judge | Uso de un modelo para calificar conversaciones o salidas según una rúbrica. |
| Eval set | Conjunto de casos de prueba con resultado esperado, para medir la calidad antes de desplegar. |
| Red teaming | Pruebas adversariales: jailbreaks, prompt injection, ingeniería social. |
| Contención | Porcentaje de conversaciones resueltas sin humano. |
| FRT / AHT | *First response time* / *average handle time*. |
| CSAT | *Customer satisfaction score*. |
| FDE | *Forward-deployed engineer*: ingeniero que implementa la solución junto al cliente. |

## AWS

| Término | Definición |
|---|---|
| Amazon Bedrock | Servicio gestionado de acceso a modelos fundacionales (Claude, Nova y otros). |
| AgentCore | Conjunto de servicios de Bedrock para ejecutar agentes: Runtime, Memory, Gateway, Identity, Policy, Observability, Evaluations, Browser y Code Interpreter. |
| Inferencia cross-region (CRIS) | Perfiles de inferencia de Bedrock que enrutan las solicitudes a otras regiones (geográficas o globales) para obtener capacidad o disponibilidad. |
| Bedrock Knowledge Bases | Servicio gestionado de RAG: ingesta, chunking, embeddings y búsqueda. |
| Bedrock Guardrails | Servicio de filtros para entradas y salidas de LLMs. |
| AWS End User Messaging Social | Servicio de AWS para la WhatsApp Business Platform. |
| Amazon Connect | Contact center en la nube: telefonía, flujos, grabación y analítica. |
| Step Functions | Orquestador de workflows como máquinas de estados. |
| EventBridge / Scheduler | Bus de eventos y programador de tareas. |
| Celda (cell) | Despliegue completo e independiente del plano de datos en una región, que atiende a un subconjunto de tenants. |
| Pool / Silo | Multi-tenancy compartida con aislamiento lógico, frente a infraestructura dedicada por tenant. |
| ABAC | Control de acceso basado en atributos (por ejemplo, `tenant_id` en etiquetas y claims). |
| SCP | *Service control policy* de AWS Organizations. |
| KMS | Servicio de gestión de llaves de cifrado. |

## Regulatorio (abreviaturas que aparecen en el paquete)

| Abreviatura | País | Significado |
|---|---|---|
| CONDUSEF / REDECO / REUS / UNE | MX | Protección al usuario financiero / registro de despachos de cobranza / registro de exclusión de publicidad / unidad de atención de reclamos |
| LFPDPPP | MX | Ley Federal de Protección de Datos Personales en Posesión de los Particulares |
| CNBV / IFPE / SOFOM | MX | Supervisor bancario / institución de fondos de pago electrónico / sociedad financiera de objeto múltiple |
| SFC / SIC | CO | Superintendencia Financiera / Superintendencia de Industria y Comercio |
| LGPD / ANPD / BCB / CDC | BR | Ley General de Protección de Datos / su autoridad / Banco Central / Código de Defensa del Consumidor |
| SBS / INDECOPI | PE | Superintendencia de Banca y Seguros / autoridad de consumo |
| CMF / SERNAC | CL | Comisión para el Mercado Financiero / Servicio Nacional del Consumidor |
| BCRA / AAIP | AR | Banco Central / Agencia de Acceso a la Información Pública (datos personales) |
| PLD / AML | Todos | Prevención de lavado de dinero |
| DPA | Todos | Contrato de tratamiento de datos (responsable–encargado) |
| Derechos ARCO | Varios | Acceso, rectificación, cancelación y oposición; varían por país (por ejemplo, portabilidad en la LGPD) |
