# WP05 · Canales e integraciones

| Campo | Valor |
|---|---|
| Oleada | 1 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, en especial 10, 12, 14, 15 y 16 |
| Insumos de otros WPs | Ninguno (oleada 1) |
| Archivo de salida | `03-entregables/WP05-canales-e-integraciones.md` |
| Acceso web | Recomendado: Meta for Developers, AWS End User Messaging, Amazon Connect, Graph API |
| Extensión orientativa | 7,000–10,000 palabras más tablas y diagramas |
| Dueño de | `[OD-01]` voz (decisión final, con WP04 y WP08), `[OD-04]` handoff, `[OD-05]` proveedor de WhatsApp, `[OD-08]` pagos, `[OD-09]` publicación; **Contrato Mínimo de Integración (CMI)**; contratos con Sora Core |

## Rol
Eres arquitecto de integraciones y canales conversacionales, con experiencia en la WhatsApp Business Platform, contact centers en la nube y fintech en LatAm (pagos y cores de crédito).

## Objetivo
Definir **cómo la plataforma habla con el mundo**: WhatsApp, web, voz, SMS/email y redes sociales, y **cómo se integra** con los sistemas de cada tenant, con sus PSPs y con Sora Core. Además, el **contrato de integración** que se le pedirá a cada cliente, multi-país por diseño.

## Secciones obligatorias

### 2.1 WhatsApp
a. **Comparación de proveedores (`[OD-05]`):** AWS End User Messaging Social, Cloud API de Meta directa y BSPs (al menos 2: Twilio, Infobip, Gupshup, 360dialog u otros con presencia en LatAm). Criterios: costo por mensaje (cargo del proveedor más Meta), onboarding de números por país, embedded signup y **rol de Sora como Tech Provider**, soporte de WhatsApp Flows y mensajes interactivos, webhooks, límites, soporte en LatAm, disponibilidad regional en AWS y lock-in. **Recomendación.**
b. **Onboarding de la WABA de cada operación de país:** verificación del negocio en Meta, número, display name, quién es dueño de qué (tenant o Sora) y qué pasa si el contrato termina (portabilidad del número).
c. **Plantillas:** categorías (marketing, utility, authentication), proceso de aprobación, variables, idiomas y **biblioteca de al menos 15 plantillas de ejemplo** (cobranza preventiva, mora temprana, promesa, plan, OTP, confirmación de pago, encuesta CSAT, reactivación de marketing para AG-CON), cada una con su categoría esperada, en español y **al menos 5 también en pt-BR**.
d. **Ventana de servicio de 24 h**, opt-in y opt-out, calificación de calidad, límites de mensajería por tier y cómo escalar el tier; manejo de bloqueos y reportes.
e. **Mensajes interactivos y WhatsApp Flows:** dónde usarlos (selección de plan de pagos, verificación, encuestas).
f. **Webhooks:** firma o verificación, idempotencia, orden, reintentos, estados (enviado, entregado, leído, fallido), y medios (imágenes de comprobantes y notas de voz, con transcripción).
g. **Políticas de Meta sobre IA (2026)** y su impacto en el modelo de Sora (ver `16 §3.5`) `[VERIFICAR]`.
h. **Precios por país núcleo:** tabla de Meta y del proveedor por categoría, para WP08.

### 2.2 Web chat
Widget embebible (JS), autenticación (anónimo L0; sesión del tenant por JWT para L1/L2), streaming, reconexión, accesibilidad, personalización de marca, i18n y entrega de transcripción. Fase 2: SDK móvil.

### 2.3 Voz (`[OD-01]`)
- **Amazon Connect:** disponibilidad de números (DID y toll-free) **por país núcleo** `[VERIFICAR]`, inbound, **outbound** (campañas salientes de Connect con reglas de cumplimiento), grabación, analítica (Contact Lens) y región.
- **Motor conversacional:** Nova Sonic (speech-to-speech), Lex/Transcribe + LLM + Polly, o un proveedor externo de voz IA integrado. Evalúa la calidad en español regional y en pt-BR, la latencia, el costo por minuto y la complejidad.
- Telefonía por país: costos y restricciones (prefijos obligatorios para telemarketing, listas de "no llamar").
- **Decisión:** si la voz entra al MVP, a la fase 1.5 o a la fase 2, con qué motor y en qué países.

### 2.4 SMS y email
Respaldo de notificaciones en el MVP (sender IDs y long codes por país `[VERIFICAR]`, SES) y su evolución a canal conversacional en la fase 2.

### 2.5 Publicación en redes (`[OD-09]`)
Meta Graph API para Instagram y Facebook: permisos, app review, tipos de publicación (imagen, carrusel, reel), programación, límites, insights y tokens por tenant. Alternativas (herramientas de gestión de redes vía API). TikTok, LinkedIn y X en la fase 2. **Recomendación.**

### 2.6 Integración con los sistemas del tenant
- **Patrones:** API síncrona (tools vía Gateway/MCP), eventos (webhooks del tenant hacia Sora: pago recibido, cambio de DPD, desembolso, cambio de datos), y **lotes** (snapshot diario de cartera vía S3 o SFTP) como plan B o complemento.
- **Contrato Mínimo de Integración (CMI):** tabla con cada endpoint que el tenant debe exponer: método, ruta sugerida, request y response (campos, tipos, obligatoriedad), SLA de latencia, idempotencia, el **T-xx que lo usa**, obligatorio u opcional, fase. Más la tabla de **eventos** que el tenant debe emitir. Más el formato del **archivo batch** de cartera. Tiene que ser coherente con las herramientas de `12-CATALOGO`.
- **Autenticación:** OAuth2 client credentials, mTLS o API keys en Secrets Manager; rotación; IP allowlisting (IPs fijas de la celda).
- **Sandbox** del tenant obligatorio; datos de prueba.
- Adaptadores para cores o plataformas comunes en LatAm, si existen y aplican (investiga y lista; muchas fintechs tienen core propio).

### 2.7 Pagos (`[OD-08]`)
- Patrón recomendado: el link o la referencia de pago **lo genera el PSP del tenant**. Sora nunca toca datos de tarjeta (PCI fuera de alcance).
- **Matriz por país núcleo:** medios de pago relevantes (transferencia inmediata, efectivo, billeteras), PSPs comunes, tipo de artefacto (link, referencia, QR, clave) y tiempo de confirmación.
- **Conciliación:** evento de pago → cierre de promesa → pausa de cadencia, y casos de "ya pagué" sin reflejo.

### 2.8 Handoff a humanos (`[OD-04]`)
Integración con el helpdesk del tenant (Zendesk, Freshdesk, HubSpot, Intercom, Salesforce u otros comunes en LatAm), frente a una inbox propia en Sora Console, frente al agent workspace de Amazon Connect. Cómo vuelve la conversación al agente. **Recomendación para el MVP**, considerando lo que ya usan las fintechs objetivo.

### 2.9 Integración con Sora Core
- Contratos que consumen los agentes: Collect (`next-action`), Pulse (`evaluations`), Origin (`decisions/reasons`), Audience (`segments`). Parte de los endpoints de la landing (`10 §4`) y define los campos adicionales necesarios para los agentes.
- **Eventos de retroalimentación hacia Sora Core** (C-27): `contact_attempt`, `contact_result`, `promise_created`, `promise_kept`, `promise_broken`, `payment_plan_accepted`, `opt_out`, `complaint`, `csat`, `content_published`, `content_performance`. Esquema JSON de cada uno, frecuencia, garantías de entrega y anonimización si aplica.
- Conectividad entre la cuenta de Sora Agents y la de Sora Core.

### 2.10 Matriz de integraciones
| Sistema | Propósito | Patrón | Autenticación | Dirección | Fase | Responsable (Sora o tenant) | Riesgo |

## Diagramas obligatorios (mínimo 8)
1. Arquitectura de canales. 2. Secuencia del webhook de WhatsApp (entrante, con medios). 3. Secuencia del envío de una plantilla y su estado. 4. Secuencia de una llamada de voz saliente de cobranza. 5. Patrones de integración con el tenant (API, eventos, lotes). 6. Secuencia de pago y conciliación. 7. Secuencia de handoff y retorno. 8. Flujo de eventos de retroalimentación a Sora Core.

## Criterios de aceptación
- [ ] `[OD-01]`, `[OD-04]`, `[OD-05]`, `[OD-08]` y `[OD-09]` están resueltas con recomendación y trade-offs.
- [ ] El CMI está completo: endpoints, eventos y batch, con campos y trazabilidad a T-xx.
- [ ] Hay ≥ 15 plantillas, ≥ 5 en pt-BR, con categoría.
- [ ] Hay matrices por país de: precios de WhatsApp, números de voz y medios de pago.
- [ ] Los contratos con Sora Core y los eventos de retroalimentación tienen esquema.
- [ ] Hay ≥ 8 diagramas válidos.
- [ ] Hay una tabla de costos de canal lista para WP08.

## Fuera de alcance
Lógica interna de los agentes (WP04), infraestructura general (WP03) y análisis legal detallado (WP06; aquí solo se señalan los puntos y se marcan `[LEGAL]`).
