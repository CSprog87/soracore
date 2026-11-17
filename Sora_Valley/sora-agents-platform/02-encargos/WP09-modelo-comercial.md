# WP09 · Modelo comercial y pricing (Agents-as-a-Service)

| Campo | Valor |
|---|---|
| Oleada | 3 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, `03-entregables/_GATE-1.md` y `_GATE-2.md` |
| Insumos de otros WPs | **WP01** (pricing del mercado, posicionamiento), **WP02** (definición de conversación y resolución, alcance), **WP08** (costos por unidad y fijos); WP10 y WP11 opcionales (costo de onboarding y de operación) |
| Archivo de salida | `03-entregables/WP09-modelo-comercial.md` |
| Acceso web | Recomendado: pricing público de comparables, costo de agentes humanos y de despachos de cobranza por país |
| Extensión orientativa | 4,000–6,000 palabras más tablas |
| Dueño de | `[OD-10]` modelo de precio; empaquetado; precios de lista; unit economics y margen; historia de ROI |

## Rol
Eres estratega de pricing y monetización de SaaS B2B con IA, con experiencia en fintech de LatAm.

## Objetivo
Definir **cómo se cobra Sora Agents**: unidad, precios, paquetes, mínimos, reglas, y el **margen resultante** por escenario. Además, la **historia de ROI** que justifica el precio frente a la alternativa humana o al despacho de cobranza, en cada país núcleo.

## Secciones obligatorias

### 2.1 Opciones de modelo de precio (`[OD-10]`)
Evalúa al menos: (a) por conversación; (b) por resolución; (c) por minuto de voz; (d) por resultado en cobranza (% del monto recuperado incremental contra el holdout); (e) suscripción por agente o plataforma; (f) setup o onboarding; (g) híbridos. Para cada una: alineación de incentivos, predictibilidad para el cliente, riesgo para Sora, complejidad de medición (¿se puede medir con lo que define WP07?) y precedentes en el mercado (WP01). Usa una matriz de evaluación con pesos.

### 2.2 Recomendación de modelo y empaquetado
- Modelo recomendado por agente (puede variar: por ejemplo, ATC por resolución y COB con un componente por resultado).
- Paquetes **Agents Starter, Growth y Scale** (`11-VISION §6`) y cómo encajan con los planes Starter, Growth y Scale de la landing, incluida la recomendación sobre vender agentes como **producto independiente o de entrada**.
- Qué incluye cada paquete: agentes, canales, países, volumen incluido, soporte, SLA, FDE y opciones (voz, pt-BR, silo).

### 2.3 Precios de lista
- Precios en **USD** (con su referencia en moneda local según `[S-02]` para MX, CO, BR, PE y CL; AR solo en USD).
- Fee de setup u onboarding por agente y por operación de país adicional.
- Suscripción y volumen incluido, precio del excedente y descuentos por volumen o compromiso.
- Fee por resultado en cobranza, si aplica: base de cálculo, holdout, tope y mínimos.
- **¿Precio único regional o ajustado por país?** Analiza el poder de compra, la competencia local y el costo de canal por país (WP08 §2.5). Recomienda.

### 2.4 Unit economics y margen
Usando WP08 §2.10:
- Margen bruto por unidad (conversación ATC, conversación COB, minuto de voz, pieza) en la mezcla base y en la económica.
- Margen bruto por **paquete** y por **escenario** (ESC-P, ESC-L, ESC-E), incluyendo los costos fijos prorrateados y el costo de operación (FDE y soporte, de WP11 si está disponible).
- Comparación con la meta `[S-65]`. Si no se alcanza, qué palancas (precio, mezcla de modelos, mínimos) la alcanzan.
- Peor caso: un tenant de bajo volumen y alto costo de canal en el país más caro.

### 2.5 Historia de ROI para el cliente
Por país núcleo:
- Costo de la alternativa humana (agentes de CX y de cobranza, `[S-68]` verificado por país) y de los despachos de cobranza (comisión típica por país `[VERIFICAR]`).
- Calculadora de ROI: entradas (volumen, costos actuales, recuperación actual), salidas (ahorro, recuperación incremental, payback).
- Un **ejemplo trabajado** para un tenant ficticio de ESC-L.

### 2.6 Reglas comerciales
Plazo de los contratos, compromisos mínimos, facturación (mensual por adelantado más excedentes vencidos), créditos por SLA, ajuste por inflación o tipo de cambio (`[RIESGO R-18]`), qué pasa con los números de WhatsApp y los datos al terminar, y cláusula de precio piloto.

### 2.7 Pricing del piloto
Propuesta concreta para el cliente de diseño: duración, precio (o descuento), compromisos de ambas partes, métricas de éxito y opción de conversión.

### 2.8 Proyección de ingresos
Ingresos mensuales y anuales por escenario con los precios recomendados. Margen bruto total. Insumo de WP12 para el caso de negocio.

### 2.9 Riesgos del modelo comercial
Canibalización con los otros productos de Sora, disputas sobre la medición del resultado, volatilidad de los costos de canal y del tipo de cambio, y presión competitiva.

## Diagramas obligatorios
1. `flowchart` de la estructura de paquetes y opciones. 2. `pie` o tabla del mix de ingresos esperado en ESC-E.

## Criterios de aceptación
- [ ] `[OD-10]` resuelta con una matriz ponderada.
- [ ] Precios de lista completos en USD, con referencias en moneda local.
- [ ] Margen por unidad, paquete y escenario, comparado con `[S-65]`.
- [ ] ROI por país con un ejemplo trabajado.
- [ ] Propuesta concreta de piloto.
- [ ] Proyección de ingresos por escenario.
- [ ] Todo número trazable a WP08 o a una fuente; nada inventado.

## Fuera de alcance
Costos de infraestructura (WP08), contratos legales en detalle (WP06 y WP11).
