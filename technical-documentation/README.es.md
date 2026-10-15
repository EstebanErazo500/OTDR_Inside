<h1 align="center">OTDR Inside · Documentación técnica</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
  <a href="data-handling.es.md"><img src="../assets/nav/manejo-datos.svg" alt="Manejo de datos"></a>
</p>

<p align="center">
  <a href="README.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="README.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

Este directorio contiene la documentación técnica pública de OTDR Inside.

El registro reconstruido cubre actualmente las entregas archivadas desde **v0.1.0 hasta v0.4.2**. Ese rango corresponde al material analizado para el repositorio público y no al final del proyecto. Las versiones posteriores deben ampliar la historia sin reescribir lo que realmente demostró cada entrega anterior.

La documentación conserva la evolución técnica. El alcance actual permite capturas revisadas del visor, arquitectura conceptual y diagramas generales/sintéticos independientes; **no permite el código fuente de la aplicación ni mediciones operativas privadas**.

## Mapa de documentación

| Página | Propósito |
|---|---|
| <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a> | Evolución de ingeniería hasta v0.4.2, incluidas las revisiones documentadas de presentación y localización experimental. |
| <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a> | Evolución arquitectónica con base detallada v0.4.1 y adiciones documentadas en 4.2 de evidencia, localización e informes. |
| <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a> | Modelo de eventos consciente de la fuente y conservación de procedencia en parejas EI/SOR, pestañas y comparación entre trazas. |
| <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a> | Capas de validación, evidencia por versión, frontera público/privado y límites de lo que los datos permiten afirmar. |
| <a href="data-handling.es.md"><img src="../assets/nav/manejo-datos.svg" alt="Manejo de datos"></a> | Frontera de permisos, revisión de capturas e ilustraciones sintéticas y restricciones de código fuente/mediciones. |

## Principios de publicación

- **la estructura no es la semántica**;
- **lo almacenado no es lo calculado**;
- **cercanía no es identidad**;
- **desconocido no significa corrupto**;
- las afirmaciones permanecen ligadas a evidencia reproducible y a la versión que las produjo;
- no es necesario publicar mediciones operativas para explicar la arquitectura pública.

---

Próxima incorporación pública prevista: una **galería de capturas revisadas del visor**, acompañada de ilustraciones sintéticas independientes cuando sea necesario.
