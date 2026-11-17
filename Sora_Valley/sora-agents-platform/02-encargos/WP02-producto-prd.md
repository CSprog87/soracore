# WP02 · Producto: casos de uso, journeys y requisitos (PRD)

| Campo | Valor |
|---|---|
| Oleada | 1 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, en especial 11, 12, 13 y 16 |
| Insumos de otros WPs | Ninguno |
| Archivo de salida | `03-entregables/WP02-producto-prd.md` |
| Acceso web | Opcional |
| Extensión orientativa | 6,000–9,000 palabras más tablas |
| Dueño de | Alcance MoSCoW del MVP; requisitos RF y RNF; definiciones de métricas de producto |

## Rol
Eres product manager senior de productos conversacionales y fintech, con experiencia en atención y cobranza en LatAm.

## Objetivo
Producir el **PRD del MVP** de Sora Agents Platform: quién lo usa, qué problemas resuelve, qué debe hacer cada agente y la plataforma (requisitos trazables y priorizados), cómo se mide, y qué entra y qué no en el MVP.

## Secciones obligatorias

### 2.1 Personas (mínimo 8)
Al menos: usuario final al corriente; usuario final en mora; prospecto; operador de cobranza del tenant; agente humano de atención del tenant; marketing manager del tenant; administrador del tenant (configuración); compliance del tenant; operador de Sora (FDE o Ops). Para cada persona: objetivos, dolores, contexto (dispositivo, canal, país), qué espera del sistema y qué le preocupa.

### 2.2 Journeys
- Journey end-to-end por agente (AG-ATC, AG-COB, AG-CON), con un diagrama `journey` de Mermaid por agente.
- Journey del **onboarding de un tenant** (de la firma al go-live).
- Journey del **operador** del tenant en Sora Console (día típico).
- Journey de un **usuario multi-intent**: entra preguntando su saldo, está en mora, termina con una promesa de pago (handover de ATC a COB).

### 2.3 Catálogo de casos de uso e intents
Tabla por agente con columnas: ID · intent · descripción · canal · auth (L0/L1/L2) · herramientas T-xx · resultado esperado · condición de escalamiento · MoSCoW · fase · **variaciones por país** (si las hay).
Mínimos: AG-ATC ≥ 22 intents; AG-COB ≥ 15; AG-CON ≥ 10; AG-RTR ≥ 8 reglas o intents de ruteo; plataforma ≥ 15 capacidades. Parte del catálogo de `12-CATALOGO` y complétalo.

### 2.4 Requisitos funcionales
- `RF-RTR-nn`, `RF-ATC-nn`, `RF-COB-nn`, `RF-CON-nn` y `RF-PLAT-nn` (plataforma: tenants, operaciones de país, configuración de agentes, conocimiento, plantillas, políticas, handoff, consola, auditoría, analítica, usuarios y roles, facturación por uso).
- Cada RF lleva: descripción, criterio de aceptación verificable, componentes C-xx implicados, prioridad MoSCoW y fase.
- **Escribe en Gherkin** (Dado / Cuando / Entonces) los **20 requisitos más críticos**, con al menos 3 casos negativos: tercero que no es el titular, intento de obtener un descuento no permitido, contacto fuera de la ventana horaria del país.

### 2.5 Requisitos no funcionales (RNF)
Latencia, disponibilidad, escalabilidad (por escenario `[S-09]`…`[S-17]`), seguridad, privacidad, auditabilidad, retención, internacionalización (idioma, variante, zona horaria, moneda y formato de números y fechas por país), accesibilidad (web chat, WCAG 2.2 AA), costo máximo por conversación (meta preliminar, que WP08 confirma) y operabilidad. Cada RNF debe ser **medible**, con métrica, umbral y método de medición. Alinea con `[S-36]`…`[S-41]`.

### 2.6 Diseño conversacional
- Persona y tono por agente, configurables por tenant y país. Ejemplos de la misma respuesta en es-MX, es-CO, es-AR (voseo) y pt-BR.
- Disclosure de IA (`[ADR-13]`), saludo, verificación de identidad sin fricción excesiva, manejo de ambigüedad, reintentos, silencio o abandono, cierre, mensajes de error y de horario.
- Uso de elementos interactivos de WhatsApp (botones, listas, Flows) frente a texto libre.
- **Diálogos de ejemplo:** al menos 3 por agente, incluidos casos difíciles (usuario enojado, deuda no reconocida, tercero que no es el titular, cliente vulnerable, intento de fraude o ingeniería social, usuario que pide una persona de inmediato). AG-CON: brief → plan → pieza → rechazo de cumplimiento → corrección.

### 2.7 Sora Console
- Módulos y pantallas: tenants y operaciones de país; configuración de agentes (persona, intents, políticas, horarios); base de conocimiento; plantillas de WhatsApp; políticas de cobranza (rangos de promesa, planes permitidos); bandeja de handoff (si `[OD-04]` lo requiere); Studio (calendario, piezas, aprobaciones); analítica; auditoría y búsqueda de conversaciones; usuarios y roles; uso y costos.
- Wireframes en texto (ASCII) de las 6 pantallas principales.
- **Matriz RBAC**: roles (admin del tenant, operador de cobranza, agente de CX, marketing, aprobador de contenido, compliance o auditor, FDE de Sora, admin de Sora) × permisos.

### 2.8 Definiciones de métricas de producto
Para cada KPI de `12-CATALOGO` y de `11-VISION §8`: definición formal, fórmula, eventos y fuente de datos que lo alimentan (insumo para WP07), segmentación (tenant, país, agente, intent) y meta del MVP. Incluye la **definición canónica de "conversación"** (inicio, fin y tiempo de inactividad) y de **"resolución"**, porque WP09 las usará para facturar.

### 2.9 Alcance del MVP
- Tabla MoSCoW consolidada por agente y plataforma.
- Qué queda para la fase 1.5, la fase 2 y la fase 3, y por qué.
- Recomendación de producto sobre `[OD-01]` (voz en el MVP) y sobre la activación de pt-BR, desde la óptica de valor para el cliente.

## Diagramas obligatorios
1. `journey` por agente (3) y para el usuario multi-intent (1).
2. `flowchart` del flujo conversacional de los 3 intents más críticos de AG-COB.
3. `flowchart` del árbol de decisión de escalamiento a humano.
4. `stateDiagram-v2` del ciclo de vida de una pieza de contenido (borrador → en revisión → requiere cambios → aprobada → programada → publicada → medida).

## Criterios de aceptación
- [ ] ≥ 8 personas, ≥ 5 journeys con diagrama.
- [ ] Mínimos de intents cumplidos, todos con MoSCoW, auth y herramientas.
- [ ] Cada RF es trazable a intent y C-xx; 20 en Gherkin, con ≥ 3 negativos.
- [ ] Todos los RNF son medibles.
- [ ] Hay ejemplos multi-idioma y multi-país en el diseño conversacional.
- [ ] Hay matriz RBAC y 6 wireframes.
- [ ] "Conversación" y "resolución" tienen definición canónica.
- [ ] Hay alcance del MVP explícito, con fases.

## Fuera de alcance
Arquitectura y servicios (WP03), diseño interno de agentes y prompts (WP04), integraciones técnicas (WP05) y precios (WP09).
