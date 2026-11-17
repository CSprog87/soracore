# WP08 · Costos de infraestructura y unit economics

| Campo | Valor |
|---|---|
| Oleada | 2 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/` (en especial **13**) y `03-entregables/_GATE-1.md` |
| Insumos de otros WPs | **WP03** (servicios y capacidad), **WP04** (mezcla de modelos y llamadas y tokens por turno), **WP05** (costos de canal por país, voz); WP02 y WP07 opcionales |
| Archivos de salida | `03-entregables/WP08-costos-y-unit-economics.md` **y** `03-entregables/WP08-modelo-costos.csv` |
| Acceso web | **Requerido**: páginas oficiales de precios de AWS (Bedrock, AgentCore, Connect, End User Messaging, etc.) y el rate card de WhatsApp de Meta |
| Herramientas | Se recomienda ejecutar Python para calcular las tablas y generar el CSV |
| Extensión orientativa | 5,000–8,000 palabras más tablas |
| Dueño de | Costo variable por unidad, costo total por escenario, costos fijos de la plataforma, sensibilidad y palancas de optimización |

## Rol
Eres analista de FinOps y unit economics de plataformas SaaS con IA generativa.

## Objetivo
Calcular, con fórmulas explícitas y precios verificados, **cuánto cuesta operar la plataforma** por unidad (conversación, minuto, pieza) y por escenario (ESC-P, ESC-L, ESC-E), qué parte es fija y cuál variable, cómo cambia por país, y qué palancas lo reducen. Es el insumo directo de WP09 (pricing) y WP12 (presupuesto).

## Secciones obligatorias

### 2.1 Hoja de precios verificada
Tabla con cada precio unitario usado: servicio, dimensión, precio, región, **URL oficial y fecha de consulta**. Reemplaza cada `P-xx` de `13-SUPUESTOS` por su valor verificado, o mantén `[VERIFICAR]` si no hay fuente. Incluye:
- Bedrock: Claude Sonnet 5.5, Haiku 5.5 y Opus 5.5 (entrada, salida, lectura y escritura de caché, batch), con la **diferencia entre endpoints en región, geográficos y globales** si existe.
- Los componentes de AgentCore usados por WP03 y WP04.
- WhatsApp por **país núcleo** y categoría (Meta más el proveedor elegido en `[OD-05]`).
- Voz por país (Connect, telefonía, motor elegido en `[OD-01]`).
- Generación de imágenes, Guardrails, Knowledge Bases y vector store (`[OD-02]`), Step Functions, EventBridge, Lambda, API Gateway, DynamoDB, S3, CloudWatch, KMS, Secrets Manager, NAT y endpoints, WAF y servicios de seguridad.
- Diferencias de precio entre `sa-east-1`, `mx-central-1` y `us-east-1` para los servicios principales.

### 2.2 Modelo de costo por unidad
Para cada unidad, desglose con fórmula y resultado:
- **Conversación AG-ATC** (chat): router + turnos × llamadas × tokens (con caché, incluido el **caché incremental del historial**) + guardrails + RAG + runtime y memoria + Gateway + almacenamiento + telemetría + evaluación en línea prorrateada + canal (mensajes de WhatsApp de servicio en ventana).
- **Conversación AG-COB** (chat): lo mismo, más las plantillas salientes prorrateadas por conversación.
- **Plantilla saliente sin LLM** (preventiva).
- **Minuto de voz AG-COB** (con el motor elegido y su alternativa).
- **Pieza de contenido AG-CON** (copy + imágenes + revisión + publicación), interactiva y en batch.
Presenta cada unidad con **mezcla base** (`[S-30]`…`[S-35]`) y **mezcla económica**, y la diferencia.

### 2.3 Costos fijos de la plataforma
Costo mensual base por entorno (dev, staging, prod) y por celda, independiente del volumen: NAT y endpoints, vector store con mínimos, números telefónicos, WAF, servicios de seguridad, logs base, consola, CI/CD y herramientas de observabilidad y evals (WP07). Explica qué se comparte entre tenants y qué escala por tenant (por ejemplo, la KMS por tenant y las bases de conocimiento).

### 2.4 Costo total por escenario
Para ESC-P, ESC-L y ESC-E: tabla por servicio y por agente, con el total mensual, el **costo por tenant** y el **costo por conversación promedio**. Agrega un `pie` de Mermaid con la distribución de costos para ESC-L. Presenta el escenario **con voz y sin voz**.

### 2.5 Variación por país
Cuánto cambia el costo por unidad según el país núcleo (WhatsApp y voz son los grandes diferenciales). Tabla por país para AG-COB y AG-ATC.

### 2.6 Análisis de sensibilidad
Mínimo estas variables, cada una a −50%, base y +50% (o en rangos razonables): tokens por llamada, llamadas por turno, tasa de acierto del caché, mezcla Sonnet/Haiku, turnos por conversación, precio de WhatsApp por país, minutos de voz y tasa de evaluación en línea. Tabla tipo tornado (ordenada por impacto).

### 2.7 Palancas de optimización
Cuantifica cada palanca (ahorro estimado en ESC-L): prompt caching bien diseñado, ruteo a Haiku, effort bajo en chat, batch para contenido y evals, recorte o compactación del historial, deduplicación de RAG, plantillas utility dentro de la ventana de servicio, Savings Plans o capacidad reservada si aplica, lifecycle de S3 y muestreo de trazas. Ordénalas por ahorro sobre esfuerzo.

### 2.8 Costo de la nube durante el build
Costo mensual estimado de los entornos durante las semanas de construcción (para WP12), incluidos los tokens de desarrollo y de evals.

### 2.9 Controles de costo
Presupuestos y alarmas de AWS, límites por tenant (tokens y conversaciones), detección de anomalías, etiquetado de costos (`tenant_id`, `agent`, `env`) y asignación de costos compartidos a tenants (método de prorrateo para el margen por tenant).

### 2.10 Resumen para WP09
Tabla final: **costo variable por unidad** (base y económico) y **costo fijo mensual por escenario**, que WP09 usará directamente.

### Archivo CSV
`WP08-modelo-costos.csv` con una fila por concepto de costo y columnas: `escenario, agente, unidad, concepto, servicio, driver, cantidad_mensual, precio_unitario_usd, fuente_precio, costo_mensual_usd, notas`. Debe permitir recalcular en una hoja de cálculo.

## Diagramas obligatorios
1. `pie` de la distribución de costos (ESC-L). 2. `flowchart` del árbol de costo de una conversación AG-COB (de qué se compone).

## Criterios de aceptación
- [ ] Cada precio con URL y fecha, o `[VERIFICAR]` justificado.
- [ ] Costo por unidad para las 5 unidades, con fórmula visible y mezcla base y económica.
- [ ] Costos fijos por entorno y celda.
- [ ] Totales por escenario, con voz y sin voz, por tenant y por conversación.
- [ ] Variación por país núcleo.
- [ ] Sensibilidad tipo tornado.
- [ ] Palancas cuantificadas.
- [ ] CSV generado y consistente con las tablas del markdown.
- [ ] Usa los valores canónicos de `_GATE-1.md` (modelos, llamadas por turno, servicios). Cualquier desviación está declarada.

## Fuera de alcance
Precios al cliente (WP09) y costo del equipo humano (WP12), salvo el costo de la nube durante el build.
