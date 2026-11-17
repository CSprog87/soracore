# WP07 · Observabilidad, evaluación y calidad de agentes

| Campo | Valor |
|---|---|
| Oleada | 2 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/` y `03-entregables/_GATE-1.md` |
| Insumos de otros WPs | **WP02** (métricas, intents, RNF), **WP03** (servicios, CI/CD), **WP04** (agentes, herramientas, workflows) |
| Archivo de salida | `03-entregables/WP07-observabilidad-y-evals.md` |
| Acceso web | Opcional: AgentCore Observability y Evaluations, convenciones semánticas de OpenTelemetry para GenAI |
| Extensión orientativa | 5,000–8,000 palabras |
| Dueño de | Telemetría, dashboards, alertas, SLOs, framework de evals, gates de release, QA humano, red teaming y herramienta de evals |

## Rol
Eres ingeniero de confiabilidad y calidad de sistemas con LLMs (LLMOps): telemetría, evals offline y en línea, QA conversacional y red teaming.

## Objetivo
Definir cómo Sora **sabe en todo momento** si cada agente, en cada tenant y cada país, funciona bien, cuesta lo esperado y cumple. También, cómo se impide que un cambio empeore la calidad.

## Secciones obligatorias

### 2.1 Modelo de telemetría
- Jerarquía de trazas: conversación → turno → llamada al modelo → herramienta → validación de política → evento.
- Atributos obligatorios en cada span: `tenant_id`, `country_op_id`, `agent`, `agent_config_version`, `prompt_version`, `model_id`, tokens (de entrada, de caché leídos y escritos, de salida), latencia, resultado y costo estimado. Alineado con las convenciones de OpenTelemetry para GenAI `[VERIFICAR]`.
- **Redacción de PII** antes de persistir (alineada con WP06).
- Flujo: AgentCore Observability y CloudWatch → lake (C-26). Retención por tipo.

### 2.2 KPIs formales y su instrumentación
Toma las definiciones de WP02 §2.8 y define para cada KPI: los eventos de origen, la consulta o transformación, la frecuencia y la segmentación. Incluye los KPIs de costo (por conversación, por tenant, por agente y por modelo) para alimentar el margen (WP09).

### 2.3 Dashboards
Especifica los widgets de: (1) Operación en vivo (Sora Ops); (2) Calidad por agente; (3) Negocio por tenant (visible al cliente en la consola); (4) Costos y margen por tenant; (5) Cumplimiento (violaciones de política, quejas, opt-outs y contactos fuera de ventana, que deben ser cero). Para cada uno: audiencia, métricas, cortes y fuente.

### 2.4 SLOs y alertas
Tabla de SLOs (disponibilidad, latencias por canal, tasa de error de herramientas, éxito de entrega de WhatsApp). Tabla de **alertas**: condición, umbral, severidad, a quién y runbook. Incluye alertas de **costo anómalo** por tenant, de **caída de la calidad de WhatsApp**, de **aumento de escalamientos** y de **drift de intents**.

### 2.5 Framework de evaluación offline
- **Eval sets por agente, intent y país:** composición (casos felices, bordes, negativos y adversariales), tamaño mínimo para el MVP (propón uno: por ejemplo, ≥ 300 casos por agente, con ≥ 30 por país activo), cómo se construyen (sintéticos, de transcripciones anonimizadas, escritos por diseñadores conversacionales) y cómo se mantienen.
- **Simulador de usuarios:** un LLM que interpreta personas (deudor que evita, enojado, vulnerable, tercero, intento de fraude) para pruebas multi-turno, con el mismo idioma y variante del país.
- **Calificación:** checks deterministas (montos iguales al resultado de la herramienta, herramienta correcta, política respetada, no revelar a terceros, idioma correcto, disclosure de IA) más **LLM-as-judge** con rúbricas. Escribe **3 rúbricas completas** (cumplimiento de cobranza, resolución en atención, calidad y cumplimiento de contenido), con escalas y ejemplos.
- Calibración del juez frente a la persona (acuerdo mínimo exigido).

### 2.6 Gates de release
Qué se evalúa ante cada tipo de cambio (prompt, configuración de tenant, modelo, herramienta o KB), umbrales mínimos para promover (por ejemplo, 0 violaciones críticas, ≥ X% de éxito por intent, no regresión mayor a Y), y cómo se integra al pipeline de WP03. Diagrama.

### 2.7 Evaluación en línea y QA humano
Muestreo (`[S-28]`), qué evalúa el juez en producción, cola de revisión humana (por ejemplo, el 2% de las conversaciones más el 100% de las marcadas), calibración semanal, ciclo de retroalimentación (CSAT, motivos de escalamiento, quejas) hacia el backlog de mejoras.

### 2.8 Experimentación
Pruebas A/B de prompts y configuraciones, holdouts de cobranza para medir el uplift (`[S-67]`), coordinación con los experimentos de Sora Collect y Audience, y guardrails estadísticos.

### 2.9 Red teaming
Plan inicial y recurrente: prompt injection (directa e indirecta), jailbreaks, ingeniería social para obtener datos o descuentos, account takeover, abuso de herramientas, contenido de marketing riesgoso. Frecuencia, responsables y criterios de salida.

### 2.10 Herramientas
AgentCore Observability y Evaluations frente a complementos (Langfuse self-hosted, Braintrust, Arize u otros). Criterios: costo, residencia de datos (las trazas contienen datos personales), integración y funcionalidades. **Recomendación** y costo estimado para WP08.

## Diagramas obligatorios (mínimo 4)
1. Flujo de telemetría (de los componentes al lake y a los dashboards). 2. Pipeline de evals offline y gates. 3. Ciclo de evaluación en línea y QA humano. 4. Ciclo de mejora continua (de la señal al cambio desplegado).

## Criterios de aceptación
- [ ] Atributos de span definidos, incluidos tenant, país, versión y costo.
- [ ] Todos los KPIs de WP02 instrumentados.
- [ ] 5 dashboards especificados.
- [ ] Tablas de SLOs y alertas con umbrales y runbooks.
- [ ] Eval sets dimensionados por agente y país; 3 rúbricas completas.
- [ ] Gates de release con umbrales numéricos.
- [ ] Plan de red teaming.
- [ ] Recomendación de herramientas con costo para WP08.

## Fuera de alcance
Infraestructura base (WP03) y lógica de agentes (WP04).
