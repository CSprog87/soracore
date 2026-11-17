# WP01 · Benchmark competitivo: Wonderful y comparables

| Campo | Valor |
|---|---|
| Oleada | 1 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, en especial 10, 11 y 12 |
| Insumos de otros WPs | Ninguno |
| Archivo de salida | `03-entregables/WP01-benchmark-mercado.md` |
| Acceso web | **Requerido.** Sin web, entrega con `[VERIFICAR]` en todo dato de mercado y limita el alcance a lo que sepas con certeza. |
| Extensión orientativa | 3,000–5,000 palabras más tablas |
| Dueño de | `[OD-11]`, recomendación de mercado del país piloto (la parte regulatoria la aporta WP06) |

## Rol
Eres analista de estrategia y producto, especializado en plataformas de agentes de IA y fintech en LatAm.

## Objetivo
Entender cómo funcionan, se venden y se cobran las plataformas de agentes como servicio (Wonderful y comparables) para definir **qué debe tener Sora Agents en su MVP, cómo posicionarse y cómo cobrar**. Además, aportar la visión de **mercado por país en LatAm** para elegir dónde pilotear.

## Preguntas que debes responder
1. ¿Qué es exactamente Wonderful? Producto, canales, idiomas, arquitectura conocida, Agent Builder, integraciones, modelo de despliegue con equipos locales, verticales, clientes públicos, pricing conocido, financiamiento y presencia en LatAm.
2. ¿Qué otros jugadores globales y latinoamericanos venden agentes de IA de cara al cliente y en qué se diferencian?
3. ¿Quién hace **cobranza con IA** (WhatsApp o voz) y **atención para fintech** en LatAm específicamente?
4. ¿Qué herramientas de **generación de contenido para redes** con IA existen y qué hacen bien o mal en sectores regulados?
5. ¿Qué modelos de precio usa el mercado (por conversación, resolución, minuto, resultado, asiento, plataforma) y en qué rangos públicos?
6. ¿Qué capacidades son *table stakes* en 2026 y cuáles diferencian?
7. ¿Cómo es el mercado de BNPL y microcrédito por país núcleo (MX, CO, BR, PE, CL, AR)? Tamaño o crecimiento, jugadores relevantes, adopción de WhatsApp e intensidad de la cobranza.
8. ¿Dónde está el espacio en blanco para Sora Agents?

## Secciones obligatorias del entregable (sección 2 de la plantilla)

### 2.1 Ficha detallada de Wonderful
Producto, canales, idiomas y localización, arquitectura (orquestación, agnosticismo de modelo), Agent Builder, integraciones, guardrails, analítica, modelo operativo (equipos embebidos o FDE), verticales y clientes públicos, pricing (si es público), financiamiento y valuación (verificando los rumores mencionados en `11-VISION`), presencia en LatAm. Cierra con "lecciones para Sora".

### 2.2 Fichas de comparables (mínimo 12, verificadas)
Una ficha corta por jugador: qué hace, canales, foco vertical o regional, modelo de precio, diferenciador y relevancia para Sora. **Lista de partida (verifica existencia y relevancia, reemplaza las que no apliquen y agrega las que falten):**
- **Globales horizontales:** Sierra, Decagon, Parloa, PolyAI, Cognigy (NICE), Intercom Fin, Zendesk AI agents, Salesforce Agentforce, Amazon Connect con agentes de IA (como referencia nativa de AWS).
- **LatAm (conversacional y WhatsApp):** Jelou, Treble, Truora, Botmaker, Yalo, Gupshup o Infobip (como BSP con IA).
- **Cobranza con IA:** Skit.ai, Prodigal y jugadores latinoamericanos de collection-tech (investígalos).
- **Contenido o marketing con IA:** Jasper, Copy.ai, Canva (Magic Studio), herramientas de gestión de redes con IA.

### 2.3 Matriz de capacidades
Filas = jugadores; columnas: WhatsApp · voz · web chat · email · español nativo con variantes · portugués · builder no-code · integraciones con core o CRM · guardrails y políticas deterministas · evals y QA · analítica · despliegue (SaaS, VPC, on-prem) · vertical financiero · cobranza · humano en el loop · modelo de servicio (self-serve o FDE) · unidad de precio. Usa "Sí / Parcial / No / n/d" y no dejes celdas vacías.

### 2.4 Modelos de pricing observados
| Jugador | Unidad de cobro | Precio público o rango | Mínimos o plataforma | Fuente y fecha |
Agrega un análisis: qué unidad alinea mejor los incentivos con un cliente fintech y por qué.

### 2.5 Modelo operativo y time-to-value
Cómo despliegan los líderes (FDE, equipos locales, partners), tiempos reportados a producción y tamaño típico del equipo de implementación. Implicaciones para el equipo de Sora (insumo de WP11).

### 2.6 Mercado por país núcleo
Tabla por país (MX, CO, BR, PE, CL, AR): señales del tamaño o crecimiento de BNPL y microcrédito, principales fintechs del segmento (como ejemplos de posibles clientes, sin contactarlos), penetración de WhatsApp, complejidad percibida de la cobranza, competencia local de agentes de IA e idioma. Cierra con un **ranking recomendado de países para el piloto** desde la óptica de mercado. WP06 aporta la óptica regulatoria y el equipo decide (`[OD-11]`).

### 2.7 Table stakes, diferenciales y futuras
Tres listas, con una justificación breve por elemento.

### 2.8 Posicionamiento recomendado
Tres opciones de posicionamiento (por ejemplo: "agentes de cobranza para fintech LatAm", "equipo de operaciones IA para crédito", "plataforma vertical con Sora Core"), con pros y contras, y una recomendación. Incluye mensajes clave y objeciones esperadas con su respuesta.

### 2.9 Implicaciones para el MVP
- Lista de capacidades **must-have** para WP02, con su justificación competitiva.
- Hipótesis de pricing para WP09: unidad y rangos.
- Evaluación de la hipótesis "agentes como producto de entrada (wedge)" de `11-VISION §6`.

## Diagramas obligatorios
1. Mapa de posicionamiento (`quadrantChart` no está permitido; usa una tabla 2×2 o un `flowchart` con cuatro cuadrantes): eje horizontal vs. vertical, eje global vs. LatAm.

## Criterios de aceptación
- [ ] La ficha de Wonderful está completa y cada dato tiene fuente y fecha, o lleva `[VERIFICAR]`.
- [ ] Hay ≥ 12 comparables verificados, con al menos 4 latinoamericanos y 2 de cobranza.
- [ ] La matriz de capacidades está completa, sin celdas vacías.
- [ ] Cada precio citado tiene fuente y fecha, o `[VERIFICAR]`.
- [ ] El ranking de países para el piloto tiene criterios explícitos.
- [ ] Hay una recomendación de posicionamiento explícita.
- [ ] Hay listas accionables para WP02 y WP09.
- [ ] No hay métricas de mercado inventadas: si no hay dato, se dice.

## Fuera de alcance
Análisis legal (WP06), costos de infraestructura (WP08), pricing final (WP09).
