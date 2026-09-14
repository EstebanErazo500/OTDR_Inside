<h1 align="center">Evolución de arquitectura · v0.1.0 → v0.3.4</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
  <img src="../assets/nav/arquitectura-current.svg" alt="Arquitectura · página actual">
</p>

<p align="center">
  <a href="architecture.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="architecture.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

<p align="center"><strong>Primero leer los bytes. Interpretar únicamente cuando la evidencia lo permite.</strong></p>

OTDR Inside no llegó directamente a la arquitectura de v0.3.4. El sistema actual surgió conservando una frontera estructural SOR estable y añadiendo alrededor de ese núcleo semántica de fabricante, análisis de eventos, procedencia y una segunda ruta de entrada EI. Por eso esta página muestra tanto **cómo evolucionó la arquitectura** como **cómo está organizado el sistema vigente en v0.3.4**.

## Evolución de la arquitectura

<p align="center">
  <img src="../assets/architecture/evolution-es-light.svg#gh-light-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo claro" width="100%">
  <img src="../assets/architecture/evolution-es-dark.svg#gh-dark-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo oscuro" width="100%">
</p>

Las nueve entregas archivadas no representan nueve reescrituras arquitectónicas. Algunas versiones robustecieron la ejecución manteniendo deliberadamente intacto el núcleo de análisis. Las transiciones arquitectónicas relevantes son:

| Versión | Estado arquitectónico | Qué cambió estructuralmente |
|---|---|---|
| **v0.1.0** | Rebanada vertical estable | Establece la cadena principal: lectura SOR segura → interpretación por perfiles → normalización → reconstrucción de traza → visor local. EXFO actúa como referencia validada y Ceyear permanece preliminar. |
| **v0.1.1** | Misma arquitectura de análisis | Añade diagnóstico de arranque, comprobación de Python y puertos locales alternativos. El motor SOR, perfiles y ruta de traza no se rediseñan. |
| **v0.1.2** | Misma arquitectura de análisis | Sustituye lanzadores de shell por una tarea de VS Code compatible con Smart App Control. Cambia el despliegue, no el flujo de datos principal. |
| **v0.2.0** | Capa de evidencia semántica | Introduce soporte por capacidad, evidencia/confianza explícita y una separación más fuerte entre estructura cruda y semántica específica de fabricante. Leer estructura deja de equivaler a soporte semántico. |
| **v0.3.0** | Rama de análisis de eventos | Añade candidatos calculados desde la curva como una nueva rama posterior a la reconstrucción, manteniéndolos separados de eventos almacenados en SOR. |
| **v0.3.1** | Separación detección / evidencia / revisión | Añade una capa de evidencia independiente y estado explícito de revisión humana, evitando mezclar detección automática con validación. |
| **v0.3.2** | Análisis híbrido contextual | Amplía el análisis con persistencia, polaridad, contexto de recuperación y lógica de región terminal, conservando candidatos suprimidos y sus motivos. |
| **v0.3.3** | Arquitectura multifuente | Añade un lector EI defensivo y una segunda ruta de entrada. El emparejamiento EI/SOR pasa a basarse en contenido y metadatos, y los registros EI adquieren procedencia propia. |
| **v0.3.4** | Arquitectura de referencia actual | Añade diagnóstico terminal D1 y localización multiescala alrededor del modelo consciente de procedencia sin reemplazar el escáner estructural estable. |

Esta evolución importa porque muestra con la misma claridad qué permaneció estable y qué cambió. `scanner.py` y el perfil EXFO validado son idénticos byte a byte en las entregas archivadas desde v0.1.0 hasta v0.3.4; las versiones posteriores expanden interpretación y análisis alrededor de esa frontera en lugar de reescribirla repetidamente.

## Arquitectura de referencia actual · v0.3.4

El siguiente diagrama representa la arquitectura actual de **v0.3.4** reconstruida a partir de la implementación archivada. Esta es la arquitectura de referencia para la migración pública del código.

<p align="center">
  <img src="../assets/architecture/system-es-light.svg#gh-light-mode-only" alt="Arquitectura OTDR Inside v0.3.4 en modo claro" width="100%">
  <img src="../assets/architecture/system-es-dark.svg#gh-dark-mode-only" alt="Arquitectura OTDR Inside v0.3.4 en modo oscuro" width="100%">
</p>

## Vista general del sistema

El analizador actual tiene dos rutas de entrada relacionadas:

- **Ruta SOR** — el archivo SOR se inspecciona de forma segura, se interpreta mediante perfiles respaldados por evidencia, se reconstruye como traza y, cuando el perfil caracterizado lo permite, se analiza para proponer candidatos de evento calculados.
- **Ruta EI** — el archivo EI se lee defensivamente conforme al diseño observado del CE6422. Puede inspeccionarse de manera independiente o emparejarse con un SOR únicamente después de superar comprobaciones de contenido y metadatos de adquisición.

Ambas rutas convergen en un **modelo de vista consciente de la procedencia**. El visor no aplana toda la información en una única tabla indiferenciada: la información almacenada en SOR, la importada desde un EI verificado, los candidatos calculados y la revisión manual permanecen distinguibles.

La interfaz se sirve localmente y las mediciones originales no se reescriben.

## Rutas de procesamiento

### Ruta SOR

1. **Lectura estructural** recorre el mapa SOR, límites de bloques, revisiones y metadatos crudos, conservando los nombres de bloque y valores originales.
2. **Perfil y normalización** aplican semántica específica de fabricante únicamente cuando la evidencia disponible coincide con un perfil caracterizado. Los campos no soportados permanecen sin resolver en vez de ser adivinados.
3. **Reconstrucción de traza** deriva el eje de distancia y el nivel relativo a partir de las muestras serializadas. Las muestras originales y cualquier relleno reconocido permanecen disponibles en el modelo de análisis.
4. **Análisis de eventos** permanece separado de los eventos almacenados. La línea Ceyear utiliza un detector base determinista, refinamiento contextual, auditoría independiente de evidencia y diagnósticos experimentales de localización.

### Ruta EI

1. **Lectura EI defensiva** valida el diseño observado y conserva las regiones crudas antes de interpretar la cola de eventos.
2. Un EI abierto de manera independiente puede mostrar su curva observada y registros importados, pero la sesión declara explícitamente que no se ha verificado un emparejamiento SOR.
3. **El emparejamiento EI/SOR se basa en contenido**, no en el nombre. La familia Ceyear caracterizada exige la relación esperada entre muestras junto con concordancia de metadatos de adquisición antes de aceptar el EI como fuente compañera.
4. Los registros derivados del EI conservan su propio origen y no sobrescriben información almacenada en SOR ni candidatos calculados desde la curva.

### Ruta de presentación

`viewer_model.py` orquesta lectura estructural, normalización, traza, análisis de eventos y el EI compañero opcional dentro de un único modelo para el visor local. La capa web representa ese modelo y produce las exportaciones de análisis/eventos sin modificar la medición fuente.

## Mapa de implementación

Los nombres siguientes corresponden a la implementación archivada v0.3.4. La migración pública sanitizada puede reorganizar rutas del paquete o identificadores visibles, pero estas fronteras de responsabilidad deberían conservarse.

| Capa | Módulo(s) archivados | Responsabilidad | Frontera deliberada |
|---|---|---|---|
| **Aplicación local** | `visor_otdr.py`, `web/*` | Interfaz HTTP local, recepción de archivos, UI estática y descargas. | Opera localmente; las mediciones fuente se procesan mediante copias temporales en lugar de reescribirse. |
| **Resolución de recursos** | `resources.py` | Resuelve proyecto, web y perfiles independientemente del directorio desde el que se inicia. | No interpreta semántica de medición. |
| **Lectura estructural** | `scanner.py` | Recorre la estructura SOR, comprueba límites y expone metadatos crudos de los bloques. | No convierte legibilidad estructural en certeza semántica de fabricante. |
| **Interpretación por perfiles** | `normalizer.py`, `profiles/*.json`, `export_signature.py` | Selecciona perfiles caracterizados, normaliza campos soportados y registra evidencia/confianza. | Conserva valores crudos; la semántica no soportada permanece explícita. |
| **Reconstrucción de traza** | `trace.py` | Reconstruye distancia y nivel relativo; distingue muestras útiles del relleno serializado reconocido. | El nivel relativo no se presenta como potencia óptica calibrada universalmente. |
| **Generación base de eventos** | `events_baseline.py` | Produce candidatos deterministas desde la curva mediante estadística local robusta. | No infiere pérdida certificada, ORL, reflectancia ni final físico de fibra. |
| **Contexto y evidencia** | `events_hybrid.py`, `event_evidence.py` | Añade persistencia, polaridad, recuperación, diagnóstico terminal y ventanas independientes de evidencia. | Los candidatos suprimidos permanecen explicables en vez de desaparecer silenciosamente. |
| **Diagnóstico de localización** | `localization.py` | Evalúa la posición local de escalón/rampa en varias escalas. | La dispersión multiescala es sensibilidad del algoritmo, no un intervalo estadístico de confianza. |
| **Interpretación EI** | `ei_inspection.py`, `ei_reader.py` | Lee defensivamente el diseño EI observado, conserva regiones crudas e importa registros soportados. | No existe escritor EI; las variantes desconocidas se rechazan en vez de forzarse. |
| **Modelo de aplicación** | `viewer_model.py` | Combina estructura, normalización, traza, fuentes de eventos, emparejamiento EI opcional y estado de presentación. | La procedencia de cada evento permanece explícita en todo el modelo. |

## La procedencia forma parte de la arquitectura

OTDR Inside trata la procedencia como datos y no como una nota añadida al final. El modelo público utiliza cuatro orígenes conceptuales:

| Origen | Significado |
|---|---|
| **Almacenado en SOR** | Evento o valor serializado en la estructura SOR original, por ejemplo una tabla `KeyEvents` cuando existe. |
| **Importado de EI** | Registro leído de un EI cuyo diseño observado está soportado; cuando aparece como compañero verificado, las comprobaciones de pareja SOR/EI ya fueron superadas. |
| **Candidato calculado** | Hipótesis de evento generada desde la curva reconstruida por la cadena de análisis. |
| **Revisión manual** | Anotación o estado de revisión creado por el usuario durante la sesión. No modifica la medición. |

Dos valores pueden aparecer juntos en la misma interfaz sin tener el mismo estatus de evidencia. Esa diferencia es intencional y se conserva en las exportaciones.

## Fronteras arquitectónicas por fabricante

| Ecosistema | Papel actual | Consecuencia arquitectónica |
|---|---|---|
| **EXFO FTB-7200D** | Línea base de referencia validada | Los eventos almacenados y la semántica SOR 2.00 caracterizada pueden fluir al modelo normalizado con alta confianza para el perfil validado. |
| **Ceyear CE6422** | Línea activa de desarrollo caracterizada | La semántica de traza, candidatos calculados y relación EI/SOR observada se manejan mediante comprobaciones explícitas de perfil/firma y procedencia. |
| **Yokogawa AQ1000** | Alcance de investigación estructural | La legibilidad estructural no se promueve a perfil semántico mientras la interpretación específica del fabricante no cuente con evidencia suficiente. |

Por tanto, el soporte es progresivo y no binario:

<p align="center"><strong>estructuralmente legible → perfil identificado → caracterizado semánticamente → validado empíricamente</strong></p>

## Invariantes arquitectónicas

- **Fuente de solo lectura.** El análisis no debe reescribir la medición original.
- **Estructura antes que semántica.** El escáner establece qué existe físicamente antes de que un perfil le asigne significado.
- **Crudo antes que normalizado.** La normalización añade una capa interpretada sin eliminar la representación almacenada.
- **Almacenado antes que calculado.** Los candidatos calculados nunca se presentan como eventos serializados por el instrumento o por software de fabricante.
- **Desconocido antes que adivinado.** Las variantes no soportadas degradan de forma explícita en lugar de recibir valores especulativos.
- **Emparejamiento por evidencia.** Una pareja EI/SOR se acepta por contenido observado y metadatos de adquisición, no por semejanza del nombre.
- **Análisis determinista.** Con la misma entrada soportada y los mismos parámetros, la cadena de eventos está diseñada para reproducir los mismos candidatos y diagnósticos.
- **Frontera de presentación local.** La aplicación sirve su interfaz en el equipo local y mantiene las mediciones operativas fuera del repositorio público.

## Qué no afirma esta arquitectura

La arquitectura no debe interpretarse como una afirmación de compatibilidad universal con SOR ni como certificación independiente de Telcordia. En particular, los candidatos calculados para Ceyear no crean pérdidas de evento, reflectancia, ORL o semántica de final de fibra certificadas. Del mismo modo, la dispersión experimental de localización no es un intervalo de incertidumbre calibrado y un EI abierto sin un SOR compañero verificado no se trata silenciosamente como una pareja confirmada.

Estos límites no son funciones faltantes ocultadas por la interfaz; son fronteras explícitas de la evidencia disponible actualmente para el proyecto.

## Por qué importa esta estructura

La estabilidad del núcleo estructural permitió añadir posteriormente semántica Ceyear, evidencia de eventos, detección híbrida, soporte EI y diagnóstico terminal sin reescribir el escáner SOR. La arquitectura refleja por ello la principal decisión de ingeniería del proyecto: **extender la interpretación alrededor de una frontera estable de datos crudos en lugar de acoplar cada regla específica de fabricante directamente al parser**.

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
</p>

<p align="center"><sub>Siguiente página independiente de documentación: Procedencia de eventos.</sub></p>
