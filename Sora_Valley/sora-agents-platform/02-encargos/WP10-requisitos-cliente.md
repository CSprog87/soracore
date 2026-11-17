# WP10 · Requisitos hacia el cliente y onboarding

| Campo | Valor |
|---|---|
| Oleada | 2 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/` y `03-entregables/_GATE-1.md` |
| Insumos de otros WPs | **WP02** (alcance, intents, métricas), **WP05** (CMI, WhatsApp, voz, pagos, handoff), **WP06** (contratos, cumplimiento por país); WP04 opcional (configuración de agentes) |
| Archivo de salida | `03-entregables/WP10-requisitos-cliente.md` |
| Acceso web | Opcional |
| Extensión orientativa | 5,000–7,000 palabras más checklists |
| Dueño de | Catálogo RC-xx, checklist de readiness, playbook de onboarding con RACI, plan de UAT y criterios de go-live |

## Rol
Eres líder de implementación (FDE lead o solutions architect) de plataformas B2B de IA para fintech en LatAm.

## Objetivo
Definir **todo lo que Sora le pide al cliente** para poner agentes en producción: contractual, técnico, de datos, de canales, de contenido, operativo, de seguridad y de cumplimiento, por agente y por país. Además, **cómo se ejecuta el onboarding**, semana a semana, hasta el go-live. Tiene que poder entregarse al cliente casi tal cual.

## Secciones obligatorias

### 2.1 Catálogo de requisitos al cliente (RC-xx)
Tabla: ID · categoría · requisito · detalle o entregable concreto · agente(s) · obligatorio u opcional · fase (antes del kickoff, construcción, UAT, go-live) · variante por país · responsable del lado del cliente (rol) · bloquea go-live (Sí/No).
Categorías mínimas:
1. **Comercial y legal:** contrato y DPA firmados, aviso de privacidad actualizado (IA, encargados, transferencias), base de consentimiento y opt-in de WhatsApp, respuesta a las preguntas legales bloqueantes de WP06 en su país, figura regulatoria del tenant.
2. **Canales:** Meta Business verificado, WABA y número por país, display name, acceso para el onboarding (embedded signup), plantillas a aprobar, números de voz (si aplica), cuentas de Instagram y Facebook con permisos (AG-CON).
3. **Integración técnica:** endpoints del **CMI** (de WP05) en sandbox y producción, eventos, archivo batch de cartera, credenciales, allowlisting de IPs, SLAs de sus APIs, contacto técnico.
4. **Datos:** histórico de conversaciones (anonimizado) para evals, histórico de cobranza para la línea base, catálogo de productos y tarifas, datos para el holdout.
5. **Contenido y conocimiento:** FAQs, términos y condiciones, políticas de producto, guiones y políticas de cobranza (rangos de promesa, planes permitidos, descuentos por tramo), protocolo de cliente vulnerable, manual de marca, leyendas obligatorias de publicidad por país, ejemplos de contenido aprobado.
6. **Operación:** equipo de escalamiento (horarios y capacidad), herramienta de handoff (`[OD-04]`), SPOC de negocio y técnico, proceso de aprobación de contenido (1 o 2 niveles), proceso de quejas formales.
7. **Seguridad:** respuesta al cuestionario de seguridad, contactos de incidentes, permisos para pruebas.
8. **Métricas:** línea base de KPIs (contención actual, FRT, recuperación por tramo, costo por gestión) y metas acordadas para el piloto.

### 2.2 Requisitos mínimos por agente
Tres tablas resumidas (AG-ATC, AG-COB, AG-CON) con lo **mínimo indispensable** para lanzar cada agente. Ayudan a vender un piloto de un solo agente.

### 2.3 Variantes por país
Qué cambia en los requisitos según el país núcleo: documentos legales, registros, leyendas, medios de pago, números y plantillas.

### 2.4 Checklist de readiness (go/no-go)
Checklist para el kickoff y checklist para el go-live, con un criterio verificable por ítem.

### 2.5 Playbook de onboarding
- Plan semana por semana, de la firma al go-live (meta `[S-45]`): fases de descubrimiento, configuración, integración, contenido y conocimiento, evals con datos del cliente, UAT, piloto controlado (porcentaje de tráfico), go-live e hypercare.
- `gantt` de Mermaid del onboarding típico de un agente y de tres agentes.
- **RACI** (Sora: FDE, ingeniero de agentes, diseñador conversacional, compliance; cliente: negocio, TI, legal, CX, cobranza, marketing) por actividad.
- Entregables de cada fase y quién los aprueba.

### 2.6 Plan de UAT
Escenarios de prueba con el cliente (basados en los intents MoSCoW Must de WP02), datos de prueba, criterios de aceptación, quién firma y cómo se manejan los hallazgos.

### 2.7 Piloto controlado y go-live
Cómo se abre el tráfico (por porcentaje, segmento, horario o país), criterios de expansión y de rollback, hypercare (duración y SLA) y reporte del piloto (KPIs frente a la línea base y frente al holdout).

### 2.8 Plantillas para el cliente
- **Data request template**: lista de lo que se pide, en formato copiable.
- **Cuestionario de descubrimiento** (unas 40 preguntas) para la primera sesión con una fintech.
- **One-pager de requisitos** para enviar en la etapa comercial.

## Diagramas obligatorios
1. `gantt` del onboarding (1 agente). 2. `gantt` del onboarding (3 agentes). 3. `flowchart` swimlane-like (subgraphs Sora y Cliente) del proceso de onboarding.

## Criterios de aceptación
- [ ] ≥ 50 RC-xx en las 8 categorías, con indicador de bloqueo.
- [ ] Requisitos mínimos por agente.
- [ ] Variantes por país núcleo.
- [ ] Checklists de readiness verificables.
- [ ] Playbook con gantt y RACI.
- [ ] Plan de UAT y de piloto con criterios numéricos.
- [ ] Cuestionario y data request copiables.
- [ ] Coherencia con el CMI de WP05 y con los requisitos legales de WP06.

## Fuera de alcance
Requisitos internos de Sora (WP11) y precios (WP09).
