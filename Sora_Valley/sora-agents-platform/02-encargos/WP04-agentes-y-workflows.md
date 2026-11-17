# WP04 · Diseño de agentes y workflows agénticos

| Campo | Valor |
|---|---|
| Oleada | 1 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, en especial 12, 13, 14 y 15 |
| Insumos de otros WPs | Ninguno (oleada 1) |
| Archivo de salida | `03-entregables/WP04-agentes-y-workflows.md` |
| Acceso web | Recomendado: documentación de AgentCore, Strands Agents, Bedrock Guardrails, Knowledge Bases y Claude en Bedrock |
| Extensión orientativa | 9,000–13,000 palabras más diagramas y esquemas. Es el WP más técnico del paquete. |
| Dueño de | `[OD-03]` framework; diseño de AG-RTR, AG-ATC, AG-COB y AG-CON; workflows W-01 a W-08; esquema de configuración de agentes; mezcla de modelos y llamadas por turno (insumo directo de WP08) |

## Rol
Eres ingeniero principal de sistemas agénticos con LLMs: diseño de agentes con herramientas, RAG, guardrails, evals y operación en producción en entornos regulados.

## Objetivo
Especificar **cómo funciona por dentro** cada agente y cada workflow, con detalle suficiente para que un equipo empiece a construir: patrón de orquestación, estados, secuencias, prompts (estructura), herramientas (esquemas), memoria, conocimiento, políticas, guardrails, escalamiento, modelos, caché, presupuestos, errores y configuración multi-tenant y multi-país.

## Secciones obligatorias

### 2.1 Patrón de orquestación global
- Router (AG-RTR) + agentes especialistas + workflows deterministas. **Criterios explícitos para decidir "workflow o agente"** en cada caso.
- **Handover entre agentes** en el mismo número (ATC ↔ COB): quién es dueño de la conversación, cómo se transfiere el estado y cómo se evita el "ping-pong".
- Cómo un workflow invoca a un agente y cómo un agente dispara un workflow (eventos).
- Diagrama general.

### 2.2 Framework y runtime (`[OD-03]`, `[ADR-03]`)
Compara Strands Agents, el Tool Runner del SDK de Anthropic y LangGraph sobre AgentCore Runtime: control del loop, soporte de MCP y Gateway, streaming, observabilidad (OTEL), testabilidad, manejo de memoria, madurez y curva. Compara también AgentCore Runtime con ECS Fargate propio (control, costo, cold start, aislamiento por sesión). **Recomendación.**

### 2.3 Especificación por agente
Para **AG-RTR, AG-ATC, AG-COB y AG-CON** (y un bosquejo de una página para AG-KYC y AG-COP), con estas subsecciones:

a. **Objetivo, límites y no-objetivos.**
b. **`stateDiagram-v2`** del ciclo de la conversación o tarea, incluidos estados de autenticación (L0/L1/L2), espera, escalamiento y cierre.
c. **Diagramas de secuencia** de los 3 a 5 flujos principales (AG-COB: respuesta a recordatorio con link de pago; promesa de pago; plan de pagos con L2; "no soy esa persona"; "ya pagué". AG-ATC: consulta de cuenta; cargo no reconocido; razones de rechazo; queja formal. AG-CON: brief → pieza aprobada → publicada; rechazo por cumplimiento).
d. **Estructura del system prompt:** secciones (rol, reglas no negociables, política del país inyectada, tono, uso de herramientas, formato de salida, escalamiento), qué parte es **fija y cacheable** y qué parte es **variable por tenant y país**. Incluye un **esqueleto de ejemplo en español** de AG-COB, de 60 a 120 líneas, con marcadores `{{variable}}`.
e. **Herramientas:** para cada T-xx que use el agente, un esquema JSON (input y output), clase de riesgo, autorización requerida, idempotencia (llave), timeout y reintentos, y el error que devuelve al LLM. Los esquemas deben ser **consistentes** con el Contrato Mínimo de Integración que diseñará WP05: usa nombres de campos neutrales y documenta tus supuestos.
f. **Memoria:** qué va a corto plazo (sesión) y a largo plazo (preferencias, resúmenes, promesas), los namespaces por tenant, país y cliente, el TTL, y **qué NUNCA se guarda en memoria** (por ejemplo, saldos: siempre se consultan en vivo).
g. **Conocimiento (RAG):** fuentes, chunking, metadatos (tenant, país, idioma, vigencia), filtros obligatorios, citas, frescura y manejo de documentos contradictorios.
h. **Guardrails:** configuración de Bedrock Guardrails (temas denegados por agente, filtros, PII, grounding contextual) y validadores propios (montos contra resultados de herramientas, ventanas horarias, no revelar datos a terceros, idioma correcto).
i. **Políticas deterministas (C-13):** al menos **10 reglas de ejemplo** en Cedar o pseudo-código. Por ejemplo: plan permitido según DPD y saldo; promesa máxima de N días; descuento máximo por tramo; contactos máximos por semana **según el país**; ventana horaria por país en la zona horaria del deudor; bloqueo de T-25 sin aprobación. Explica cómo se cargan por tenant y país.
j. **Escalamiento:** disparadores, **esquema JSON del payload de handoff**, prioridad y SLA, y qué ve la persona.
k. **Modelo por paso:** modelo (Haiku, Sonnet u Opus), effort, max tokens y modo de razonamiento por paso. **Tabla de llamadas LLM por turno** y de tokens estimados (confirma o ajusta `[S-21]`…`[S-24]`; WP08 la usará).
l. **Prompt caching:** dónde van los breakpoints, cómo evitar invalidaciones (sin timestamps ni IDs en el prefijo, herramientas en orden determinista) y el TTL recomendado según el patrón de tráfico.
m. **Errores y fallbacks:** fallo de una herramienta, timeout del modelo, refusal, guardrail bloqueado, integración caída, mensaje duplicado, usuario que responde días después (ventana de 24 h vencida).
n. **Presupuesto por conversación:** turnos máximos, tokens y llamadas a herramientas, y qué hace el agente al excederlos.

### 2.4 Workflows deterministas W-01 a W-08
Para cada uno: objetivo, disparador, diagrama (`flowchart` o `stateDiagram-v2` que represente la máquina de Step Functions), pasos, puntos donde invoca un agente, esperas (task tokens, temporizadores), reglas de país aplicadas, eventos emitidos, manejo de errores e idempotencia. Detalla en especial:
- **W-01 Cadencia de cobranza:** integración con next-action, validaciones de ventana, frecuencia, opt-out y listas de exclusión por país, selección de plantilla, espera de respuesta, reprogramación.
- **W-04 Pipeline de contenido:** generación, revisión automática, aprobación humana de 1 o 2 niveles con task token, publicación, medición y reintentos.
- **W-07 Onboarding de un tenant u operación de país:** qué se aprovisiona automáticamente (KMS, namespaces, KB, secretos, configuración, plantillas) y qué es manual.

### 2.5 Esquema de configuración de agentes (C-32)
- Esquema completo en YAML **comentado**: tenant → operación de país → agente. Incluye persona, idioma y variante, tono por segmento, intents habilitados, herramientas, políticas (referencias a C-13), horarios, plantillas, umbrales de escalamiento, modelo y effort, límites de presupuesto, configuración de memoria y de RAG, y canales.
- Un ejemplo completo para un tenant ficticio con dos operaciones de país (una en español y otra en pt-BR).
- **Versionado y promoción** (dev → staging → prod) ligados a los gates de evals; rollback.

### 2.6 Voz (`[OD-01]`, diseño técnico)
Para las dos arquitecturas candidatas (speech-to-speech frente a STT → LLM → TTS): presupuesto de latencia por componente, turn-taking y barge-in, confirmación de montos y fechas por voz, DTMF como respaldo, aviso de grabación, manejo de ruido y de acentos, español y portugués. Recomendación técnica (WP05 decide junto con canales y costos).

### 2.7 Trazas de ejemplo
Tres conversaciones completas, en JSON o en formato legible, con cada llamada al modelo, cada llamada a herramienta y sus resultados, validaciones de política y eventos emitidos:
1. AG-ATC: consulta de próxima cuota y link de pago (L1).
2. AG-COB: respuesta a un recordatorio → no puede pagar todo → promesa de pago (con reglas de país).
3. AG-CON: brief → pieza → rechazo por omitir el costo total del crédito → corrección → aprobación → programación.

### 2.8 Seguridad de los agentes
Mitigaciones concretas contra prompt injection (directa e indirecta vía KB o resultados de herramientas), excessive agency, exfiltración de datos, account takeover conversacional y manipulación para obtener descuentos. Enlaza con WP06 sin duplicarlo.

### 2.9 Guía de implementación con Claude en Bedrock
Notas prácticas a partir de `[ADR-02]`: `tool_choice` (sin forzado en Sonnet y Opus 5.5, usar `strict`), effort por paso, razonamiento, structured outputs, manejo de refusal, caché y batch. Marca `[VERIFICAR]` lo que dependa de la documentación vigente.

## Diagramas obligatorios (mínimo 20)
Orquestación general (1) · `stateDiagram-v2` por agente (4) · secuencias por agente (≥ 12 en total) · W-01 a W-08 (8) · voz (1).

## Criterios de aceptación
- [ ] Cada T-xx de `12-CATALOGO` tiene esquema de entrada y salida, clase, autorización e idempotencia.
- [ ] Cada agente del MVP tiene stateDiagram, ≥ 3 secuencias, esqueleto de prompt (al menos el de AG-COB completo), memoria, RAG, guardrails, escalamiento y presupuesto.
- [ ] **Ninguna acción W-alto puede ejecutarse sin una política determinista**, demostrado en los diagramas.
- [ ] Hay ≥ 10 reglas de política, incluidas al menos 3 que varían por país.
- [ ] W-01 a W-08 están diagramados y descritos.
- [ ] El YAML de configuración está completo y hay un ejemplo multi-país.
- [ ] La tabla de llamadas LLM y tokens por turno está lista para WP08.
- [ ] `[OD-03]` está resuelta.
- [ ] Hay 3 trazas completas.

## Fuera de alcance
Selección de servicios de infraestructura (WP03), proveedores de canal (WP05), costos en USD (WP08) y evals detalladas (WP07). Aquí solo se definen los puntos de enganche.
