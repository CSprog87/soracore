# WP03 · Arquitectura AWS e infraestructura

| Campo | Valor |
|---|---|
| Oleada | 1 |
| Modelo sugerido | Claude Sonnet 5.5 |
| Lee obligatoriamente | Todo `01-contexto/`, en especial 13, 14 y 15 |
| Insumos de otros WPs | Ninguno (oleada 1) |
| Archivo de salida | `03-entregables/WP03-arquitectura-aws.md` |
| Acceso web | Recomendado: disponibilidad de servicios por región, cuotas, límites |
| Extensión orientativa | 6,000–9,000 palabras más diagramas |
| Dueño de | `[OD-02]` vector store, `[OD-06]` región y celdas, `[OD-07]` stack de consola; confirmación de `[ADR-09]` |

## Rol
Eres arquitecto de soluciones AWS senior (nivel Professional), con experiencia en SaaS multi-tenant, plataformas de IA generativa y requisitos de residencia de datos en LatAm.

## Objetivo
Diseñar la **arquitectura AWS de referencia del MVP**, lista para construir y preparada para crecer a varias celdas regionales y a cientos de miles de conversaciones mensuales: servicios, red, cuentas, multi-tenancy, región y residencia, resiliencia, capacidad, CI/CD e IaC.

## Secciones obligatorias

### 2.1 Principios y drivers de arquitectura
Lista priorizada: aislamiento de tenants, residencia por país, serverless-first, costo variable, observabilidad, cumplimiento, time-to-market. Explica qué trade-offs implica cada uno.

### 2.2 Vistas C4 (con flowchart de Mermaid)
- **Contexto (nivel 1):** Sora Agents Platform, actores y sistemas externos (Meta, telefonía, sistemas del tenant, Sora Core, PSPs, AWS Bedrock).
- **Contenedores (nivel 2):** los 32 componentes C-xx agrupados por plano.

### 2.3 Arquitectura AWS detallada por plano
Para cada plano (canales, ingesta, agentes, workflows, integración, humano, datos, confianza y facturación):
- Un diagrama con **servicios AWS concretos** e IDs C-xx.
- La tabla de servicios:

| C-xx | Servicio AWS | Función | Configuración clave del MVP | Alternativa considerada y por qué no | Disponible en `sa-east-1` | Disponible en `mx-central-1` | Driver de costo |
|---|---|---|---|---|---|---|---|

### 2.4 Estructura de cuentas y gobierno
AWS Organizations y Control Tower; OUs; cuentas por entorno y por celda; SCPs clave (bloqueo de regiones no permitidas, protección de CloudTrail, restricción de modelos de Bedrock); IAM Identity Center; relación con las cuentas de Sora Core; opción silo por tenant.

### 2.5 Red
VPC por entorno y celda; subnets; **VPC endpoints o PrivateLink** para Bedrock, AgentCore, DynamoDB, S3, Secrets Manager y otros; NAT con **IPs elásticas fijas por celda** para el allowlisting de los tenants; conectividad opcional por VPN o PrivateLink con tenants grandes; control de egress; DNS. Incluye el diagrama de red.

### 2.6 Multi-tenancy y multi-país
- Modelo pool del MVP y camino a silo (`[ADR-06]`).
- Jerarquía **tenant → operación de país** (`[S-07]`) y cómo se propaga el contexto (`tenant_id`, `country_op_id`) en cada salto: claims del JWT, atributos de sesión de AgentCore, mensajes de SQS, eventos de EventBridge, trazas.
- Aislamiento por capa: DynamoDB (diseño de llaves más condiciones IAM tipo `dynamodb:LeadingKeys`); S3 (prefijos, políticas de bucket, KMS por tenant); Knowledge Bases (por tenant y país, o filtros de metadatos, con su justificación); AgentCore Memory (namespaces); Gateway (targets y credenciales por tenant); Secrets Manager; logs y trazas (redacción y particionamiento).
- Noisy neighbor: cuotas y throttling por tenant, aislamiento de capacidad de Bedrock.
- **Pruebas automáticas de aislamiento** que deben correr en CI.

### 2.7 Regiones, celdas y residencia (`[OD-06]`)
- **Matriz de disponibilidad** de cada servicio requerido en `sa-east-1`, `mx-central-1`, la región de Chile (si existe) y `us-east-1`, con fuente y fecha.
- **Disponibilidad de los modelos Claude** (Sonnet 5.5, Haiku 5.5, Opus 5.5) por región, y los perfiles de inferencia (en región, geográfico o global) accesibles desde cada región LatAm `[VERIFICAR]`.
- El modelo de celdas de `15-ARQUITECTURA §6`: valídalo, define qué datos viven en el plano de control global (sin datos personales) y cuáles en la celda, y cómo se asigna la región hogar a una operación de país.
- **Recomendación de la celda del MVP**, con trade-offs de latencia, costo, disponibilidad y residencia. Coordina tus supuestos con lo que WP06 analizará; deja explícitas las preguntas legales.

### 2.8 Modelo de datos operativo
- Entidades: Tenant, CountryOperation, Channel, AgentConfig (versionada), Customer (referencia, mínimo de datos), Conversation, Message, Session, ToolCall, Handoff, Campaign, ContactAttempt, PromiseToPay, PaymentPlan, ContentPiece, Approval, UsageEvent y AuditEvent.
- `erDiagram` de Mermaid.
- Diseño de tablas de DynamoDB (single-table o múltiples, con justificación), llaves, GSIs y TTL.
- Layout de S3 (buckets y prefijos por tenant, país y fecha).
- Qué datos personales se guardan, cuáles **no** (minimización) y dónde.

### 2.9 Escalabilidad y capacidad por escenario
Para ESC-P, ESC-L y ESC-E: mensajes por segundo en pico (`[S-36]`), RPM y TPM de Bedrock por modelo, sesiones concurrentes de AgentCore, concurrencia de Lambda, throughput de WhatsApp por número, llamadas de voz concurrentes. Tabla de **cuotas que hay que solicitar** a AWS, con valor y timing.

### 2.10 Resiliencia y continuidad
SLOs (`[S-37]`), Multi-AZ, RPO y RTO por componente, backups (PITR de DynamoDB, versionado de S3), estrategia de DR (backup and restore o pilot light; justifica para el MVP), **degradación elegante** (fallback de modelo, cola si cae una integración, mensaje al usuario, circuit breakers) y runbooks mínimos.

### 2.11 Entornos, CI/CD y despliegue
- dev, staging y prod, más sandbox por tenant (`[S-40]`).
- Pipelines separados para: infraestructura, servicios (Lambdas, contenedores), **agentes** (contenedores en AgentCore Runtime), **configuraciones y prompts** (C-32), y bases de conocimiento.
- **Gates de evals** antes de promover (lo define WP07; aquí se integra al pipeline).
- Estrategia de despliegue: canary por tenant, feature flags (AppConfig) y rollback.
- Diagrama del pipeline.

### 2.12 IaC
Herramienta (`[ADR-09]`: confirmar CDK o justificar Terraform), estructura de repositorios y módulos, manejo de configuración por celda y por entorno.

### 2.13 Seguridad de la arquitectura (resumen)
Resumen de los controles de infraestructura y su ubicación. El detalle lo hace WP06; aquí se asegura que la arquitectura los soporte.

## Diagramas obligatorios (mínimo 10)
1. C4 de contexto. 2. C4 de contenedores. 3. Infraestructura AWS completa (puede dividirse por plano). 4. Red. 5. Cuentas y OUs. 6. Aislamiento multi-tenant (cómo viaja el `tenant_id` y dónde se aplica). 7. Celdas regionales y plano de control. 8. Secuencia de un mensaje entrante de WhatsApp **a nivel de servicios AWS concretos**. 9. Pipeline de CI/CD con gates. 10. `erDiagram` del modelo de datos. Y, si cabe, DR.

## Criterios de aceptación
- [ ] Los 32 C-xx están mapeados a servicios concretos (o justificado si alguno sobra o falta).
- [ ] Cada servicio tiene su disponibilidad por región verificada o marcada `[VERIFICAR]`.
- [ ] `[OD-02]`, `[OD-06]` y `[OD-07]` están resueltas con recomendación y trade-offs.
- [ ] Hay capacidad y cuotas por escenario.
- [ ] El aislamiento multi-tenant está especificado en cada capa y tiene pruebas definidas.
- [ ] La jerarquía tenant → operación de país está soportada de punta a punta.
- [ ] ≥ 10 diagramas válidos.
- [ ] No contradice las ADR sin `[CAMBIO-ADR]`.

## Fuera de alcance
Diseño interno de agentes y prompts (WP04), protocolos de canales (WP05), costos detallados (WP08; aquí solo se identifican los drivers de costo), detalle legal (WP06).
