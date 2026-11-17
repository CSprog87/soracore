# WP11 · Requisitos internos de Sora Valley (equipo, cuentas, proveedores, operación)

| Campo | Valor |
|---|---|
| Oleada | 2 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/` y `03-entregables/_GATE-1.md` |
| Insumos de otros WPs | **WP03** (cuentas, servicios, cuotas), **WP05** (proveedores de canal, Tech Provider, voz), **WP06** (certificaciones, contratos, legal por país); WP01 opcional (modelo operativo de los líderes) |
| Archivo de salida | `03-entregables/WP11-requisitos-internos.md` |
| Acceso web | Recomendado: programas de AWS para startups e ISVs, requisitos de Meta Tech Provider, costos de herramientas SaaS |
| Extensión orientativa | 5,000–7,000 palabras |
| Dueño de | Catálogo RI-xx, organización y roles, prerequisitos con AWS, Meta y terceros, procesos operativos, presupuesto de herramientas no-cloud |

## Rol
Eres COO o Head of Platform de una startup B2B de IA que va a operar servicios gestionados en varios países de LatAm.

## Objetivo
Definir **todo lo que Sora Valley necesita tener o hacer internamente** para construir, vender y operar Sora Agents Platform: personas, cuentas, programas de proveedores, legal y corporativo, herramientas, procesos y presupuesto no-cloud.

## Secciones obligatorias

### 2.1 Catálogo de requisitos internos (RI-xx)
Tabla: ID · categoría · requisito · detalle · cuándo se necesita (antes del build, durante el build, antes del piloto, en escala) · costo estimado · dueño interno · bloquea (Sí/No).
Categorías mínimas: organización y talento; AWS; Meta y canales; legal y corporativo por país; seguridad y certificaciones; herramientas; procesos operativos; finanzas y facturación; comercial.

### 2.2 Organización y roles
- Roles necesarios (de `[S-50]`…`[S-59]` y otros que detectes): responsabilidades, perfil, seniority, si es tiempo completo o parcial, y si conviene contratar o tercerizar.
- **Equipo del MVP** frente al **equipo en ESC-L** frente al **equipo en ESC-E** (cuántos de cada rol y cuándo entran).
- **Modelo FDE:** cuántos tenants atiende un FDE en onboarding y en operación; equipo local por país o centralizado; idiomas (portugués para Brasil).
- Organigrama en Mermaid.
- Relación con el equipo de Sora Core (ML): interfaces y responsabilidades compartidas.

### 2.3 Prerequisitos con AWS
Organización y Control Tower; acceso a modelos de Bedrock y **solicitudes de aumento de cuotas** (tomadas de WP03 §2.9) con tiempos; plan de soporte de AWS recomendado (Business o Enterprise On-Ramp) y su costo; programas aplicables (créditos para startups, ISV Accelerate, Marketplace, Generative AI Accelerator u otros vigentes `[VERIFICAR]`); relación con el account team.

### 2.4 Prerequisitos con Meta y canales
Verificación del negocio de Sora, rol de **Tech Provider** o partner (requisitos y tiempos `[VERIFICAR]`), app review para Graph API, relación con el proveedor de WhatsApp elegido (`[OD-05]`), números y telefonía por país (requisitos locales para obtener números, `[VERIFICAR]`).

### 2.5 Legal y corporativo
- Entidad o entidades legales: ¿desde dónde factura Sora a clientes en cada país? Implicaciones fiscales (retenciones, impuestos a servicios digitales, establecimiento permanente) `[LEGAL]` `[VERIFICAR]`.
- **Facturación electrónica por país** (por ejemplo, CFDI, DIAN, NF-e/NFS-e) y proveedores para emitirla.
- Plantillas contractuales: MSA, DPA, SLA, AUP y orden de servicio. Asesoría legal por país (presupuesto, `[S-59]`).
- Seguros: ciberseguro y responsabilidad profesional (E&O).
- Respuesta a la pregunta crítica de WP06 sobre el registro como agencia de cobranza.

### 2.6 Seguridad y certificaciones
Programa de seguridad mínimo antes del piloto (políticas, gestión de accesos, gestión de vendors, capacitación). Roadmap de SOC 2 e ISO 27001 (de WP06), con costos de auditor y de herramienta de cumplimiento (tipo Vanta o Drata, `[VERIFICAR]`).

### 2.7 Herramientas internas
Repositorios y CI/CD, gestión de proyectos, documentación, on-call (PagerDuty, Opsgenie o alternativas), status page, CRM comercial, soporte al cliente (tickets), herramienta de evals y observabilidad complementaria (de WP07), gestión de secretos de equipo. Tabla con el costo mensual por escenario.

### 2.8 Procesos operativos
Define (con un diagrama para los 3 más críticos):
- On-call y gestión de incidentes (severidades alineadas con WP06).
- **Release de prompts y configuraciones** (quién propone, evals, aprobación, despliegue, rollback).
- Onboarding de tenants (interno, complemento de WP10).
- Soporte al cliente: niveles L1 a L3, SLAs por plan y horarios por zona horaria (`[S-41]`).
- Revisión periódica de calidad y cumplimiento por tenant (QBR).
- Gestión de cambios de las políticas de Meta y de regulación por país (vigilancia regulatoria).
- Gestión de costos (FinOps) mensual por tenant.

### 2.9 SLAs que Sora ofrecerá
Por plan: disponibilidad, tiempo de respuesta ante incidentes, soporte, créditos por incumplimiento. Coherente con `[S-37]` y con WP03.

### 2.10 Presupuesto interno no-cloud
Tabla por escenario (build, ESC-P, ESC-L, ESC-E) de herramientas, legal, certificaciones, seguros, soporte de AWS y otros. Lo usa WP12. Las personas se cuentan en WP12, aquí solo la estructura del equipo.

## Diagramas obligatorios
1. Organigrama (MVP y ESC-E). 2. Proceso de release de prompts y configuraciones. 3. Proceso de gestión de incidentes. 4. Proceso de soporte L1 a L3.

## Criterios de aceptación
- [ ] ≥ 40 RI-xx en todas las categorías, con costo y dueño.
- [ ] Equipo por fase con el número de personas por rol.
- [ ] Prerequisitos con AWS y Meta con tiempos.
- [ ] Legal, corporativo y facturación por país identificados con `[LEGAL]`.
- [ ] Procesos operativos definidos y diagramados.
- [ ] Presupuesto no-cloud por escenario.

## Fuera de alcance
Requisitos al cliente (WP10), costos de nube (WP08) y plan de construcción (WP12).
