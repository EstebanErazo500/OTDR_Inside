<h1 align="center">Evolución de arquitectura · entregas documentadas</h1>

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

> **Alcance de esta página.** Este documento reconstruye la arquitectura representada por las entregas archivadas que ya fueron analizadas para el repositorio público, desde **v0.1.0 hasta v0.3.4**. Ese rango es un límite documental, no el final del proyecto. Las versiones posteriores deben ampliar esta historia en lugar de presentar v0.3.4 como una arquitectura definitiva.

OTDR Inside se entiende mejor como una secuencia de decisiones de ingeniería que como un único diagrama terminado. El proyecto partió de una cadena SOR estructural estable y, sobre esa base, fue incorporando interpretación de fabricante respaldada por evidencia, análisis de eventos desde la curva, procedencia explícita, revisión humana y una segunda ruta de entrada EI.

La página se organiza en tres niveles:

| Nivel | Qué responde |
|---|---|
| **Mapa de evolución** | Qué capacidades arquitectónicas aparecieron y en qué orden. |
| **Notas versión por versión** | Qué cambió en cada entrega archivada, qué se preservó y por qué el cambio fue relevante. |
| **Instantánea detallada de implementación** | Cómo se relacionan los módulos presentes en v0.3.4. Es un ejemplo detallado, no una afirmación de que v0.3.4 sea la versión final. |

## Mapa de evolución

<p align="center">
  <img src="../assets/architecture/evolution-es-light.svg#gh-light-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo claro" width="100%">
  <img src="../assets/architecture/evolution-es-dark.svg#gh-dark-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo oscuro" width="100%">
</p>

Las entregas archivadas no representan reescrituras sucesivas. Varias versiones conservaron deliberadamente el núcleo de análisis mientras robustecían ejecución, interpretación o diagnóstico alrededor de él. La frontera estructural definida al comienzo del proyecto sigue actuando como ancla para las capas posteriores.

## Desarrollo arquitectónico versión por versión

### v0.1.0 — Base estructural funcional

**Qué introduce.** La primera entrega archivada ya forma una rebanada vertical completa: apertura local y de solo lectura de `.SOR`, lectura estructural mediante el mapa SOR y bloques nombrados, interpretación por perfiles, normalización, reconstrucción de traza, visor interactivo, eventos EXFO almacenados y exportación JSON/CSV. EXFO funciona como perfil de referencia validado, mientras Ceyear todavía se encuentra en una fase preliminar.

**Frontera arquitectónica.** Desde esta versión se separan lectura estructural e interpretación semántica. El escáner determina qué existe físicamente en el archivo; los perfiles y la normalización determinan qué puede interpretarse con evidencia.

**Por qué importa.** Aquí nace la columna vertebral que se mantiene durante el resto de la secuencia: `SOR → scanner → perfil/normalización → traza → visor`. Las versiones posteriores crecen alrededor de esta ruta en lugar de reemplazarla.

### v0.1.1 — Robustez de ejecución sin rediseñar el motor

**Qué introduce.** Diagnóstico de arranque, comprobación explícita de versión de Python, fallos visibles y prueba de puertos locales alternativos cuando el puerto predeterminado no está disponible.

**Qué preserva.** El escáner SOR, los perfiles, la normalización y la reconstrucción de traza permanecen esencialmente iguales. El foco cambia desde la lógica de análisis hacia la confiabilidad de inicio y fallo en un entorno Windows.

**Por qué importa.** Las preocupaciones de ejecución quedan alrededor del analizador y no dentro de su núcleo estructural o semántico. La envolvente de aplicación puede evolucionar sin alterar la lógica de medición.

### v0.1.2 — Flujo de lanzamiento compatible con la seguridad del sistema

**Qué introduce.** Se eliminan los lanzadores de shell y el flujo local pasa a una tarea integrada de VS Code, de forma que la aplicación pueda coexistir con Windows Smart App Control sin pedir al usuario debilitar controles de seguridad del sistema operativo.

**Qué preserva.** La entrega archivada mantiene sin cambios el motor SOR, los perfiles y el comportamiento de solo lectura.

**Por qué importa.** El despliegue se trata como una capa adaptadora alrededor del analizador. Una restricción de seguridad cambia la forma de ejecutar el programa, no la forma de leer o interpretar una medición OTDR.

### v0.2.0 — La evidencia pasa a formar parte de la interpretación semántica

**Qué introduce.** El comportamiento de curva del Ceyear CE6422 se caracteriza frente a software de referencia usando material controlado. El soporte pasa a expresarse por capacidad en lugar de una etiqueta binaria soportado/no soportado; se adjuntan evidencia y confianza a los campos interpretados; se separan alcance nominal y extensión muestreada; y el nivel vertical recuperado se mantiene explícitamente como relativo.

**Qué preserva.** La legibilidad estructural sigue dependiendo del mismo límite de escaneo. Los valores crudos permanecen disponibles aun cuando se añade una interpretación normalizada, y la semántica de eventos no soportada no se inventa solo porque la curva pueda reconstruirse.

**Por qué importa.** Esta versión formaliza la separación entre **leer bytes** y **afirmar significado**. La semántica específica de fabricante se convierte en una capa respaldada por evidencia y deja de ser una suposición embebida en el parser.

### v0.3.0 — El análisis de eventos calculados aparece como una rama independiente

**Qué introduce.** Un detector experimental sobre la curva propone picos y transiciones persistentes para la familia Ceyear caracterizada. La implementación distingue la región serializada de muestras de la región útil para análisis y mantiene los candidatos calculados separados de la información de eventos almacenada en el SOR.

El visor incorpora selección de candidatos y soporte para anotaciones manuales, mientras las magnitudes diagnósticas se mantienen relativas y no se presentan como pérdidas, reflectancia, ORL o final de fibra certificados.

**Por qué importa.** El análisis de eventos se agrega **después de la reconstrucción de traza**. El parser y el normalizador no necesitan fingir que los eventos calculados fueron serializados por el instrumento o por software de fabricante.

### v0.3.1 — Detección, evidencia y revisión humana se separan

**Qué introduce.** En lugar de sustituir el generador base únicamente porque otros umbrales producen resultados distintos, la versión conserva el detector base y añade una capa independiente de evidencia: múltiples ventanas de contexto, mediciones de ruido local, contexto vecino y motivos explícitos. La revisión humana obtiene estados propios `pending / accepted / rejected`, además de comentarios e historial.

**Qué preserva.** Generación automática y validación humana siguen siendo conceptos distintos. El estado de revisión no modifica la medición fuente ni convierte retroactivamente un candidato calculado en un evento almacenado.

**Por qué importa.** La arquitectura pasa a representar tres preguntas separadas: *¿qué propuso el detector?, ¿qué evidencia rodea la propuesta?, ¿qué decidió una persona al revisarla?* Cada una puede evolucionar de manera independiente.

### v0.3.2 — El análisis de eventos se vuelve contextual y explicable

**Qué introduce.** El detector híbrido incorpora polaridad, persistencia, contexto de recuperación y lógica de región terminal/ruido. Puede considerar un número limitado de propuestas adicionales de cambio persistente, y los candidatos suprimidos conservan la regla y el vector de características que explican por qué fueron descartados.

**Qué preserva.** El camino base continúa disponible como fundamento determinista, mientras la capa contextual refina sus resultados en vez de borrarlos silenciosamente.

**Por qué importa.** El análisis de eventos deja de parecer una sola pasada por umbrales y pasa a una decisión por capas. La explicabilidad forma parte del modelo, incluso para decisiones negativas.

### v0.3.3 — La arquitectura se vuelve multifuente

**Qué introduce.** Un lector defensivo de `.EI` crea una segunda ruta de entrada. El emparejamiento EI/SOR deja de aceptarse por semejanza de nombres: para la familia caracterizada se exige concordancia entre metadatos de adquisición y la relación complementaria esperada entre muestras antes de considerar el EI como compañero verificado.

La información EI obtiene su propia categoría de procedencia junto con la información almacenada en SOR, los candidatos calculados y la revisión manual. Un EI también puede inspeccionarse de manera independiente sin afirmar silenciosamente que existe una pareja SOR verificada.

**Por qué importa.** Es el cambio más grande en la frontera de entrada dentro de la secuencia archivada. OTDR Inside pasa de una cadena centrada en un solo archivo a un modelo consciente de procedencia capaz de combinar fuentes relacionadas sin igualar su estatus de evidencia.

### v0.3.4 — Se agregan diagnóstico terminal y localización

**Qué introduce.** Esta entrega añade una región terminal D1 navegable, diagnósticos experimentales de localización multiescala, salidas CSV/JSON alineadas y regresión específica sobre comportamiento terminal, determinismo y reducción de visualización.

El experimento de localización distingue expresamente la dispersión multiescala como **sensibilidad del algoritmo, no como intervalo estadístico de confianza**, y no reemplaza automáticamente una posición principal solo por utilizar un estimador más complejo.

**Por qué importa.** El diagnóstico crece alrededor del modelo de eventos con procedencia sin obligar a cambiar el escáner estructural ni la frontera de emparejamiento EI/SOR. v0.3.4 es simplemente el límite superior de las entregas archivadas analizadas para esta página; **no** se presenta como la arquitectura final del proyecto.

## Hilo arquitectónico entre versiones

A lo largo de la secuencia documentada se repite un patrón coherente:

| Aspecto | Estado inicial | Cómo se amplía |
|---|---|---|
| **Estructura** | Lectura SOR segura en v0.1.0 | Permanece como frontera estable de datos crudos durante toda la secuencia archivada. |
| **Semántica** | Interpretación por perfiles | Incorpora evidencia por capacidad, confianza y estados no soportados explícitos en v0.2.0. |
| **Eventos** | Eventos EXFO almacenados | Incorpora candidatos calculados, evidencia, refinamiento contextual y revisión en v0.3.x. |
| **Fuentes** | Solo SOR | Añade lectura EI defensiva y emparejamiento EI/SOR basado en evidencia en v0.3.3. |
| **Diagnóstico** | Visualización de traza y eventos almacenados | Añade razonamiento de región terminal y diagnóstico de localización hacia v0.3.4. |

Dos componentes base son idénticos byte a byte en las entregas archivadas desde v0.1.0 hasta v0.3.4: `scanner.py` y el perfil EXFO validado. Esa continuidad permite ver que las capacidades posteriores se añadieron alrededor de un contrato estructural estable.

## Instantánea detallada de implementación · v0.3.4

El resto de esta página utiliza **v0.3.4 como instantánea detallada de implementación** porque es la entrega más completa incluida hasta ahora en este conjunto documental. No se trata como versión final ni como punto de cierre de la historia arquitectónica. Cuando se incorporen entregas posteriores, la sección de evolución debe crecer y la instantánea detallada podrá actualizarse sin reescribir las decisiones anteriores.

<p align="center">
  <img src="../assets/architecture/system-es-light.svg#gh-light-mode-only" alt="Instantánea de implementación OTDR Inside v0.3.4 en modo claro" width="100%">
  <img src="../assets/architecture/system-es-dark.svg#gh-dark-mode-only" alt="Instantánea de implementación OTDR Inside v0.3.4 en modo oscuro" width="100%">
</p>

## Vista general del sistema en esta instantánea

La implementación v0.3.4 contiene dos rutas de entrada relacionadas:

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

Los nombres siguientes corresponden a la implementación archivada v0.3.4. La migración pública sanitizada puede reorganizar rutas del paquete o identificadores visibles, pero lo importante del diseño son estas fronteras de responsabilidad.

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

OTDR Inside trata la procedencia como datos y no como una nota añadida al final. El modelo documentado utiliza cuatro orígenes conceptuales:

| Origen | Significado |
|---|---|
| **Almacenado en SOR** | Evento o valor serializado en la estructura SOR original, por ejemplo una tabla `KeyEvents` cuando existe. |
| **Importado de EI** | Registro leído de un EI cuyo diseño observado está soportado; cuando aparece como compañero verificado, las comprobaciones de pareja SOR/EI ya fueron superadas. |
| **Candidato calculado** | Hipótesis de evento generada desde la curva reconstruida por la cadena de análisis. |
| **Revisión manual** | Anotación o estado de revisión creado por el usuario durante la sesión. No modifica la medición. |

Dos valores pueden aparecer juntos en la misma interfaz sin tener el mismo estatus de evidencia. Esa diferencia es intencional y se conserva en las exportaciones.

## Fronteras arquitectónicas por fabricante en la instantánea documentada

| Ecosistema | Papel en la instantánea documentada | Consecuencia arquitectónica |
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

## Qué no afirma la arquitectura documentada

La arquitectura documentada no debe interpretarse como una afirmación de compatibilidad universal con SOR ni como certificación independiente de Telcordia. En particular, los candidatos calculados para Ceyear no crean pérdidas de evento, reflectancia, ORL o semántica de final de fibra certificadas. Del mismo modo, la dispersión experimental de localización no es un intervalo de incertidumbre calibrado y un EI abierto sin un SOR compañero verificado no se trata silenciosamente como una pareja confirmada.

Estos límites son fronteras explícitas de la evidencia representada por las implementaciones archivadas. Versiones futuras pueden ampliar capacidades, pero las afirmaciones históricas deben seguir vinculadas a lo que cada entrega realmente demostró.

## Dirección de diseño a lo largo de la secuencia

La estabilidad del núcleo estructural permitió añadir posteriormente semántica Ceyear, evidencia de eventos, detección híbrida, soporte EI y diagnóstico terminal sin reescribir el escáner SOR. La decisión arquitectónica recurrente es **extender la interpretación alrededor de una frontera estable de datos crudos en lugar de acoplar cada regla específica de fabricante directamente al parser**.

Esa dirección debe seguir siendo trazable cuando se incorporen nuevas versiones a la documentación. Las entregas posteriores deben ampliar la línea de tiempo y no convertir retrospectivamente v0.3.4 en un punto final.

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
</p>

<p align="center"><sub>Siguiente página independiente de documentación: Procedencia de eventos.</sub></p>
