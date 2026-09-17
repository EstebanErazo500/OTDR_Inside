<h1 align="center">Procedencia de eventos · modelo documentado</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
  <img src="../assets/nav/procedencia-eventos-current.svg" alt="Procedencia de eventos · página actual">
</p>

<p align="center">
  <a href="event-provenance.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

<p align="center"><strong>El origen forma parte de los datos. La confianza y la revisión son dimensiones diferentes.</strong></p>

> **Alcance.** Esta página documenta el modelo de procedencia representado por las entregas archivadas reconstruidas hasta **v0.3.5**. Explica cómo OTDR Inside mantiene diferenciada la información almacenada, importada, calculada y revisada manualmente. Las versiones posteriores pueden ampliar el modelo, pero deben conservar el significado histórico de estas categorías.

El análisis OTDR se vuelve ambiguo cuando valores que aparecen juntos en una interfaz se tratan como si provinieran de la misma fuente. OTDR Inside evita esa mezcla conservando **de dónde salió cada valor o evento** junto con el valor mismo.

<p align="center">
  <img src="../assets/provenance/model-es-light.svg#gh-light-mode-only" alt="Modelo de procedencia de eventos de OTDR Inside en modo claro" width="100%">
  <img src="../assets/provenance/model-es-dark.svg#gh-dark-mode-only" alt="Modelo de procedencia de eventos de OTDR Inside en modo oscuro" width="100%">
</p>

## Cuatro orígenes conceptuales

| Origen | Qué significa | Qué no significa |
|---|---|---|
| **Almacenado en SOR** | El valor o evento está serializado en la estructura SOR original, por ejemplo una tabla `KeyEvents` cuando existe. | No se convierte automáticamente en verdad universal para cualquier fabricante o campo semántico. |
| **Importado de EI** | El registro fue leído desde un diseño EI soportado. Cuando aparece como compañero verificado, las comprobaciones de pareja EI/SOR fueron superadas. | La información EI no sobrescribe al SOR ni se transforma silenciosamente en información almacenada en SOR. |
| **Candidato calculado** | La cadena de análisis derivó una hipótesis desde la traza reconstruida y la evidencia que la rodea. | No fue serializado por el equipo solo porque aparezca junto a eventos almacenados. |
| **Revisión manual** | Una persona añadió estado de revisión, comentarios o anotaciones durante la sesión de análisis. | La revisión humana no reescribe la medición original ni cambia el origen histórico del evento subyacente. |

El modelo no ordena estos orígenes de “mejor” a “peor”. Responden preguntas diferentes. La procedencia registra **fuente**; la evidencia, el soporte semántico y la revisión describen otras dimensiones.

## Procedencia, evidencia, revisión y confianza no son sinónimos

OTDR Inside mantiene separados cuatro conceptos relacionados:

| Dimensión | Pregunta que responde |
|---|---|
| **Procedencia** | ¿De dónde provino esta información? |
| **Evidencia** | ¿Qué mediciones, relaciones o contexto local respaldan la interpretación? |
| **Estado de revisión** | ¿Una persona revisó el elemento y cuál fue su decisión? |
| **Confianza / soporte semántico** | ¿Qué tan respaldado está el significado mostrado para este formato o perfil? |

Un candidato calculado puede tener evidencia local fuerte y seguir siendo **calculado**. Un evento almacenado puede estar serializado en el archivo y aun requerir interpretación semántica específica del fabricante. Un candidato aceptado manualmente continúa siendo un candidato calculado con estado de revisión aceptado; la revisión no reescribe la procedencia.

## Ruta SOR · información almacenada y traza reconstruida

La ruta SOR puede exponer dos clases de información al mismo tiempo:

1. **Información de eventos almacenada**, cuando el archivo contiene realmente una estructura como `KeyEvents`.
2. **Muestras de traza**, que pueden reconstruirse y analizarse independientemente de que exista una tabla de eventos.

Esta diferencia es especialmente importante para la familia Ceyear caracterizada. La ausencia de `KeyEvents` significa que **no se encontró una tabla KeyEvents almacenada en ese SOR**. No demuestra que la traza óptica carezca de eventos.

Por eso, el análisis calculado permanece después de la reconstrucción de traza y recibe su propia procedencia en lugar de insertarse dentro de la categoría de eventos almacenados.

## Ruta EI · información importada con estado de pareja explícito

Desde v0.3.3, `.EI` constituye una segunda ruta de entrada. La información EI conserva su propio origen tanto si el archivo se abre de forma independiente como si se utiliza como compañero verificado.

Para la familia EI/SOR caracterizada, la pareja no se acepta únicamente por semejanza del nombre. La implementación comprueba la relación observada entre muestras junto con metadatos de adquisición antes de considerar el EI como fuente compañera verificada.

Esa diferencia se conserva en el visor:

- **EI abierto de forma independiente** — los registros pueden inspeccionarse como información importada de EI, mientras la sesión declara que no existe una pareja SOR verificada.
- **Pareja EI/SOR verificada** — la información importada desde EI puede compararse con el SOR y con el análisis calculado, pero su origen continúa siendo EI.

La verificación de pareja fortalece la relación entre los archivos; no fusiona sus procedencias.

## Candidatos calculados · el análisis permanece visiblemente calculado

La línea 0.3 incorpora hipótesis de evento derivadas de la curva reconstruida. La detección, el refinamiento contextual y el análisis de evidencia pueden fortalecer o suprimir una propuesta, pero ninguna de esas etapas la convierte en información almacenada por el equipo.

El ciclo conceptual es:

`traza → propuesta → análisis contextual → evidencia → candidato calculado → revisión humana opcional`

Los candidatos suprimidos pueden conservar motivos diagnósticos y contexto de características, de forma que una decisión negativa siga siendo explicable en lugar de desaparecer silenciosamente.

## v0.3.5 · evidencia terminal sin inflar la procedencia

v0.3.5 añade una etapa separada de evidencia alrededor de la región terminal de diagnóstico. Una transición puede promoverse a **posible candidato de final no reflectivo** únicamente cuando coincide la evidencia terminal empleada por la implementación, incluida la conducta relevante del borde o transición, la caída relativa y el contexto de ruido posterior persistente.

Si esa evidencia no coincide, la región permanece como diagnóstico **D** en lugar de forzarse dentro de una etiqueta de evento.

Lo más importante es que un resultado terminal promovido mantiene **procedencia calculada**. La evidencia adicional modifica el soporte de la hipótesis; no convierte esa hipótesis en un evento almacenado por el instrumento ni en un final físico de fibra certificado.

El modelo documentado conserva entonces esta relación:

`diagnóstico terminal D → compuerta de evidencia → posible candidato de final no reflectivo (CALCULADO)`

no:

`diagnóstico terminal D → final de fibra certificado`

## Revisión humana · la evaluación se superpone al origen, no lo reemplaza

La revisión humana se separó explícitamente de la detección en v0.3.1. El estado de revisión puede registrar valores como `pending`, `accepted` o `rejected`, junto con comentarios e historial.

Esto permite combinaciones como:

| Procedencia | Estado de revisión | Interpretación |
|---|---|---|
| Candidato calculado | Pendiente | Propuesta algorítmica a la espera de evaluación humana. |
| Candidato calculado | Aceptado | Propuesta algorítmica aceptada por una persona; sigue siendo calculada. |
| Candidato calculado | Rechazado | La propuesta se conserva históricamente pero no fue aceptada por el revisor. |
| Importado de EI | Revisado | Registro importado de EI con una evaluación humana adicional; el origen sigue siendo EI. |
| Almacenado en SOR | Revisado | Información de fuente almacenada con contexto de revisión; el origen sigue siendo SOR. |

La capa de revisión es aditiva. Nunca modifica la medición fuente.

## Cómo evolucionó el modelo de procedencia

| Versión | Cambio relacionado con procedencia |
|---|---|
| **v0.1.0** | Los eventos EXFO almacenados y la información estructural/normalizada ya se distinguen de la representación cruda del archivo. |
| **v0.3.0** | Los candidatos derivados de la curva pasan a ser una fuente calculada explícita, separada de los eventos almacenados. |
| **v0.3.1** | Evidencia y revisión humana se convierten en capas independientes alrededor de los candidatos calculados. |
| **v0.3.3** | EI aparece como segunda fuente importada con estado de pareja explícito. |
| **v0.3.5** | La evidencia terminal puede respaldar un posible candidato de final no reflectivo sin dejar de mantenerlo dentro del origen calculado. |

## Contrato de presentación y exportación

El visor y la migración pública deberían conservar varios invariantes:

- el origen de un evento permanece visible cuando distintas fuentes se muestran juntas;
- los registros almacenados y calculados no se fusionan silenciosamente dentro de una misma categoría semántica;
- el estado de pareja EI se mantiene separado de la propia procedencia EI;
- la revisión humana añade estado y comentario sin reescribir el origen de la fuente;
- los valores crudos y la semántica no soportada permanecen disponibles cuando el modelo lo permite;
- la evidencia terminal puede fortalecer una hipótesis calculada sin convertirla en una afirmación física certificada.

Estas reglas aplican tanto a la interfaz como a las exportaciones de análisis/eventos, de modo que la procedencia sobreviva más allá de la pantalla donde se inspeccionó inicialmente el evento.

## Qué no afirma este modelo

La procedencia no es una puntuación de verdad. Un valor almacenado no es automáticamente más correcto físicamente que cualquier resultado calculado; una revisión aceptada no es una nueva medición; un registro importado desde EI no equivale a un evento almacenado en SOR; y un candidato terminal de v0.3.5 no es una declaración calibrada del extremo físico de la fibra.

El objetivo de la procedencia es más concreto y más útil: **hacer explícito el origen de cada dato para que la interpretación, validación y revisión posteriores no borren cómo fue obtenido.**

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
</p>

<p align="center"><sub>Siguiente página independiente de documentación: Validación.</sub></p>
