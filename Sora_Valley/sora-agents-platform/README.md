# Sora Agents Platform: paquete de diseño del MVP (Agents-as-a-Service sobre AWS)

> Versión 0.1 · 13-nov-2026 · Estado: listo para que lo ejecute el pool de agentes
> Origen: la propuesta de negocio de la landing `../Main.dc.html` (mockup de diseño de Sora Valley)

## 1. Qué es esto

Este paquete reúne el **contexto**, las **decisiones base** y los **encargos de trabajo (WP)** que necesita un pool de agentes (Claude Sonnet) para diseñar y estimar de punta a punta el MVP de **Sora Agents Platform**. Sora Agents Platform es la línea de negocio de *agentes de IA como servicio* de Sora Valley. Sigue el modelo de Wonderful, pero está verticalizada para fintechs de crédito (BNPL y microcréditos) en **Latinoamérica**, con operación **multi-país desde el diseño**, y se construye sobre AWS.

> **Alcance geográfico (directriz del equipo):** el mercado es **LatAm**, no un solo país. La landing dice "empezando por México", pero la plataforma se diseña multi-país desde el día 1: regulación, idioma (variantes del español y portugués de Brasil), monedas, medios de pago, precios de WhatsApp y regiones AWS por país. El país del primer piloto es una decisión abierta (`[OD-11]`).

La corrida debe producir un expediente completo con:

- Arquitectura AWS con diagramas de infraestructura, red, cuentas y multi-tenancy.
- Diseño de cada agente y de los workflows agénticos y deterministas: estados, secuencias, herramientas y políticas.
- Canales (WhatsApp, web, voz, redes sociales) e integraciones con el cliente y con Sora Core.
- Datos, seguridad, privacidad y cumplimiento regulatorio multi-país en LatAm (matriz por país).
- Observabilidad, evaluación y calidad de los agentes.
- Costos de infraestructura por escenario y unit economics por conversación.
- Modelo comercial y pricing del servicio.
- Requisitos hacia el cliente y requisitos internos de Sora Valley.
- Plan del MVP con backlog, estimación de esfuerzo, equipo, presupuesto y riesgos.
- Un **documento maestro** que lo integra todo.

## 2. Para quién

| Lector | Qué hace con el paquete |
|---|---|
| Agentes ejecutores (Sonnet) | Cada uno toma un WP, lee el contexto y escribe su entregable en `03-entregables/`. |
| Revisor de gate (Sonnet u Opus) | Al cerrar cada oleada revisa consistencia y fija los valores canónicos de la siguiente. |
| Integrador (WP13) | Consolida todo en el documento maestro. |
| Equipo Sora Valley | Toma las decisiones en cada gate y revisa el expediente final. |

## 3. Estructura

```
sora-agents-platform/
├── README.md                          ← este archivo
├── 00-orquestacion/
│   ├── 00-PLAYBOOK-ORQUESTACION.md    ← cómo lanzar y coordinar el pool de agentes
│   ├── 01-CONVENCIONES.md             ← reglas comunes: idioma, IDs, marcadores, diagramas, cálculos
│   ├── 02-PLANTILLA-ENTREGABLE.md     ← estructura obligatoria de cada entregable + checklist
│   └── 03-REVISOR-DE-GATE.md          ← instrucciones del revisor entre oleadas
├── 01-contexto/
│   ├── 10-SORA-VALLEY-NEGOCIO.md      ← la propuesta de negocio según la landing
│   ├── 11-VISION-SORA-AGENTS-AAAS.md  ← visión AaaS, referencia Wonderful, alcance del MVP
│   ├── 12-CATALOGO-AGENTES-MVP.md     ← agentes, intents, herramientas, niveles de autenticación, KPIs
│   ├── 13-SUPUESTOS-Y-ESCENARIOS.md   ← FUENTE ÚNICA de volúmenes, tokens, precios de referencia y costos
│   ├── 14-DECISIONES-BASE.md          ← ADRs ya tomadas + decisiones abiertas (OD)
│   ├── 15-ARQUITECTURA-REFERENCIA-BORRADOR.md ← componentes C-xx, diagramas semilla
│   ├── 16-REGULATORIO-Y-RIESGOS-LATAM.md ← matriz regulatoria por país (a verificar) + riesgos semilla
│   └── 17-GLOSARIO.md
├── 02-encargos/                       ← un archivo por WP (instrucciones para cada agente)
│   ├── WP01 … WP13
└── 03-entregables/                    ← aquí escriben los agentes (vacío al inicio)
    └── README.md
```

## 4. Orden de lectura obligatorio para todo agente

1. `README.md` (este archivo)
2. `00-orquestacion/01-CONVENCIONES.md`
3. `00-orquestacion/02-PLANTILLA-ENTREGABLE.md`
4. `01-contexto/10` → `17`, completos y en orden
5. Su encargo en `02-encargos/WPxx-*.md`
6. Los entregables previos y las notas de gate (`03-entregables/_GATE-n.md`) que indique su encargo

## 5. Mapa de encargos

| WP | Título | Oleada | Depende de | Entregable |
|---|---|---|---|---|
| WP01 | Benchmark competitivo (Wonderful y comparables) | 1 | — | `03-entregables/WP01-benchmark-mercado.md` |
| WP02 | Producto: casos de uso, journeys y requisitos (PRD) | 1 | — | `03-entregables/WP02-producto-prd.md` |
| WP03 | Arquitectura AWS e infraestructura | 1 | — | `03-entregables/WP03-arquitectura-aws.md` |
| WP04 | Diseño de agentes y workflows agénticos | 1 | — | `03-entregables/WP04-agentes-y-workflows.md` |
| WP05 | Canales e integraciones | 1 | — | `03-entregables/WP05-canales-e-integraciones.md` |
| WP06 | Datos, seguridad, privacidad y cumplimiento | 1 | — | `03-entregables/WP06-seguridad-y-cumplimiento.md` |
| WP07 | Observabilidad, evaluación y calidad | 2 | WP02, WP03, WP04 | `03-entregables/WP07-observabilidad-y-evals.md` |
| WP08 | Costos de infraestructura y unit economics | 2 | WP03, WP04, WP05 | `03-entregables/WP08-costos-y-unit-economics.md` (+ `.csv`) |
| WP10 | Requisitos hacia el cliente y onboarding | 2 | WP02, WP05, WP06 | `03-entregables/WP10-requisitos-cliente.md` |
| WP11 | Requisitos internos de Sora Valley | 2 | WP03, WP05, WP06 | `03-entregables/WP11-requisitos-internos.md` |
| WP09 | Modelo comercial y pricing | 3 | WP01, WP02, WP08 | `03-entregables/WP09-modelo-comercial.md` |
| WP12 | Plan del MVP, backlog y estimación | 3 | WP01–WP11 | `03-entregables/WP12-plan-mvp-y-estimacion.md` |
| WP13 | Integración y documento maestro | 4 | Todos | `03-entregables/00-DOCUMENTO-MAESTRO.md` + `99-REPORTE-CONSISTENCIA.md` |

Entre oleadas corre un **revisor de gate**, que escribe `03-entregables/_GATE-1.md`, `_GATE-2.md` y `_GATE-3.md`.

## 6. Cómo ejecutar

Ver `00-orquestacion/00-PLAYBOOK-ORQUESTACION.md`. Ahí están el plan de oleadas, el prompt de lanzamiento listo para copiar, los puntos donde decide una persona y las formas de correrlo en Claude Code.

## 7. Qué NO es este paquete

- **No es el diseño final.** Es el insumo para producirlo. Las decisiones marcadas como abiertas (OD-xx) las resuelven los WPs y las confirma el equipo en los gates.
- **Las cifras de `13-SUPUESTOS` son hipótesis de trabajo**, no datos de clientes reales. Se ajustan en el piloto.
- **Nada aquí constituye asesoría legal.** Todo punto regulatorio está marcado para validación.
- Los precios de referencia **deben verificarse** contra fuentes oficiales durante la corrida.
