# 01 · Convenciones comunes para todos los agentes

Estas reglas son obligatorias. Su objetivo es que trece entregables escritos en paralelo por agentes distintos se puedan integrar sin reescribirlos.

---

## 1. Idioma y estilo

- **Los entregables se escriben en español neutro latinoamericano**: "cuota", "mora", "aclaración", "plan de pagos", "estado de cuenta", "línea de crédito". Cuando un término cambia por país (por ejemplo "aclaración", "reclamo" o "PQR"), usa el neutro y anota las variantes donde importe.
- **El alcance es LatAm multi-país, no un país.** Ningún diseño puede asumir un único país. Cuando algo depende del país (regulación, pagos, idioma, precios de canal, región AWS), preséntalo como **matriz por país** con los países núcleo `[S-04]`.
- **Los agentes del producto** hablan la variante local del tenant y del usuario: es-MX, es-CO, es-PE, es-CL, es-AR (con voseo) y otras, y **pt-BR** para Brasil (`[ADR-14]`).
- **Nombres propios y términos técnicos estándar en inglés, sin traducir**: Amazon Bedrock, AgentCore Runtime, Step Functions, prompt caching, handoff, guardrails, webhook.
- **Dos lectores por entregable:**
  1. Liderazgo de Sora Valley: necesita conclusiones, números y decisiones. Por eso cada entregable abre con un resumen ejecutivo.
  2. Equipo de construcción: necesita precisión técnica. Por eso el cuerpo baja al detalle.
- Usa tablas para comparaciones, requisitos y catálogos, y prosa para razonamiento y trade-offs.
- Sin marketing ni relleno. Cada sección aporta al menos una decisión, un dato, un requisito o un riesgo.
- Distingue siempre entre **hecho verificado**, **estimación**, **supuesto** y **recomendación**.

## 2. Marcadores obligatorios

| Marcador | Cuándo usarlo |
|---|---|
| `[S-xx]` | Usas un supuesto de `01-contexto/13-SUPUESTOS-Y-ESCENARIOS.md`. |
| `[S-NUEVO-n]` | Introduces un supuesto propio. Lístalo en la sección 4 de tu entregable con su justificación. |
| `[ADR-xx]` | Aplicas una decisión base de `14-DECISIONES-BASE.md`. |
| `[OD-xx]` | Tocas una decisión abierta. Si tu WP es dueño de resolverla, resuélvela con recomendación explícita. |
| `[CAMBIO-ADR]` | Propones cambiar una decisión base. Justifícalo en la sección 3. **No actúes como si ya estuviera cambiada.** |
| `[VERIFICAR]` | Dato no confirmado en fuente oficial durante esta corrida: precio, disponibilidad regional, cuota o característica. |
| `[LEGAL]` | Punto que requiere validación de un abogado. Nunca afirmes cumplimiento sin este marcador. |
| `[PREGUNTA-ABIERTA]` | Algo que el equipo de Sora Valley o el cliente debe responder. |
| `[RIESGO R-xx]` | Riesgo registrado. Los nuevos se numeran `R-WPnn-mm`. |

## 3. Identificadores canónicos

| Tipo | Formato | Fuente |
|---|---|---|
| Componentes de arquitectura | `C-01` … `C-32` | `15-ARQUITECTURA-REFERENCIA-BORRADOR.md`. Uno nuevo se llama `C-NUEVO-<nombre>` y se justifica. |
| Agentes | `AG-RTR` (router), `AG-ATC` (atención), `AG-COB` (cartera y cobranza), `AG-CON` (contenido); en fase 2, `AG-KYC` y `AG-COP` | `12-CATALOGO-AGENTES-MVP.md` |
| Herramientas de agentes | `T-01` … `T-29` | `12-CATALOGO-AGENTES-MVP.md` |
| Workflows | `W-01` … `W-08` | `14-DECISIONES-BASE.md` (lista); WP04 los detalla |
| Niveles de autenticación del usuario final | `L0`, `L1`, `L2` | `12-CATALOGO-AGENTES-MVP.md` |
| Requisitos funcionales | `RF-ATC-nn`, `RF-COB-nn`, `RF-CON-nn`, `RF-RTR-nn`, `RF-PLAT-nn` | WP02 |
| Requisitos no funcionales | `RNF-nn` | WP02 |
| Requisitos hacia el cliente | `RC-nn` | WP10 |
| Requisitos internos | `RI-nn` | WP11 |
| Riesgos | `R-01` … `R-18` (semilla en `16`); nuevos `R-WPnn-mm` | — |
| Épicas e historias | `EP-nn`, `HU-EP-nn-mm` | WP12 |
| Escenarios de volumen | `ESC-P` (piloto), `ESC-L` (lanzamiento), `ESC-E` (escala) | `13-SUPUESTOS` |
| Países | Códigos ISO de 2 letras: `MX`, `CO`, `BR`, `PE`, `CL`, `AR`, y `LATAM-OTROS` para el resto | `13-SUPUESTOS [S-04]` |
| Supuestos | `S-xx` | `13-SUPUESTOS` |
| Precios de referencia | `P-xx` | `13-SUPUESTOS` |

## 4. Números, monedas y cálculos

- **Costos de nube y precios a cliente en USD.** Cuando ayude a la lectura comercial, agrega la referencia en moneda local (MXN, COP, BRL, PEN, CLP, ARS) con los tipos de cambio `[S-02]`. Por la volatilidad del ARS, las referencias en pesos argentinos son solo indicativas.
- **Toda cifra calculada muestra la fórmula, los insumos y las unidades.** Ejemplo:
  `Costo LLM AG-ATC/mes = conversaciones [S-10] × turnos [S-20] × llamadas por turno [S-21] × costo por llamada (USD)`.
- En los resúmenes, redondea a 2–3 cifras significativas. En las tablas de cálculo, conserva la precisión.
- Un mes son 30 días y un año son 12 meses. Las horas pico salen de `[S-36]`.
- Los volúmenes salen de `13-SUPUESTOS`. Si usas otro valor, decláralo como `[S-NUEVO-n]`.
- No des precisión falsa. Si un rango es más honesto que un número, usa un rango y explica de qué depende.

## 5. Precios y disponibilidad regional

- **Fuente preferida:** las páginas oficiales (`aws.amazon.com/<servicio>/pricing`, guías de usuario de AWS, `developers.facebook.com` para WhatsApp y Graph API, documentación de Claude en Bedrock). Cita la URL y la fecha de consulta.
- **Sin acceso web:** usa la tabla `P-xx` de `13-SUPUESTOS` y marca `[VERIFICAR]`.
- **Región por defecto para precios:** `us-east-1`. Compara con las regiones AWS de LatAm donde el servicio exista: `sa-east-1` (São Paulo), `mx-central-1` (México) y la región de Chile si ya está operativa `[VERIFICAR]`.
- **Nunca inventes un precio.** Si no hay referencia, escribe "n/d `[VERIFICAR]`" y explica cómo afecta al resultado.
- **Los precios que varían por país** (tarifas de WhatsApp de Meta, telefonía, SMS) se presentan en tabla por país núcleo `[S-04]`.
- La disponibilidad de servicios y modelos en las regiones de LatAm es crítica (ver `[ADR-08]`). Todo WP que proponga un servicio AWS indica si está disponible en `sa-east-1` y en `mx-central-1`.

## 6. Diagramas

- **Todos en Mermaid**, dentro de bloques de código con el identificador `mermaid`.
- **Tipos permitidos:** `flowchart`, `sequenceDiagram`, `stateDiagram-v2`, `erDiagram`, `journey`, `gantt`, `pie`, `classDiagram`.
- **Evita los tipos experimentales** (`C4Context`, `architecture-beta`, `block-beta`), porque no renderizan en todos lados.
- **Reglas de sintaxis que evitan errores de render:**
  - Pon entre comillas dobles toda etiqueta con espacios, paréntesis, dos puntos o acentos: `C08["C-08 Agent Runtime (AgentCore)"]`.
  - Haz los saltos de línea con `<br/>`.
  - No uses `end`, `graph` ni `subgraph` como id de nodo.
  - Los ids van sin espacios, sin acentos y sin guiones: `C08`, `AGCOB`, `WAadapter`.
  - No pongas comillas dobles dentro de una etiqueta. Usa comillas simples o nada.
  - En `sequenceDiagram` declara `participant X as Nombre legible` y no uses `;` en los mensajes.
  - En `gantt` usa `dateFormat YYYY-MM-DD`.
  - En `flowchart`, escribe "y" en lugar de `&` dentro de etiquetas.
- **Cada diagrama lleva:** un encabezado con título, el diagrama, y 2–6 líneas de explicación debajo. En los nodos van los IDs `C-xx`, `AG-xx` o `W-xx` que correspondan.
- **Máximo unos 35 nodos por diagrama.** Si crece más, divídelo en vistas.
- Revisa mentalmente la sintaxis antes de entregar. Un diagrama que no renderiza cuenta como diagrama faltante.

## 7. Reglas de trabajo

1. **Escribe solo en tu archivo de salida.** No edites el contexto ni los entregables ajenos.
2. **No resuelvas contradicciones en silencio.** Documenta en la sección 3 (`[CAMBIO-ADR]`) o en preguntas abiertas.
3. **No inventes** clientes, testimonios, cifras de mercado, nombres de personas ni certificaciones.
4. **No prometas cumplimiento legal.** Todo cumplimiento lleva `[LEGAL]`.
5. **Si una sección del encargo no aplica**, deja una línea que diga por qué. No la omitas.
6. **Completo antes que corto.** Pero una tabla bien hecha vale más que tres párrafos.
7. **Al terminar**, recorre los criterios de aceptación de tu encargo y la checklist general de la plantilla. Corrige antes de entregar.
8. **Tu respuesta final al orquestador** (no el archivo) incluye: la ruta del entregable, 5 hallazgos clave, los supuestos nuevos, los conflictos con decisiones base y las preguntas abiertas bloqueantes.

## 8. Formato de archivo

- Sigue `02-PLANTILLA-ENTREGABLE.md` y numera los encabezados.
- Un solo `.md` por WP, salvo los anexos que pida el encargo (por ejemplo, el `.csv` de WP08).
- Pon la fuente al final de la sección, o en el anexo de fuentes con número `[F1]`, `[F2]`…
