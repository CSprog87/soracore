# 10 · Sora Valley: la propuesta de negocio (según la landing)

> Fuente: `Main.dc.html`, el mockup de la landing de Sora Valley. Todo lo que no aparece en la landing está marcado como **inferencia** o **supuesto**.

## 1. En una frase

**Sora Valley ofrece modelos de riesgo listos para producción, sin que la fintech tenga que construir un equipo de ML.** Es machine learning y decisioning para fintechs de **BNPL y microcréditos** en **Latinoamérica**.

> **Alcance geográfico (directriz del equipo, prevalece sobre la landing):** la landing dice "Latinoamérica, empezando por México", pero **el alcance de Sora Agents Platform es LatAm multi-país**. México es un mercado más, no el ancla del diseño. Los ejemplos de la landing en MXN son ilustrativos. Ver §8 y `[OD-11]` (país del primer piloto).

## 2. Problema que ataca

| # | Problema | Texto de la landing |
|---|---|---|
| 01 | Riesgo desde el día 1 | "Cada solicitud necesita una decisión informada desde el lanzamiento." |
| 02 | Talento de ML escaso y caro | "Contratar y retener un equipo propio consume tiempo y presupuesto." |
| 03 | Meses para montar infraestructura | "Datos, entrenamiento y despliegue retrasan tu salida al mercado." |

## 3. Sora Core: el diferenciador central

"Un solo cerebro para todo el ciclo de vida del crédito." Todos los productos se conectan al mismo núcleo, y lo que aprende uno mejora a los demás:

- Lo que pasa en **cobranza** reentrena la **originación**.
- El comportamiento de la **cartera** afina cada **línea asignada**.
- Las **campañas** aprenden de cómo paga cada segmento.

**Implicación para la plataforma de agentes:** los agentes son *ejecutores conectados a Sora Core*. Consumen sus decisiones (a quién contactar, cuándo, con qué oferta) y devuelven cada interacción como dato de entrenamiento. Este ciclo es el principal diferenciador frente a un proveedor genérico de chatbots.

## 4. Catálogo de productos

Etapas del ciclo de vida del crédito en la landing: **Adquirir → Aprobar → Asignar → Crecer → Cobrar**.

| Producto | Categoría | Promesa | Capacidades clave | Métricas | Endpoint de ejemplo | Etapas |
|---|---|---|---|---|---|---|
| **Sora Origin** | Riesgo · Originación | "Decide en milisegundos a quién darle crédito, incluso a quien el buró no ve." | Score < 300 ms; datos alternativos (dispositivo, comportamiento en la app, transaccional autorizado); fraude integrado; arranque en frío; decisiones explicables; champion/challenger | Tasa de aprobación, mora temprana, pérdida esperada, fraude de primera parte, tiempo de decisión | `POST /v1/origin/decisions` → `decision`, `score`, `fraud_flags`, `reasons[]`, `model_version` | Aprobar |
| **Sora Limit** | Riesgo · Asignación de línea | "Préstale a cada uno lo que maximiza tu ganancia." | Optimiza la ganancia esperada; respeta restricciones reales (fondeo, pérdida máxima); simulador de escenarios; frontera eficiente | Ganancia por usuario, pérdida esperada, volumen colocado, utilización, uso de fondeo | `POST /v1/limit/assignments` → `recommended_line`, `currency: MXN` | Asignar |
| **Sora Pulse** | Riesgo · Gestión de línea | "Tu cartera, re-evaluada todos los días." | Re-evaluación continua; aumentos causales (experimentos); alertas tempranas; todo versionado | Utilización, mora a 30 días, rentabilidad, reactivación, roll rate | `POST /v1/pulse/evaluations` → `action: increase`, `new_line`, `reasons[]`, `experiment_group` | Crecer |
| **Sora Collect** | Riesgo · Cobranza inteligente | "Cobra más, molestando menos." | Probabilidad de pago diaria; **siguiente mejor acción** (a quién, cuándo, canal, tono, oferta: recordatorio, plan de pagos, reestructuración); **agentes de cobranza por WhatsApp y voz, con escalamiento a humanos**; aprendizaje continuo | Tasa de recuperación, roll rate, contactabilidad, costo por peso recuperado, promesas cumplidas | `POST /v1/collect/next-action` → `channel: whatsapp`, `send_at`, `tone: amable`, `offer: reminder`, `pay_probability: 0.72` | Cobrar |
| **Sora Audience** | Crecimiento · Segmentación y campañas | "Deja de regalar incentivos a quien igual iba a comprar." | Modelos de uplift; 4 segmentos (persuadibles, compradores seguros, casos perdidos, no molestar); bandits multi-brazo; presupuesto optimizado | Costo de incentivo por conversión, activación, ingreso incremental, retorno de campañas | `POST /v1/audience/segments` → segmentos + `recommended_incentive` | Adquirir, Crecer |
| **Sora Agents** | IA y plataforma · Agentes de IA | "Un equipo de operaciones que nunca duerme." | **KYC en minutos** (documentos, selfie, prueba de vida); **atención 24/7** con acceso seguro a la cuenta; **copiloto de riesgo** (explica decisiones del modelo); **workflows configurables** con monitoreo y trazabilidad | Tiempo de KYC, conversión del onboarding, costo de atención, tiempo de primera respuesta, casos escalados a humanos | `POST /v1/agents/kyc/sessions` → `status: verified`, `extracted`, `liveness`, `trace_id` | Adquirir, Aprobar |
| **Sora Forge** | IA y plataforma · MLOps en tu casa | "De notebook a producción en días, no en trimestres." | Plataforma en la nube del cliente (feature store, entrenamiento, registro, serving, drift); niveles de madurez 0 → 2 → **"Nivel IA: agentes y LLMs en producción"**; agnóstica a la nube (GCP, AWS u otras); transferencia de conocimiento | Tiempo a producción, frecuencia de despliegue, detección de drift, tiempo de reentrenamiento | `POST /v1/forge/deployments` | Todas |

### Flujo de Sora Agents según la landing

1. **Recibe el caso:** un documento, una conversación o una solicitud.
2. **Lee y valida:** extrae datos y los contrasta uno por uno.
3. **Actúa:** resuelve con acceso seguro a los sistemas del cliente.
4. **Escala si hace falta:** pasa el caso a una persona con todo el contexto.

### Demo de atención en la landing (comportamiento esperado del agente)

| Pregunta del usuario | Respuesta | Acciones visibles |
|---|---|---|
| "¿Cuándo es mi próxima cuota?" | "Tu próxima cuota vence el 28 de noviembre y es de $1,250 MXN." | Consultó tu cuenta |
| "Quiero adelantar un pago" | "Listo. Puedes pagar hoy desde $200. Te envié el link de pago." | Consultó tu saldo · Generó un link de pago |
| "No reconozco un cargo" | "Entiendo. Marqué el cargo en revisión y abrí tu caso. Una persona de nuestro equipo te contactará." | Marcó el cargo en revisión · Escaló a un analista humano |

**Patrón que se debe conservar:** el agente **muestra las acciones que ejecutó** ("Consultó tu cuenta"). Esa transparencia es parte de la propuesta de valor y de la trazabilidad.

### Demo de cobranza en la landing (estrategia de contacto por perfil)

| Perfil del deudor | Probabilidad de pago | Estrategia |
|---|---|---|
| "Olvida las fechas" | Alta (82%) | Día 1 WhatsApp, tono amable, recordatorio con link de pago · Día 3 SMS, tono directo, recordatorio de saldo |
| "Ingreso irregular" | Media (55%) | Día 2 WhatsApp, tono empático, plan de pagos en dos cuotas · Día 6 llamada, tono empático, confirmar el plan |
| "Evita el contacto" | Baja (28%) | Día 1 SMS, tono neutral, aviso de saldo · Día 4 email, tono formal, estado de cuenta · Día 8 llamada, tono firme y respetuoso, propuesta de reestructuración |

**Implicación:** la estrategia (canal, día, tono, oferta) **la decide Sora Collect**, el modelo. El agente de cobranza **la ejecuta y conversa**. Esto se formaliza en `[ADR-04]`.

## 5. Cómo funciona (modelo de servicio)

1. **Conectas tus datos:** integramos las fuentes que ya tienes.
2. **Entrenamos con tus datos:** modelos hechos a la medida de tu operación.
3. **Lo consumes por API:** decisiones en tiempo real dentro de tu producto.
4. **Monitoreamos y mejoramos:** seguimiento continuo y reentrenamiento.

## 6. Por qué Sora Valley (credenciales declaradas)

- "Un equipo que ya operó riesgo a escala": modelos de riesgo en BNPL y microcréditos en LatAm.
- Modelos que operan en producción, más allá del prototipo.
- Conocimiento de la región.
- "Tu equipo de ML, ampliado": se integran sin que el cliente tenga que contratar.

## 7. Planes (precios "a conversar")

| Plan | Para | Incluye |
|---|---|---|
| Starter | Lanzar el primer modelo | Un producto de riesgo en producción · Acceso por API · Monitoreo del modelo |
| Growth | Escalar la cartera | Productos de riesgo y crecimiento · Reentrenamiento continuo · Acompañamiento del equipo |
| Scale | Operar a gran volumen | Catálogo completo · **Agentes de IA y MLOps en tu casa** · Equipo dedicado |

**Hoy los agentes solo aparecen en el plan Scale.** Una de las preguntas del modelo comercial (WP09) es si Sora Agents debe venderse también como producto independiente o como puerta de entrada, porque su time-to-value es menor que el de un modelo de riesgo.

## 8. Cliente objetivo (ICP), por inferencia

> Inferencia a partir de la landing. WP01 y WP02 deben validarla o ajustarla.

- Fintechs de **BNPL y microcrédito en Latinoamérica**: los países núcleo de análisis son México, Colombia, Brasil, Perú, Chile y Argentina `[S-04]`, y el resto de LatAm se cubre de forma genérica. Están en etapa temprana o de crecimiento y **no tienen un equipo de ML robusto**.
- Créditos de **ticket bajo y alto volumen**, con pagos semanales, quincenales o mensuales.
- Usuarios **mobile-first y WhatsApp-first** (en toda la región WhatsApp es el canal dominante), muchos sub-bancarizados o con poco historial en buró.
- Operación de **atención y cobranza con equipos pequeños o tercerizados**, presionada por costos.
- **Fintechs con operación en varios países**: un mismo tenant puede tener operaciones en 2 o más países, con regulación, moneda e idioma distintos en cada una. Es un requisito de diseño: tenant → operación por país.
- **Figura regulatoria variable por país** (ejemplos a verificar): SOFOM E.N.R. o IFPE en México; vigiladas o no vigiladas por la SFC en Colombia; SCD/SEP en Brasil; empresas no supervisadas por la SBS en Perú; emisores de tarjetas o prestamistas no bancarios en Chile; proveedores no financieros de crédito registrados ante el BCRA en Argentina `[LEGAL]`. Ver `16-REGULATORIO-Y-RIESGOS-LATAM.md`.

## 9. Lo que la landing NO define (y este paquete debe resolver)

| Hueco | Dónde se resuelve |
|---|---|
| Modelo de precio por uso de agentes (por conversación, por resolución, por minuto, por resultado) | WP09 |
| Agentes de **contenido para redes sociales**: no existen en la landing y son una ampliación pedida | `12-CATALOGO`, WP02, WP04 |
| **Gestión de cartera y cobranza conversacional** completa: la landing la menciona dentro de Sora Collect, pero sin diseño | `12-CATALOGO`, WP04 |
| Canal de **voz**: mencionado, sin definir | WP05 |
| **Builder y consola** de configuración de agentes | WP02, WP04 |
| Infraestructura: Forge es agnóstico a la nube, pero la plataforma de agentes se decide **sobre AWS** | `[ADR-01]`, WP03 |
| Multi-tenancy, seguridad y cumplimiento | WP03, WP06 |
| **Operación multi-país** (regulación, idioma, moneda, pagos, residencia de datos por país) | `13-SUPUESTOS [S-04]`, `16-REGULATORIO`, WP03, WP05, WP06 |

## 10. Tensiones a tener presentes

1. **Forge es agnóstico a la nube; Sora Agents Platform es AWS-nativa.** No es contradictorio: Forge despliega en la nube del cliente y Agents es un servicio gestionado por Sora en su propia cuenta de AWS. Una versión "agentes en la nube del cliente" (silo/BYOC) puede ser una opción premium futura (`[ADR-06]`).
2. **El LLM conversa; los modelos de Sora deciden.** No se debe diluir la propuesta de "decisiones de riesgo con modelos propios" dejando que un LLM decida montos u ofertas (`[ADR-04]`).
3. **El endpoint de la landing es de KYC**, pero el MVP prioriza atención, cobranza y contenido. KYC queda en fase 2 (`11-VISION`). La landing deberá actualizarse cuando se defina el MVP; eso queda fuera de este paquete.
