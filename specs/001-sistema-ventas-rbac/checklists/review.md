# Review Checklist: Sistema de Ventas con RBAC y Facturación

**Purpose**: Validar la **calidad de los requisitos** del feature 001 antes de release (gate de aceptación para Stakeholder/QA). Este checklist pone a prueba cómo están escritos los requisitos — no si el código funciona. Úsalo para confirmar que cada item del spec sea completo, claro, medible, consistente y libre de ambigüedad antes de firmar la aprobación de release.

**Created**: 2026-04-19
**Feature**: [spec.md](../spec.md)
**Audiencia**: Stakeholder / QA (aceptación pre-release)
**Profundidad**: Estándar (25 items)

## Requirement Completeness

- [ ] CHK001 ¿Las 5 historias de usuario (US1–US5) tienen **Why this priority**, **Independent Test** y ≥1 escenario Given/When/Then declarados? [Completeness, Spec §User Scenarios]
- [ ] CHK002 ¿Hay requisitos documentados para todos los flujos de fallo listados en Edge Cases (stock insuficiente, SMTP caído, usuario borrado con sesión activa, factura ya anulada, contraseña temporal vencida)? [Coverage, Spec §Edge Cases]
- [ ] CHK003 ¿Existen requisitos explícitos de accesibilidad (navegación por teclado, lectores de pantalla, contraste mínimo) para las pantallas del sistema, o se declara formalmente fuera de alcance? [Gap]
- [ ] CHK004 ¿Se documenta como requisito (no sólo como Assumption) la existencia de un usuario administrador semilla al desplegar? [Completeness, Spec §Assumptions]
- [ ] CHK005 ¿Los requisitos cubren escenarios de concurrencia entre usuarios (dos vendedores editando la misma factura, o modificación de permisos mientras un usuario los consume)? [Coverage, Gap]

## Requirement Clarity

- [ ] CHK006 ¿La cláusula "contraseñas triviales" en FR-012 está respaldada por una lista negra concreta en el nivel de requisito, o queda a criterio del implementador? [Ambiguity, Spec §FR-012]
- [ ] CHK007 ¿Qué se considera un "mensaje operativo" cuando falla la carga de roles/rutas (Edge Case + FR-010) — está especificado el contenido o tono esperado? [Clarity, Spec §Edge Cases]
- [ ] CHK008 ¿"Datos relacionados de forma legible" (FR-019) está concretado con las columnas exactas a mostrar en los listados de Cliente y Vendedor? [Clarity, Spec §FR-019]
- [ ] CHK009 ¿La ventana de 24 horas para la contraseña temporal es un requisito vinculante o un default ajustable sin control de cambios? [Ambiguity, Spec §Assumptions]
- [ ] CHK010 ¿"Mensaje neutro" en recuperación (FR-015) está definido con el texto o patrón exacto, o queda abierto a interpretación? [Clarity, Spec §FR-015]

## Requirement Consistency

- [ ] CHK011 ¿FR-003 (sesión mantenida entre peticiones) es consistente con la Assumption de "timeout 30 minutos de inactividad"? [Consistency, Spec §FR-003 vs §Assumptions]
- [ ] CHK012 ¿Los requisitos RBAC (FR-006..FR-010) son coherentes con el Edge Case "cambio de permisos con sesión abierta aplica al siguiente login"? [Consistency, Spec §Edge Cases]
- [ ] CHK013 ¿Las reglas de CRUD (FR-016..FR-020) son uniformes entre los 7 catálogos, o hay divergencias no justificadas (ej. confirmación obligatoria sólo en algunos)? [Consistency, Spec §FR-016..020]
- [ ] CHK014 ¿Hay coherencia entre FR-005 (cambio obligatorio bloquea navegación) y FR-008 (rutas públicas) en lo relativo a cómo se trata `/cambiar-contrasena` durante el estado "temporal"? [Consistency, Spec §FR-005 vs §FR-008]

## Acceptance Criteria Quality

- [ ] CHK015 ¿Cada escenario de aceptación en las 5 US es independientemente verificable sin depender de otra US? [Acceptance Criteria, Spec §User Scenarios]
- [ ] CHK016 ¿Cada Success Criterion (SC-001..SC-010) puede medirse objetivamente por un QA sin leer el código? [Measurability, Spec §Success Criteria]
- [ ] CHK017 ¿La métrica de "5 minutos" en SC-007 está definida como tiempo de reloj total desde la solicitud hasta la nueva contraseña, o sólo la fase post-login? [Clarity, Spec §SC-007]

## Scenario Coverage

- [ ] CHK018 ¿Hay requisitos que describan el comportamiento de rollback si el envío SMTP falla después de persistir la contraseña temporal (más allá del Edge Case general)? [Coverage, Exception Flow]
- [ ] CHK019 ¿Está especificado cómo debe comportarse el sistema ante una contraseña temporal **reutilizada** después de un cambio exitoso (caso distinto a "vencida")? [Coverage, Gap]
- [ ] CHK020 ¿Los requisitos de consulta histórica de facturas anuladas definen filtros, orden o paginación para uso auditor, o sólo "permanecen visibles" (FR-032)? [Gap, Spec §FR-032]

## Non-Functional Requirements

- [ ] CHK021 ¿Las metas de rendimiento (SC-002 login ≤5s, SC-004 factura ≤3s) están calificadas con supuestos de carga (ej. N usuarios concurrentes) o son puntuales en vacío? [Completeness, Spec §SC-002 y §SC-004]
- [ ] CHK022 ¿La retención de 7 años (SC-003) aplica sólo a facturas anuladas o alcanza también a facturas borradas físicamente por admin y a otros documentos? [Clarity, Spec §SC-003 vs §FR-034]
- [ ] CHK023 ¿Los requisitos de seguridad (hash irreversible FR-013, neutralidad FR-002/FR-015, RBAC por ruta FR-007) están trazados a un modelo de amenazas explícito o implícito documentado? [Traceability, Gap]

## Dependencies & Assumptions

- [ ] CHK024 ¿La dependencia con la API externa `ApiGenericaCsharp` está documentada como requisito (versiones/endpoints/SPs mínimos) y no sólo como supuesto? [Dependency, Spec §Assumptions]
- [ ] CHK025 ¿El compromiso con la identidad Zenith (`Manual_de_Marca_Zenith.md`) está reflejado en requisitos funcionales explícitos o delegado únicamente a la constitución? [Traceability, Gap]

## Notes

- Marca con `[x]` sólo cuando el requisito esté **escrito con la calidad suficiente**, no cuando el código esté hecho.
- Si un item falla, la acción es **actualizar `spec.md`** (o `/speckit-clarify` para ambigüedades específicas), no marcarlo arbitrariamente como OK.
- Cobertura con referencias de trazabilidad: 25/25 = **100%** (todos los items citan `[Spec §…]`, `[Gap]`, `[Ambiguity]`, `[Conflict]`, `[Assumption]`, `[Dependency]` o `[Traceability]`).
- Este checklist se puede ejecutar de forma incremental: al resolver un ítem, añadir en la línea la referencia a la versión de spec.md que lo fijó (ej. `CHK006 ✅ resuelto en spec.md §FR-012 v2`).
