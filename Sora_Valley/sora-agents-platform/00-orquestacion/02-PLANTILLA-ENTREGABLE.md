# 02 · Plantilla obligatoria de entregable

Copia esta estructura en tu archivo de salida. Las secciones 0, 1 y 3 a 8 son fijas. La sección 2 la definen las secciones obligatorias de tu encargo: numéralas 2.1, 2.2, etc.

---

```markdown
# WPxx · <Título del encargo>

| Campo | Valor |
|---|---|
| Encargo | 02-encargos/WPxx-....md |
| Autor | Agente <modelo> · corrida <fecha> |
| Versión | 1.0 |
| Insumos leídos | lista de archivos de contexto, entregables y gates leídos |
| Acceso web en esta corrida | Sí / No |
| Estado | Completo / Completo con pendientes (enumerar) |

## 0. Resumen ejecutivo
- 6 a 12 bullets: conclusiones, números clave, recomendaciones y decisiones tomadas.
- Debe entenderse sin leer el resto.

## 1. Alcance
- Qué cubre este entregable.
- Qué NO cubre y qué WP lo cubre.

## 2. Contenido principal
### 2.1 …
### 2.2 …
(Una subsección por cada "sección obligatoria" del encargo, en el mismo orden.)

## 3. Decisiones tomadas en este WP
| ID | Decisión | Opciones evaluadas | Elegida | Razón | Impacto en otros WPs |
|---|---|---|---|---|---|
| OD-xx / D-WPxx-nn | … | … | … | … | … |

Si propones cambiar una ADR: fila con `[CAMBIO-ADR]` y su justificación.

## 4. Supuestos
### 4.1 Supuestos de 13-SUPUESTOS que usaste
Lista de S-xx.
### 4.2 Supuestos nuevos propuestos
| ID | Supuesto | Valor | Justificación | Sensibilidad (alta/media/baja) |
|---|---|---|---|---|

## 5. Riesgos
| ID | Riesgo | Probabilidad (A/M/B) | Impacto (A/M/B) | Mitigación | Dueño sugerido |
|---|---|---|---|---|---|

## 6. Preguntas abiertas
| # | Pregunta | Para quién (Sora / Cliente / Legal) | ¿Bloqueante? | Qué cambia según la respuesta |
|---|---|---|---|---|

## 7. Interfaces con otros WPs
| WP | Qué le entrego | Qué necesito de él |
|---|---|---|

## 8. Checklist de autoverificación
- [ ] Todas las secciones obligatorias del encargo están presentes.
- [ ] Se cumplen todos los criterios de aceptación del encargo (enumerarlos).
- [ ] Todos los diagramas son Mermaid válido y llevan IDs canónicos.
- [ ] Toda cifra tiene fórmula o fuente; todo precio sin fuente lleva [VERIFICAR].
- [ ] Todo punto regulatorio lleva [LEGAL].
- [ ] No se contradicen decisiones base sin [CAMBIO-ADR].
- [ ] Los supuestos nuevos están listados en 4.2.

## Anexo A · Fuentes
[F1] Título, URL, fecha de consulta.
```

---

## Checklist general de calidad (la aplica también el revisor de gate)

| # | Criterio | Cómo se verifica |
|---|---|---|
| Q1 | Completo | Todas las secciones obligatorias del encargo existen y tienen contenido sustantivo. |
| Q2 | Trazable | Requisitos, componentes y herramientas usan IDs canónicos. |
| Q3 | Consistente | Los números salen de `13-SUPUESTOS` o están declarados como supuestos nuevos. |
| Q4 | Verificable | Precios, disponibilidad y normas tienen fuente o marcador. |
| Q5 | Accionable | Cada sección termina en una decisión, un requisito, una tarea o una pregunta concreta. |
| Q6 | Visual | Están todos los diagramas pedidos y renderizan. |
| Q7 | Honesto | Los riesgos y las incertidumbres están explícitos; no hay afirmaciones de cumplimiento sin `[LEGAL]`. |
| Q8 | Integrable | La sección 7 deja claras las interfaces con otros WPs. |
