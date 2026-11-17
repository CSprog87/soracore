# WP13 · Integración, consistencia y documento maestro

| Campo | Valor |
|---|---|
| Oleada | 4 |
| Modelo sugerido | Claude Sonnet 5.5 u Opus 5.5 |
| Lee obligatoriamente | Todo el paquete: `00-orquestacion/`, `01-contexto/`, `02-encargos/` y **todos** los archivos de `03-entregables/`, incluidos los tres `_GATE-n.md` |
| Archivos de salida | `03-entregables/00-DOCUMENTO-MAESTRO.md` y `03-entregables/99-REPORTE-CONSISTENCIA.md` |
| Acceso web | No requerido |
| Extensión orientativa | Documento maestro: 6,000–9,000 palabras. Reporte: lo necesario. |

## Rol
Eres el arquitecto principal y el editor del expediente. Tu trabajo es **integrar, no inventar**.

## Objetivo
Entregar al liderazgo de Sora Valley **un solo documento** que permita decidir la construcción del MVP, y un reporte que deje trazadas todas las inconsistencias y cómo se resolvieron.

## Reglas
1. **No introduces datos nuevos.** Todo número, decisión o requisito del documento maestro viene de un entregable (cita el WP y la sección) o de una nota de gate.
2. Cuando dos entregables se contradicen, aplica esta jerarquía: **decisión humana registrada en un gate > ADR > valor canónico de un gate > entregable del WP dueño del tema > otros entregables.** Registra cada caso en el reporte.
3. Puedes **reutilizar diagramas** de los entregables (cópialos y cita la fuente). Solo creas diagramas nuevos de resumen.
4. El documento maestro debe entenderse sin leer los entregables. Los entregables son el anexo técnico.

## Estructura obligatoria de `00-DOCUMENTO-MAESTRO.md`

0. **Resumen ejecutivo** (máximo 2 páginas): qué es Sora Agents Platform, por qué ahora, qué incluye el MVP, cuánto cuesta construirlo, cuánto cuesta operarlo por conversación y por escenario, a qué precio se vende y con qué margen, en cuánto tiempo, en qué país se pilotea, top 5 riesgos y **decisiones que se piden al liderazgo**.
1. **Contexto y oportunidad:** Sora Valley, el mercado LatAm, la referencia de Wonderful y el posicionamiento (WP01).
2. **El producto:** agentes, capacidades, alcance del MVP y fases (WP02, `12-CATALOGO`).
3. **Cómo funciona:** arquitectura de alto nivel (2 o 3 diagramas), agentes y workflows clave, modelo multi-tenant y multi-país, regiones y celdas (WP03, WP04).
4. **Canales e integraciones:** decisiones de canal, Contrato Mínimo de Integración (resumen) y Sora Core (WP05).
5. **Confianza:** seguridad, privacidad, cumplimiento por país (resumen de la matriz) y preguntas legales bloqueantes (WP06).
6. **Calidad y operación:** observabilidad, evals, gates y SLAs (WP07, WP11).
7. **Economía:** costos por unidad y escenario, pricing, margen, ROI del cliente y proyección (WP08, WP09).
8. **Requisitos:** resumen de lo que se pide al cliente y de lo que necesita Sora internamente (WP10, WP11).
9. **Plan:** roadmap, equipo, presupuesto del build y de 12 meses de OPEX, y plan del piloto (WP12).
10. **Riesgos:** top 10 consolidados.
11. **Registro de decisiones:** todas las ADR y OD con su estado final (decidida por una persona, recomendada pendiente de confirmar o diferida) y su fuente.
12. **Próximos pasos:** los 10 siguientes pasos concretos con dueño sugerido.
13. **Índice de anexos:** enlaces a cada entregable.

## Estructura obligatoria de `99-REPORTE-CONSISTENCIA.md`

1. **Tabla de inconsistencias:** # · tema · fuentes en conflicto (WP y sección) · descripción · resolución aplicada · regla de jerarquía usada · ¿requiere confirmación humana?
2. **Cobertura:** cada C-xx, W-xx, T-xx, agente y OD → en qué entregables está cubierto. Marca los huecos.
3. **Marcadores residuales:** conteo y lista de `[VERIFICAR]`, `[LEGAL]` y `[PREGUNTA-ABIERTA]` que siguen abiertos, agrupados por tema y con prioridad.
4. **Calidad por entregable:** la checklist Q1–Q8 aplicada a cada WP (OK, observación o falla).
5. **Recomendaciones de siguiente iteración:** qué WPs convendría profundizar o rehacer, y por qué.

## Criterios de aceptación
- [ ] Ningún dato del documento maestro sin fuente (WP y sección).
- [ ] El resumen ejecutivo responde: alcance, costo del build, costo de operación, precio, margen, plazo, país piloto, riesgos y decisiones solicitadas.
- [ ] Registro de decisiones completo (14 ADR y 11 OD).
- [ ] Reporte de consistencia con cobertura completa y marcadores residuales.
- [ ] Diagramas válidos.
