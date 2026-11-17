# WP12 · Plan del MVP, backlog y estimación

| Campo | Valor |
|---|---|
| Oleada | 3 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, `03-entregables/_GATE-1.md` y `_GATE-2.md` |
| Insumos de otros WPs | **WP01 a WP08, WP10 y WP11**. WP09 corre en paralelo: si existe un borrador, úsalo para el caso de negocio; si no, deja el enganche. |
| Archivo de salida | `03-entregables/WP12-plan-mvp-y-estimacion.md` |
| Acceso web | Opcional |
| Extensión orientativa | 6,000–9,000 palabras más backlog |
| Dueño de | Alcance final del MVP, fases, roadmap, épicas e historias, estimación de esfuerzo, equipo y calendario, presupuesto del MVP, ruta crítica y riesgos consolidados |

## Rol
Eres director de ingeniería y delivery con experiencia en lanzar plataformas de IA en producción para clientes regulados.

## Objetivo
Convertir todo el diseño en un **plan ejecutable y presupuestado**: qué se construye, en qué orden, con quién, en cuánto tiempo, cuánto cuesta y qué puede salir mal. La meta es `[S-61]`: piloto en producción en ≤ 16 semanas, o justificar otra.

## Secciones obligatorias

### 2.1 Alcance final del MVP
Consolida el MoSCoW de WP02 con las decisiones del Gate 1 (voz, país piloto, pt-BR y handoff). Tabla final de "dentro, fase 1.5, fase 2 y fase 3" por agente y plataforma, con la justificación de cada exclusión.

### 2.2 Fases y roadmap
- **Fase 0, fundaciones:** cuentas, IaC, CI/CD, seguridad base, observabilidad, modelo de datos, consola esqueleto.
- **Fase 1, MVP:** router, AG-ATC y AG-COB por WhatsApp y web, AG-CON, handoff, evals y gates, onboarding del cliente de diseño.
- **Fase 1.5:** voz y otros ítems diferidos, si aplica.
- **Fase 2 y 3:** resumen.
- `gantt` de Mermaid por semana con hitos, demos y dependencias del cliente (de WP10).
- **Plan paralelo del onboarding del cliente de diseño** (las integraciones del tenant suelen ser la ruta crítica, `[RIESGO R-11]`).

### 2.3 Épicas e historias
- Épicas EP-nn que cubran **todos** los componentes C-xx, workflows W-xx, agentes, canales, integraciones, seguridad, observabilidad y evals, consola, facturación por uso, y documentación y runbooks.
- Para cada épica: objetivo, C-xx y RF-xx que cubre, dependencias y criterios de terminado.
- Historias HU-EP-nn-mm para las épicas de la fase 0 y la fase 1 (las de fases posteriores pueden quedar a nivel de épica). **Mínimo 80 historias** con su estimación.

### 2.4 Estimación de esfuerzo
- Estimación **de tres puntos (PERT)** por épica: optimista, más probable y pesimista, en semanas-persona, con el **rol** que la ejecuta.
- Supuestos de productividad declarados, incluido el uso de asistentes de código con IA en el equipo.
- Totales por rol y por fase. Rango de confianza (P50 y P80).

### 2.5 Equipo y calendario
- Composición del equipo para el MVP (coherente con WP11 §2.2): roles, cantidad y semanas de dedicación.
- Calendario resultante y cómo cambia con ±1 ingeniero (análisis de capacidad).
- Ruta crítica explícita.

### 2.6 Presupuesto del MVP
| Concepto | Fuente | Monto (USD) |
|---|---|---|
| Equipo (roles × semanas × `[S-50]`…`[S-59]`) | WP12 | |
| Nube durante el build | WP08 §2.8 | |
| Herramientas, legal, certificaciones y seguros durante el build | WP11 §2.10 | |
| Contingencia `[S-60]` | — | |
| **Total del build hasta el go-live del piloto** | | |
| OPEX de los primeros 12 meses (nube según ESC-P → ESC-L, equipo de operación, herramientas) | WP08, WP11 | |
Incluye un rango (P50 y P80) y el desglose mensual (curva de gasto).

### 2.7 Caso de negocio resumido
Con los ingresos de WP09 (si está disponible): inversión, ingresos y margen por escenario, mes de break-even y sensibilidad (retraso del piloto, menor volumen).

### 2.8 Plan del piloto con el cliente de diseño
Objetivos, métricas de éxito con umbrales numéricos (de WP02 y WP07), duración, plan de holdout para AG-COB, criterios para pasar a producción plena y para expandir a un segundo país.

### 2.9 Riesgos consolidados
Fusiona y deduplica los riesgos de todos los WPs: los **top 15**, con probabilidad, impacto, mitigación, dueño y disparador (la señal temprana).

### 2.10 Dependencias y decisiones pendientes
Lista de lo que el equipo de Sora Valley o el cliente deben decidir o entregar, y para cuándo, para no frenar el plan.

## Diagramas obligatorios
1. `gantt` del roadmap del MVP. 2. `gantt` del onboarding del cliente de diseño en paralelo (o integrado en el anterior). 3. `flowchart` de las dependencias entre épicas (ruta crítica resaltada en el texto). 4. `pie` de la distribución del presupuesto del build.

## Criterios de aceptación
- [ ] Las épicas cubren el 100% de los C-xx y W-xx del alcance del MVP (tabla de trazabilidad).
- [ ] ≥ 80 historias estimadas para las fases 0 y 1.
- [ ] Estimación PERT con P50 y P80.
- [ ] Calendario con ruta crítica; cumple o justifica la desviación de `[S-61]`.
- [ ] Presupuesto completo (build más 12 meses de OPEX) con rango.
- [ ] Plan del piloto con umbrales numéricos.
- [ ] Top 15 riesgos consolidados.
- [ ] Coherente con `_GATE-1.md` y `_GATE-2.md`.

## Fuera de alcance
Rediseñar arquitectura o agentes. Si detectas un problema, lo registras como riesgo o pregunta, no lo rediseñas.
