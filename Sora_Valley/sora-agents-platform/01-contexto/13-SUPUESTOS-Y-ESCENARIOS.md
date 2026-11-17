# 13 · Supuestos y escenarios (fuente única de números)

> **Regla:** todo WP que necesite un número lo toma de aquí y cita su ID (`[S-xx]` o `[P-xx]`). Si necesita otro valor, lo declara como `[S-NUEVO-n]`, y el revisor de gate decide si pasa a canónico.
> Estos valores son **hipótesis de trabajo razonables**, no datos de clientes. Su propósito es que los trece WPs calculen con la misma base. El piloto los reemplazará con datos reales.

---

## A. Mercado y alcance geográfico

| ID | Supuesto | Valor |
|---|---|---|
| S-01 | Cliente tipo | Fintech de BNPL o microcrédito en LatAm con 50k–500k cuentas activas; ticket promedio de USD 80–450; pagos semanales, quincenales o mensuales; base de usuarios mobile-first y WhatsApp-first |
| S-02 | Tipos de cambio indicativos `[VERIFICAR]` | USD/MXN 18.5 · USD/COP 4,000 · USD/BRL 5.5 · USD/PEN 3.7 · USD/CLP 950 · USD/ARS: no usar (volatilidad); cotizar en USD |
| S-03 | Idiomas del producto | Español con variantes por país (es-MX, es-CO, es-PE, es-CL, es-AR con voseo, es-419 genérico) y portugués de Brasil (pt-BR) |
| S-04 | Países núcleo de análisis | **MX, CO, BR, PE, CL, AR.** El resto de LatAm (EC, CR, PA, GT, DO, UY, PY, BO, SV, HN, VE y otros) se trata como `LATAM-OTROS`, con configuración genérica y revisión legal caso por caso. |
| S-05 | País del piloto | **Por decidir** `[OD-11]`. Los cálculos de ESC-P usan "país hispanohablante genérico" y presentan la variación por país en tabla. |
| S-06 | Zonas horarias | De UTC-3 a UTC-6. Toda regla horaria se evalúa en la **zona horaria del usuario final**, no en la del tenant ni en la de Sora. |
| S-07 | Estructura del tenant | Un tenant es una fintech (grupo). Puede tener 1 a N **operaciones de país**, cada una con su número de WhatsApp, regulación, moneda, idioma, PSP e integraciones propias. |

## B. Escenarios de volumen

| ID | Variable | ESC-P · Piloto | ESC-L · Lanzamiento | ESC-E · Escala |
|---|---|---|---|---|
| S-08 | Horizonte | Meses 1–3 tras el go-live | Meses 4–12 | Año 2 |
| S-09 | Tenants activos / operaciones de país | 1 / 1 | 3 / 4 (2–3 países hispanohablantes) | 10 / 14 (≥ 5 países, incluido Brasil con ~25% del volumen) |
| S-10 | AG-ATC: conversaciones por mes | 10,000 | 60,000 | 300,000 |
| S-11 | Cuentas activas totales | 50,000 | 300,000 | 1,500,000 |
| S-12 | AG-COB: cuentas en gestión por mes (D-5 a D+90) | 5,000 | 30,000 | 150,000 |
| S-13 | AG-COB: plantillas salientes por mes (utility, sin LLM) | 40,000 | 240,000 | 1,200,000 |
| S-14 | AG-COB: conversaciones con LLM por mes (respuestas e inbound) | 6,000 | 36,000 | 180,000 |
| S-15 | AG-COB: minutos de voz por mes (si se habilita, `[OD-01]`) | 5,000 | 40,000 | 250,000 |
| S-16 | AG-CON: marcas activas / piezas de copy por mes | 1 / 100 | 3 / 300 | 10 / 1,000 |
| S-17 | AG-CON: imágenes generadas por mes (incluye descartes) | 200 | 600 | 2,000 |
| S-18 | Usuarios de consola (operadores del tenant y de Sora) | 10 | 40 | 150 |
| S-19 | Tasa de handoff a humano (ATC / COB) | 15% / 20% | 12% / 18% | 10% / 15% |

## C. Forma de la conversación (para el cálculo de tokens)

| ID | Parámetro | AG-ATC | AG-COB chat | AG-COB voz | AG-CON (por pieza) |
|---|---|---|---|---|---|
| S-20 | Turnos del usuario por conversación | 4 | 5 | 8 por llamada (3.5 min promedio) | n/a |
| S-21 | Llamadas LLM por turno (incluye loops de herramientas) | 2.0 | 2.5 | 1.5 | 3 por pieza (plan, copy, autocrítica) |
| S-22 | Tokens de entrada por llamada · prefijo estable cacheable (system, herramientas, políticas del tenant) | 5,000 | 6,000 | 4,000 | 8,000 |
| S-23 | Tokens de entrada por llamada · parte variable (historial nuevo, contexto, resultados de herramientas), promedio | 3,000 | 3,500 | 2,500 | 6,000 |
| S-24 | Tokens de salida por llamada (incluye razonamiento) | 350 | 400 | 150 | 2,500 |
| S-25 | Tasa de acierto del caché sobre el prefijo estable | 85% | 85% | 85% | 50% |
| S-26 | Llamadas de AG-RTR por mensaje entrante | 1 llamada: 1,500 tokens de entrada y 50 de salida (Haiku) | idem | n/a | n/a |
| S-27 | Consultas RAG por conversación | 1.5 | 0.5 | 0.5 | 1 |
| S-28 | Evaluación en línea (LLM-as-judge) | 10% de las conversaciones; 8,000 tokens de entrada y 500 de salida por evaluación (Haiku) | idem | idem | 100% de las piezas |
| S-29 | Guardrails evaluados por turno | Entrada y salida | Entrada y salida | Salida | Salida |

> **Nota para WP08:** S-23 es conservador. En un loop multi-turno, el historial anterior también se sirve desde el caché de forma incremental. WP08 debe modelar ese efecto y presentar la diferencia.

## D. Mezcla de modelos (base `[ADR-02]`)

| ID | Uso | Modelo base | Variante económica que se debe evaluar |
|---|---|---|---|
| S-30 | Router y clasificación (AG-RTR) | Claude Haiku 5.5 | — |
| S-31 | Conversación AG-ATC | Claude Sonnet 5.5 | Haiku 5.5 en el 60% de los turnos (FAQs y consultas simples) y Sonnet en el resto |
| S-32 | Conversación AG-COB | Claude Sonnet 5.5 | Haiku 5.5 en preventiva; Sonnet en mora |
| S-33 | AG-CON: copy y plan | Claude Sonnet 5.5 (procesamiento por lotes cuando no sea interactivo) | Opus 5.5 solo para el plan editorial mensual |
| S-34 | Resúmenes de handoff, LLM-as-judge, etiquetado | Claude Haiku 5.5 | — |
| S-35 | Voz | Según `[OD-01]`: speech-to-speech (Amazon Nova Sonic u otro) o pipeline STT → LLM → TTS | — |

## E. Plataforma y operación

| ID | Supuesto | Valor |
|---|---|---|
| S-36 | Factor de pico | El 60% del tráfico diario ocurre en 8 horas. Pico horario = 2.5 × el promedio horario. Los días de pago (quincena o fin de mes) suman 2× el volumen diario. |
| S-37 | Disponibilidad objetivo | MVP 99.5% · Escala 99.9% |
| S-38 | Latencias objetivo (p90) | Primera respuesta por WhatsApp ≤ 6 s · primer token en web chat ≤ 1.5 s · respuesta de voz por turno ≤ 1.5 s · router ≤ 0.8 s |
| S-39 | Retención | Conversaciones "calientes" 90 días; archivo de transcripciones y bitácora de gestiones de cobranza 5 años; grabaciones de voz 2 años. **Todo depende del país** `[LEGAL]`. |
| S-40 | Entornos | dev, staging y prod. Más un sandbox por tenant durante el onboarding (lógico, no una cuenta separada). |
| S-41 | Horario de soporte de Sora | Lunes a viernes, 8:00–20:00 en la zona horaria de cada tenant. Guardia 24/7 para incidentes Sev-1 desde ESC-L. |
| S-42 | Tamaño de la base de conocimiento por operación de país | Unos 500 documentos y unos 5,000 chunks |
| S-43 | Mensajes por conversación de WhatsApp | ATC: unos 9 mensajes (4 del usuario y 5 del agente) · COB: unos 11 |
| S-44 | Payload promedio por mensaje almacenado (con metadatos y trazas) | 4 KB de mensaje + 40 KB de traza por turno |
| S-45 | Objetivo de time-to-live de un tenant nuevo | ESC-L ≤ 4 semanas · ESC-E ≤ 2 semanas desde la firma |

## F. Equipo y costos internos (para estimar el build)

> Costo mensual **cargado** (salario, cargas e impuestos, o tarifa de contractor) de talento senior **distribuido en LatAm**. Son valores de referencia que el equipo de Sora Valley debe ajustar `[PREGUNTA-ABIERTA]`.

| ID | Rol | USD por mes |
|---|---|---|
| S-50 | Tech Lead / Arquitecto | 9,000 |
| S-51 | Ingeniero senior backend / de agentes | 7,000 |
| S-52 | Ingeniero de plataforma / DevOps / SRE | 7,000 |
| S-53 | Ingeniero ML / LLM (evals, RAG, prompts) | 7,500 |
| S-54 | FDE / Solutions engineer | 6,500 |
| S-55 | Diseñador conversacional | 4,500 |
| S-56 | QA / Eval engineer | 4,500 |
| S-57 | Product manager | 6,500 |
| S-58 | Diseñador UX/UI (parcial) | 4,500 |
| S-59 | Seguridad y cumplimiento (fraccional) más asesoría legal por país | 3,000 + presupuesto legal por país `[VERIFICAR]` |
| S-60 | Contingencia sobre la estimación de build | 20% |
| S-61 | Objetivo de calendario | Piloto en producción con 1 cliente de diseño en **≤ 16 semanas** desde el kickoff |

## G. Objetivos de negocio (hipótesis que WP09 debe validar)

| ID | Objetivo | Valor |
|---|---|---|
| S-65 | Margen bruto objetivo | ≥ 65% en ESC-L · ≥ 75% en ESC-E |
| S-66 | Contención de AG-ATC | ≥ 60% en el piloto → ≥ 75% en escala |
| S-67 | Uplift de recuperación de AG-COB contra el holdout | Hipótesis: +10% a +20% relativo (se mide en el piloto) |
| S-68 | Costo de un agente humano de atención o cobranza en LatAm (para la historia de ROI) | USD 600–1,200 por mes cargado según el país `[VERIFICAR por país]` |

---

## H. Precios de referencia (P-xx)

> **Uso:** solo si no puedes verificar en la fuente oficial. Todo valor de esta tabla se cita con `[VERIFICAR]`. Los precios de Claude son los de **la API de Anthropic (first-party)** al 3-nov-2026. **Los precios en Amazon Bedrock se publican aparte** y pueden diferir, también según el tipo de endpoint (en región, geográfico o global). WP08 debe confirmarlos en la página de precios de Bedrock.

| ID | Concepto | Valor de referencia | Unidad | Estado y notas |
|---|---|---|---|---|
| P-01 | Claude Sonnet 5.5 · entrada / salida | 2.00 / 10.00 | USD por MTok | API 1P. Bedrock `[VERIFICAR]`. |
| P-02 | Claude Sonnet 5.5 · lectura de caché | 0.10 | USD por MTok | API 1P `[VERIFICAR]` |
| P-03 | Escritura de caché | 1.25× el precio de entrada (TTL 5 min) · 2× (TTL 1 h) | — | API 1P `[VERIFICAR]` |
| P-04 | Claude Haiku 5.5 · entrada / salida (prompts ≤ 100K tokens) | 0.10 / 0.50 | USD por MTok | API 1P. Por encima de 100K: 0.50 / 2.50 `[VERIFICAR]` |
| P-05 | Claude Opus 5.5 · entrada / salida / lectura de caché | 4.00 / 20.00 / 0.20 | USD por MTok | API 1P `[VERIFICAR]` |
| P-06 | Descuento por procesamiento por lotes (batch) | 50% | — | API 1P. En Bedrock, verificar batch inference `[VERIFICAR]`. |
| P-07 | Diferencia de precio entre inferencia en región o geográfica y global en Bedrock | Podría existir un sobreprecio (~10%) en los endpoints regionales o geográficos | — | `[VERIFICAR]` |
| P-08 | AgentCore Runtime (cómputo activo) | ~0.0895 por vCPU-hora · ~0.00945 por GB-hora | USD | Fuente secundaria `[VERIFICAR]` |
| P-09 | AgentCore Memory, Gateway, Identity, Policy, Evaluations, Observability | n/d | Por componente | Ver la página oficial de precios de AgentCore `[VERIFICAR]` |
| P-10 | WhatsApp (Meta) · plantillas utility y authentication | **Varía por país.** Orden de magnitud: 0.001–0.03 por mensaje | USD | Modelo por mensaje desde julio de 2025. Las respuestas de servicio dentro de la ventana de 24 h no tienen cargo de Meta, y las utility dentro de la ventana tampoco `[VERIFICAR en el rate card de Meta por país]`. |
| P-11 | WhatsApp (Meta) · plantillas marketing | **Varía por país.** Orden de magnitud: 0.01–0.09 por mensaje | USD | `[VERIFICAR en el rate card por país]` |
| P-12 | Cargo de AWS End User Messaging Social por mensaje (adicional al de Meta) | ~0.005 | USD por mensaje | `[VERIFICAR]` |
| P-13 | Amazon Connect · uso de servicio de voz | ~0.018 | USD por minuto | Más telefonía por país (entrante, saliente y número por día) `[VERIFICAR por país]` |
| P-14 | Amazon Nova Sonic (speech-to-speech) | n/d | USD por cada 1K tokens de voz | `[VERIFICAR]`, incluida la disponibilidad en español y portugués |
| P-15 | Amazon Transcribe streaming | ~0.024 | USD por minuto (tier 1) | `[VERIFICAR]` |
| P-16 | Amazon Polly · neural | ~16 | USD por millón de caracteres | Las voces generative cuestan más `[VERIFICAR]` |
| P-17 | Generación de imágenes en Bedrock (por ejemplo, Amazon Nova Canvas) | ~0.04–0.08 | USD por imagen | `[VERIFICAR modelo, disponibilidad y precio]` |
| P-18 | Amazon Bedrock Guardrails (filtros de contenido y temas denegados) | ~0.15 | USD por cada 1,000 text units, por política | `[VERIFICAR]` |
| P-19 | Vector store | OpenSearch Serverless tiene un **costo mínimo fijo mensual relevante** por OCUs; S3 Vectors y Aurora pgvector son alternativas de menor costo base | — | `[VERIFICAR]` (`[OD-02]`) |
| P-20 | CloudWatch Logs · ingestión | ~0.50 | USD por GB | `[VERIFICAR]` |
| P-21 | Precios de servicios AWS en `sa-east-1` y `mx-central-1` frente a `us-east-1` | São Paulo suele costar más que us-east-1 | — | `[VERIFICAR por servicio]` |

### Fórmula de referencia del costo LLM por llamada

```
costo_llamada (USD) = [ (tokens_prefijo × tasa_acierto_cache × precio_lectura_cache)
                       + (tokens_prefijo × (1 − tasa_acierto_cache) × precio_escritura_cache)
                       + (tokens_variables × precio_entrada)
                       + (tokens_salida × precio_salida) ] / 1,000,000
```

WP08 debe refinarla con el caché incremental del historial y con los guardrails, el RAG y las evaluaciones por conversación.
