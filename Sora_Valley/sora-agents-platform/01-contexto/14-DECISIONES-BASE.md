# 14 · Decisiones base (ADR) y decisiones abiertas (OD)

Las **ADR** ya están tomadas. Todo WP debe respetarlas. Si un WP tiene evidencia fuerte en contra, propone el cambio con `[CAMBIO-ADR]` y no actúa como si ya estuviera cambiada.
Las **OD** son decisiones abiertas. Cada una tiene un WP dueño, que debe resolverla con una recomendación explícita. El equipo la confirma en el gate.

---

## 1. Decisiones base (ADR)

### ADR-01 · AWS como nube única de la plataforma de agentes
- **Decisión:** Sora Agents Platform corre en AWS, en cuentas propiedad de Sora Valley.
- **Razón:** directriz del negocio; ecosistema de IA gestionada (Bedrock, AgentCore); regiones en LatAm.
- **Consecuencia:** se usan servicios gestionados de AWS siempre que existan y cumplan. Se usan servicios de terceros solo cuando AWS no cubre la necesidad (por ejemplo, la API de publicación de redes sociales) o cuando el costo o la calidad lo justifican con números.

### ADR-02 · Modelos vía Amazon Bedrock, con Claude como familia principal y capa agnóstica
- **Decisión:** la inferencia LLM pasa por Amazon Bedrock. La familia principal es Claude: **Sonnet 5.5** para conversación, **Haiku 5.5** para el router, la clasificación, los resúmenes y los evaluadores, y **Opus 5.5** solo para tareas complejas fuera de línea. Existe una capa de acceso a modelos (C-15) que permite cambiar de modelo o proveedor por configuración (Amazon Nova u otros).
- **Notas técnicas que WP04 y WP08 deben verificar:**
  - Los IDs de modelo en Bedrock llevan el prefijo `anthropic.` (por ejemplo, `anthropic.claude-sonnet-5-5`). Con inferencia cross-region se usan perfiles de inferencia con prefijo geográfico o global. Los IDs y perfiles exactos están en la consola de Bedrock `[VERIFICAR]`.
  - Cliente recomendado para código propio: el SDK oficial de Anthropic con su cliente para Bedrock (Messages API sobre Bedrock), o la API Converse de Bedrock si el framework lo requiere `[VERIFICAR]`.
  - En Sonnet 5.5 y Opus 5.5 **no se admite `tool_choice` forzado** (`any` o `tool`). Hay que usar `auto` más `strict: true` en las herramientas e instrucciones explícitas, o salidas estructuradas.
  - El razonamiento es **adaptativo**. Para chat con baja latencia conviene un `effort` bajo. En Sonnet 5.5, el modo de razonamiento `between_tools` permite apagar el pensamiento extendido en effort `high` o menor.
  - **Prompt caching:** prefijo estable en el orden herramientas → system → mensajes. En Bedrock el caché es por organización (cuenta). Las lecturas cuestan mucho menos que la entrada normal (ver P-02 y P-03).
  - Manejar el `stop_reason: refusal` con un fallback de modelo del lado del cliente.
  - El procesamiento por lotes (batch) se usa para cargas no interactivas: contenido masivo y evals offline.

### ADR-03 · Runtime de agentes sobre Amazon Bedrock AgentCore
- **Decisión:** los agentes conversacionales se ejecutan en **AgentCore Runtime**, junto con **Memory**, **Gateway** (herramientas vía MCP), **Identity**, **Observability**, **Policy** y **Evaluations** donde aplique.
- **Razón:** aislamiento por sesión, escalado gestionado, herramientas vía MCP y observabilidad nativa. Reduce la infraestructura propia del MVP.
- **Abierto:** el framework del agente dentro del runtime (`[OD-03]`). WP04 compara Strands Agents, el Tool Runner del SDK de Anthropic y LangGraph. También compara AgentCore con una alternativa propia en ECS Fargate en costo y control, y confirma o propone un `[CAMBIO-ADR]`.

### ADR-04 · "El LLM conversa, el sistema decide"
- **Decisión:** toda acción W-alto (montos, planes de pago, descuentos, cambios de datos, publicación de contenido) pasa por el **Policy and Decision Service (C-13)**, que es determinista, y por las APIs de Sora Core. El LLM elige *qué herramienta* usar y *cómo explicarlo*, pero **no calcula montos, no inventa ofertas y no decide la elegibilidad**.
- **Consecuencia:** las herramientas de escritura validan sus parámetros contra la política del tenant y del país antes de ejecutar. Los rechazos vuelven al agente como resultado de la herramienta, con un motivo.

### ADR-05 · Workflows deterministas con Step Functions y EventBridge
- **Decisión:** los procesos largos o programados se modelan como workflows en **AWS Step Functions**, disparados por **EventBridge Scheduler** o por eventos de **EventBridge**. Incluye cadencias de cobranza, seguimiento de promesas, la aprobación de contenido con espera humana, el onboarding de tenants y las solicitudes de derechos sobre datos. Los workflows invocan a los agentes cuando hace falta conversar.
- **Lista inicial de workflows (WP04 los detalla):**

| ID | Workflow |
|---|---|
| W-01 | Cadencia de cobranza outbound (next-action → validación de ventanas y frecuencias por país → plantilla → espera de respuesta → agente) |
| W-02 | Promesa de pago: recordatorio, verificación de pago y promesa cumplida o rota |
| W-03 | Plan de pagos con confirmación y formalización (con aprobación humana opcional según el monto) |
| W-04 | Pipeline de contenido: generación, revisión automática, aprobación humana (task token), publicación y medición |
| W-05 | Handoff a humano y retorno al agente |
| W-06 | Ingesta y actualización de la base de conocimiento por tenant y país |
| W-07 | Onboarding y aprovisionamiento de un tenant y de una operación de país |
| W-08 | Opt-out y derechos sobre datos personales (acceso, rectificación, cancelación, oposición y equivalentes por país) |

### ADR-06 · Multi-tenancy en modelo "pool" con aislamiento lógico fuerte
- **Decisión (MVP):** infraestructura compartida y aislamiento lógico: `tenant_id` y `country_op_id` en cada registro y evento; ABAC con IAM; una **clave KMS por tenant**; prefijos de S3 por tenant; base de conocimiento y namespace de memoria por tenant y país; secretos por tenant; cuotas y throttling por tenant.
- **Opción premium (fase 3):** "silo", con una cuenta AWS dedicada por tenant, o despliegue en la cuenta del cliente.

### ADR-07 · Canales
- **WhatsApp:** canal primario, sobre **AWS End User Messaging Social** (WhatsApp Business Platform). WP05 debe compararlo con la Cloud API de Meta directa y con BSPs (`[OD-05]`).
- **Web chat:** widget propio sobre API Gateway WebSocket.
- **Voz:** **Amazon Connect**. El motor conversacional se decide en `[OD-01]`.
- **Publicación en redes:** Meta Graph API (Instagram y Facebook) en el MVP.

### ADR-08 · Regiones y residencia de datos (provisional, multi-país)
- **Principio:** la arquitectura es **por celdas regionales**. Cada operación de país de un tenant tiene una **región "hogar"** donde viven sus datos en reposo. El plano de control (configuración, facturación) puede ser global.
- **Candidatas en LatAm:** `sa-east-1` (São Paulo) y `mx-central-1` (México), y la región de Chile si ya está operativa `[VERIFICAR]`. **Respaldo:** `us-east-1` para servicios no disponibles en LatAm.
- **Inferencia LLM:** es probable que requiera **inferencia cross-region** (perfil geográfico o global) si los modelos Claude no están disponibles en la región hogar. Esto implica **transferencia internacional de datos**, que debe analizarse país por país `[LEGAL]`.
- **MVP:** **una sola celda**. WP03 recomienda cuál (`[OD-06]`), con la matriz de disponibilidad de servicios por región y el análisis de residencia de WP06. El diseño debe permitir agregar celdas sin rediseñar.

### ADR-09 · Infraestructura como código obligatoria
- **Decisión:** el 100% de la infraestructura se define como código y hay pipelines por entorno. La herramienta por defecto es **AWS CDK (TypeScript)**. WP03 puede proponer Terraform con justificación.

### ADR-10 · Lenguajes
- **Agentes, workers y herramientas:** Python.
- **Consola web:** TypeScript (React o Next.js).
- **Infraestructura:** según ADR-09.

### ADR-11 · Observabilidad y evaluación desde el día 1
- **Decisión:** trazas OpenTelemetry por conversación, turno, llamada al modelo y herramienta, con **AgentCore Observability y CloudWatch** como base. Las evals offline son un gate obligatorio antes de promover cualquier cambio de prompt, configuración o modelo. Hay evals en línea por muestreo. WP07 puede proponer una herramienta complementaria.

### ADR-12 · Datos de un tenant solo para ese tenant
- **Decisión:** los datos de un tenant no se usan para mejorar agentes ni modelos de otro tenant sin autorización contractual explícita. El ciclo de retroalimentación hacia Sora Core es **por tenant**. Se usa Bedrock con la configuración que garantiza que los prompts no se usan para entrenar los modelos del proveedor `[VERIFICAR términos vigentes]`.

### ADR-13 · Transparencia de IA
- **Decisión:** el usuario final siempre sabe que habla con un asistente de IA, puede pedir una persona y ve qué acciones se ejecutaron sobre su cuenta.

### ADR-14 · Multi-idioma y multi-país por configuración
- **Decisión:** idioma, variante, tono, zona horaria, moneda, medios de pago, reglas de cobranza, reglas de publicidad, leyendas y plantillas son **atributos de la operación de país**, no código. El MVP opera en español. **pt-BR está diseñado y presupuestado**, y su activación depende de `[OD-11]`.

---

## 2. Decisiones abiertas (OD)

| ID | Decisión | WP dueño | Opciones a evaluar | Criterios |
|---|---|---|---|---|
| OD-01 | Voz: si entra al MVP y con qué motor | WP05 (WP04 diseña, WP08 costea) | (a) Amazon Connect + Nova Sonic (speech-to-speech); (b) Connect + Transcribe + LLM + Polly; (c) proveedor externo de voz IA integrado a Connect; (d) voz en fase 2 | Latencia, calidad en español y portugués, costo por minuto, complejidad, cumplimiento (grabación, horarios) |
| OD-02 | Vector store del RAG | WP03 | S3 Vectors · OpenSearch Serverless · Aurora PostgreSQL con pgvector | Costo base fijo, aislamiento por tenant, latencia, operación |
| OD-03 | Framework de agentes | WP04 | Strands Agents · Tool Runner del SDK de Anthropic · LangGraph | Control del loop, compatibilidad con AgentCore, testabilidad, observabilidad, curva de aprendizaje |
| OD-04 | Herramienta de handoff e inbox humano | WP05 | Integrar el helpdesk del tenant (Zendesk, Freshdesk, HubSpot, Intercom, etc.) · inbox propia en Sora Console · agent workspace de Amazon Connect | Lo que ya usan los clientes objetivo, costo, tiempo de construcción, experiencia del agente humano |
| OD-05 | Proveedor de WhatsApp | WP05 | AWS End User Messaging Social · Cloud API de Meta directa · BSP (por ejemplo, Twilio, Infobip, Gupshup, 360dialog) | Costo por mensaje, onboarding de números por país, soporte, funciones (Flows, interactivos), rol de Sora como Tech Provider |
| OD-06 | Región de la celda del MVP y plan de celdas | WP03 (con WP06) | `sa-east-1` · `mx-central-1` · `us-east-1` · combinación | Disponibilidad de servicios y modelos, latencia, residencia y transferencias por país, costo |
| OD-07 | Stack de Sora Console | WP03 | Next.js en Amplify Hosting · SPA en S3 + CloudFront con API Gateway · AppSync | Velocidad de desarrollo, autenticación, costo |
| OD-08 | Integración de pagos | WP05 | Vía el PSP del tenant (recomendado) · conectores propios por país | Alcance de PCI, tiempo de integración, cobertura por país |
| OD-09 | Publicación de contenido | WP05 | Graph API directa · herramientas de gestión de redes vía API | App review de Meta, costo, mantenimiento |
| OD-10 | Modelo de precio | WP09 | Por conversación · por resolución · por minuto · por resultado (% recuperado) · suscripción · híbrido | Margen, predictibilidad para el cliente, alineación de incentivos, benchmark |
| OD-11 | País del primer piloto y orden de expansión en LatAm | WP01 (mercado) + WP06 (regulación); la decide el equipo | MX · CO · BR · PE · CL · AR · otro | Tamaño del mercado BNPL y microcrédito, complejidad regulatoria (cobranza y datos), disponibilidad de AWS y de modelos, idioma, red de contactos y cliente de diseño disponible |
