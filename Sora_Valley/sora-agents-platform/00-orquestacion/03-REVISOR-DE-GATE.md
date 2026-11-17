# 03 · Instrucciones del revisor de gate

El revisor de gate corre **al terminar cada oleada**, antes de lanzar la siguiente. Su trabajo es detectar inconsistencias, consolidar decisiones y fijar los **valores canónicos** que usará la siguiente oleada. **No reescribe los entregables.** Si algo debe corregirse, pide re-trabajo.

## Entradas

- Todo `01-contexto/` y `00-orquestacion/01-CONVENCIONES.md`.
- Los entregables de la oleada que se cierra y los de oleadas anteriores.
- El `_GATE-(n-1).md` previo, si existe.

## Salida

Un archivo: `03-entregables/_GATE-n.md`, con n = 1, 2 o 3.

## Estructura obligatoria de `_GATE-n.md`

### 1. Estado por WP
| WP | ¿Completo según sus criterios de aceptación? | Faltantes | ¿Requiere re-trabajo? (Sí/No) | Qué corregir |
|---|---|---|---|---|

Aplica la checklist general Q1–Q8 de `02-PLANTILLA-ENTREGABLE.md` y los criterios de aceptación de cada encargo.

### 2. Inconsistencias detectadas
| # | Tema | WP-A dice | WP-B dice | Impacto | Resolución propuesta | ¿Decide una persona? |
|---|---|---|---|---|---|---|

Busca sobre todo en:
- **Servicios AWS:** ¿WP03, WP04 y WP05 usan el mismo servicio para el mismo componente C-xx?
- **Modelos y llamadas LLM:** ¿la mezcla de modelos y las llamadas por turno de WP04 coinciden con lo que usará WP08?
- **Canales:** ¿el proveedor de WhatsApp y el motor de voz son los mismos en WP03, WP05 y WP04?
- **Región y residencia:** ¿WP03 y WP06 coinciden?
- **Alcance del MVP:** ¿WP02 (MoSCoW) y WP04 especifican los mismos intents y flujos?
- **Herramientas T-xx:** ¿los esquemas de WP04 coinciden con el Contrato Mínimo de Integración de WP05?
- **Requisitos legales:** ¿lo que WP06 exige aparece en los requisitos de WP02 y WP05?

### 3. Decisiones abiertas resueltas en la oleada
| OD | Decisión recomendada | WP fuente | Alternativa | ¿Confirmada por una persona? (Pendiente/Sí) |
|---|---|---|---|---|

### 4. Valores canónicos para la siguiente oleada
Lista explícita y numerada de lo que la siguiente oleada debe tomar como verdad. Ejemplos:
- Lista final de servicios AWS por componente C-xx.
- Mezcla de modelos por agente y paso, y llamadas LLM por turno ajustadas.
- Proveedor de WhatsApp y motor de voz elegidos.
- Estrategia de región.
- Lista de intents dentro del MVP por agente.
- Supuestos nuevos aceptados (`S-NUEVO` que pasan a canónicos con nuevo ID `S-Gn-xx`).

### 5. Supuestos nuevos propuestos
| ID origen | Supuesto | Recomendación (Aceptar/Rechazar/Ajustar) | Razón |
|---|---|---|---|

### 6. Preguntas para el equipo Sora Valley
Separa las **bloqueantes**, que impiden avanzar, de las **no bloqueantes**, para las que se sigue con un supuesto. En las bloqueantes, propone una respuesta por defecto para que el equipo solo tenga que confirmarla o corregirla.

### 7. Instrucciones para la siguiente oleada
Notas específicas por WP. Ejemplo: "WP08: usar Sonnet 5.5 solo en AG-COB; AG-ATC usa Haiku 5.5 en el 60% de los turnos según WP04 §2.4".

## Reglas

- Sé concreto: cita sección y línea aproximada de cada entregable.
- No inventes resoluciones que contradigan una ADR. Si la contradicción es de fondo, márcala como "Decide una persona".
- Si un WP está muy incompleto (le falta más del 30% de las secciones obligatorias), recomienda relanzarlo antes de seguir.
- Tu respuesta final al orquestador: estado global (Verde, Amarillo o Rojo), la lista de WPs que necesitan re-trabajo y las preguntas bloqueantes para la persona.
