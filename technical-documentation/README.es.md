<h1 align="center">OTDR Inside · Documentación técnica</h1>

<p align="center">
  <a href="project.es.md"><img src="assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="how-it-works.es.md"><img src="assets/nav/como-funciona.svg" alt="Recorrido guiado"></a>
  <a href="compatibility-matrix.es.md"><img src="assets/nav/compatibilidad.svg" alt="Matriz de compatibilidad"></a>
  <a href="case-studies.es.md"><img src="assets/nav/casos-estudio.svg" alt="Casos de estudio"></a>
  <a href="version-history.es.md"><img src="assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
  <a href="architecture.es.md"><img src="assets/nav/arquitectura.svg" alt="Arquitectura"></a>
  <a href="event-provenance.es.md"><img src="assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <a href="validation.es.md"><img src="assets/nav/validacion.svg" alt="Validación"></a>
  <a href="data-handling.es.md"><img src="assets/nav/manejo-datos.svg" alt="Manejo de datos"></a>
</p>

<p align="center">
  <a href="README.md"><img src="assets/nav/lang-en.svg" alt="English"></a>
  <a href="README.es.md"><img src="assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

Este directorio contiene la documentación técnica pública de OTDR Inside.

El registro reconstruido cubre v0.1.0 hasta la **candidata v0.4.7 del 19 de noviembre de 2026**, incluidas las revisiones intermedias de 4.6. La aplicación independiente para Windows y sus ajustes posteriores quedan pendientes de inspección.

La documentación conserva la evolución técnica. El alcance actual permite capturas revisadas del visor, arquitectura conceptual y diagramas generales/sintéticos independientes; **no permite el código fuente de la aplicación ni mediciones operativas privadas**.

## Mapa de documentación

| Página | Propósito |
|---|---|
| <a href="how-it-works.es.md"><img src="assets/nav/como-funciona.svg" alt="Recorrido guiado"></a> | Flujo del operador en cinco etapas: entradas caracterizadas, evidencia, revisión LSA/IA, comparación e informes (candidata v0.4.7); sin código público ni capturas inventadas. |
| <a href="compatibility-matrix.es.md"><img src="assets/nav/compatibilidad.svg" alt="Matriz de compatibilidad"></a> | Alcance por fabricante y formato de EXFO, Ceyear, Yokogawa, JDSU, NOVKER y F2H estructural, estados por capacidad, evidencia, precondiciones y variantes no demostradas. |
| <a href="case-studies.es.md"><img src="assets/nav/casos-estudio.svg" alt="Casos de estudio"></a> | Concordancia de posiciones, pérdidas LSA, lectores antiguos y localización consultiva con ejemplos sintéticos, evidencia archivada y estado de desarrollo. |
| <a href="version-history.es.md"><img src="assets/nav/historial-versiones.svg" alt="Historial de versiones"></a> | Evolución de ingeniería hasta candidata v0.4.7, incluidas Yokogawa/JDSU/SOZ, estimación de pérdidas y límites de la evidencia. |
| <a href="architecture.es.md"><img src="assets/nav/arquitectura.svg" alt="Arquitectura"></a> | Evolución hasta la base multiarchivo 4.1, auditoría 4.2 y ampliaciones 4.3–4.7 de lectores, IA, revisión y comparación. |
| <a href="event-provenance.es.md"><img src="assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a> | Modelo de procedencia de entradas caracterizadas y posiciones nativas/calculadas; asesoría RF y pérdida nativa frente a LSA separadas. |
| <a href="validation.es.md"><img src="assets/nav/validacion.svg" alt="Validación"></a> | Validación hasta candidata v0.4.7, discrepancias de referencias, pruebas archivadas y límites IA/LSA. |
| <a href="data-handling.es.md"><img src="assets/nav/manejo-datos.svg" alt="Manejo de datos"></a> | Frontera de permisos, revisión de capturas e ilustraciones sintéticas y restricciones de código fuente/mediciones. |

## Principios de publicación

- **la estructura no es la semántica**;
- **lo almacenado no es lo calculado**;
- **cercanía no es identidad**;
- **desconocido no significa corrupto**;
- las afirmaciones permanecen ligadas a evidencia reproducible y a la versión que las produjo;
- no es necesario publicar mediciones operativas para explicar la arquitectura pública.

---

Siguiente paso: verificar la compilación definitiva de Windows e incorporar capturas autorizadas. La candidata no se presenta como el ejecutable final.
