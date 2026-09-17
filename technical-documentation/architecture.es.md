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

> **Alcance de esta página.** Este documento reconstruye la arquitectura representada por las entregas archivadas actualmente analizadas para el repositorio público, desde **v0.1.0 hasta v0.3.5**. Ese rango es un límite documental, no el final del proyecto. Las versiones posteriores deben ampliar esta historia sin convertir retroactivamente la última instantánea documentada en una arquitectura definitiva.

OTDR Inside se entiende mejor como una secuencia de decisiones de ingeniería que como un único diagrama terminado. El proyecto parte de una ruta SOR estructural estable y después incorpora semántica de fabricante, evidencia explícita, análisis de eventos desde la curva, revisión humana, EI como segunda fuente, diagnóstico terminal y finalmente una etapa independiente de evidencia terminal.

La página se organiza en tres niveles:

| Nivel | Qué responde |
|---|---|
| **Mapa de evolución** | Qué capacidades arquitectónicas aparecieron y en qué orden. |
| **Notas versión por versión** | Qué cambió en cada entrega, qué se preservó y por qué fue relevante. |
| **Instantánea detallada de implementación** | Cómo se relacionan los módulos presentes en v0.3.5. Es una instantánea detallada, no una afirmación de que v0.3.5 sea la versión final. |

## Mapa de evolución

<p align="center">
  <img src="../assets/architecture/evolution-es-light.svg#gh-light-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo claro" width="100%">
  <img src="../assets/architecture/evolution-es-dark.svg#gh-dark-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo oscuro" width="100%">
</p>

Las entregas archivadas no representan reescrituras sucesivas. Varias versiones conservan deliberadamente el núcleo de análisis mientras robustecen ejecución, interpretación o diagnóstico alrededor de él. La frontera estructural definida al comienzo del proyecto sigue actuando como ancla para las capas posteriores.

## Desarrollo arquitectónico versión por versión

### v0.1.0 — Base estructural funcional

**Qué introduce.** Apertura local y de solo lectura de `.SOR`, lectura estructural mediante el mapa SOR y bloques nombrados, interpretación por perfiles, normalización, reconstrucción de traza, visor interactivo, eventos EXFO almacenados y exportación JSON/CSV. EXFO funciona como perfil de referencia validado y Ceyear todavía permanece preliminar.

**Frontera arquitectónica.** Lectura estructural e interpretación semántica se separan desde el comienzo. El escáner establece qué existe físicamente; los perfiles y la normalización deciden qué puede interpretarse con evidencia.

**Por qué importa.** Aquí nace la columna vertebral de larga duración: `SOR → scanner → perfil/normalización → traza → visor`.

### v0.1.1 — Robustez de ejecución sin rediseñar el motor

**Qué introduce.** Diagnóstico de arranque, comprobación explícita de Python, fallos visibles y puertos locales alternativos cuando el predeterminado no está disponible.

**Qué preserva.** Escáner, perfiles, normalización y reconstrucción de traza permanecen esencialmente iguales.

**Por qué importa.** Las preocupaciones de ejecución quedan alrededor del analizador y no contaminan la lógica de medición.

### v0.1.2 — Flujo de lanzamiento compatible con seguridad

**Qué introduce.** Se retiran los lanzadores de shell y el inicio pasa a una tarea integrada de VS Code compatible con Smart App Control sin pedir al usuario debilitar la seguridad del sistema.

**Qué preserva.** Motor SOR, perfiles y comportamiento de solo lectura permanecen sin cambios.

**Por qué importa.** El despliegue evoluciona de forma independiente al parser y a la cadena semántica.

### v0.2.0 — La evidencia pasa a formar parte de la interpretación

**Qué introduce.** El comportamiento de curva Ceyear se caracteriza frente a software de referencia con material controlado. El soporte pasa a expresarse por capacidad; evidencia y confianza acompañan a los campos interpretados; alcance nominal y extensión muestreada se separan; el nivel vertical recuperado permanece explícitamente relativo.

**Qué preserva.** La legibilidad estructural sigue dependiendo de la misma frontera de datos crudos y la semántica de eventos no soportada no se inventa solo porque la curva pueda reconstruirse.

**Por qué importa.** La arquitectura formaliza la diferencia entre **leer bytes** y **afirmar significado**.

### v0.3.0 — El análisis de eventos calculados aparece como rama independiente

**Qué introduce.** Un detector sobre la curva propone picos y transiciones persistentes para la familia Ceyear caracterizada. Se distingue la región serializada de muestras de la región útil de análisis y los candidatos calculados permanecen separados de la información de eventos almacenada en el SOR.

**Qué preserva.** El análisis de eventos ocurre después de reconstruir la traza; parser y normalizador no fingen que los candidatos calculados fueron serializados por el equipo o software de fabricante.

**Por qué importa.** La inferencia de eventos se convierte en una capa explícita con procedencia propia.

### v0.3.1 — Detección, evidencia y revisión humana se separan

**Qué introduce.** Ventanas independientes de evidencia, medidas de ruido/contexto y motivos explícitos, sin reemplazar el generador base solo porque otros umbrales produzcan salidas diferentes. La revisión humana recibe estados `pending / accepted / rejected`, comentarios e historial.

**Qué preserva.** Generación automática y validación humana siguen siendo información distinta.

**Por qué importa.** El modelo responde por separado: *¿qué propuso el detector?, ¿qué evidencia rodea la propuesta?, ¿qué decidió una persona?*

### v0.3.2 — El análisis de eventos se vuelve contextual y explicable

**Qué introduce.** El detector híbrido añade polaridad, persistencia, contexto de recuperación y lógica terminal/ruido. Puede considerar un número limitado de propuestas adicionales y conserva regla y vector de características de candidatos suprimidos.

**Qué preserva.** La ruta base sigue siendo el fundamento determinista, mientras la capa contextual refina resultados sin borrarlos silenciosamente.

**Por qué importa.** La explicabilidad entra al modelo incluso para decisiones negativas.

### v0.3.3 — La arquitectura se vuelve multifuente

**Qué introduce.** Un lector defensivo `.EI` crea una segunda ruta de entrada. El emparejamiento EI/SOR se acepta por contenido y metadatos, no por semejanza de nombres. La información EI recibe su propia procedencia junto con SOR almacenado, candidatos calculados y revisión manual.

**Qué preserva.** Un EI puede inspeccionarse sin afirmar silenciosamente que existe una pareja SOR verificada y los registros EI no sobrescriben otros orígenes.

**Por qué importa.** OTDR Inside pasa de una cadena centrada en un solo archivo a un modelo multifuente consciente de procedencia.

### v0.3.4 — Se agregan diagnóstico terminal y localización

**Qué introduce.** Región terminal D1 navegable, localización experimental multiescala, exportaciones alineadas y regresión dedicada al comportamiento terminal, determinismo y reducción de visualización.

**Qué preserva.** La dispersión multiescala se trata como sensibilidad del algoritmo, no como incertidumbre estadística, y el estimador experimental no reemplaza automáticamente la posición principal.

**Por qué importa.** El comportamiento terminal se vuelve una capa diagnóstica visible sin promoverse todavía, por defecto, a un final físico candidato.

### v0.3.5 — La evidencia terminal se convierte en una etapa de decisión independiente

**Qué introduce.** El nuevo módulo `terminal.py` evalúa si una transición gruesa de ruido ya detectada tiene suficiente apoyo para convertirse en **posible final no reflectivo**. La evaluación combina acuerdo del punto de cambio en tres ventanas, persistencia de ruido posterior y descenso relativo adicional respecto a la tendencia anterior. Si ya existe un final reflectivo seleccionado por las reglas previas, la vía no reflectiva no lo sustituye.

**Qué preserva.** Si la evidencia no es completa, la región permanece como diagnóstico D. Si se promueve, el final sigue siendo calculado y revisable; no obtiene pérdida, reflectancia, ORL ni semántica de extremo físico certificado inventadas.

**Guardia adicional.** Cuando un final no reflectivo sí tiene apoyo, los cambios débiles cercanos se comparan con el ruido preterminal local en vez de excluirse de forma indiscriminada. Al mismo tiempo se conserva el dominio de búsqueda suplementaria establecido para que nueva estructura terminal no desplace propuestas previas solo por un límite de cantidad.

**Por qué importa.** La arquitectura distingue ahora **detección terminal**, **evidencia terminal** y **representación del evento terminal**. La promoción desde región diagnóstica a candidato queda auditada en lugar de depender de un efecto oculto de umbral.

## Hilo arquitectónico entre versiones

| Aspecto | Estado inicial | Cómo se amplía |
|---|---|---|
| **Estructura** | Lectura SOR segura en v0.1.0 | Permanece como frontera estable de datos crudos hasta v0.3.5. |
| **Semántica** | Interpretación por perfiles | Incorpora evidencia por capacidad, confianza y estados no soportados explícitos en v0.2.0. |
| **Eventos** | Eventos EXFO almacenados | Incorpora candidatos calculados, evidencia, refinamiento contextual, revisión y razonamiento terminal. |
| **Fuentes** | Solo SOR | Añade lectura EI defensiva y emparejamiento EI/SOR basado en evidencia en v0.3.3. |
| **Diagnóstico** | Visualización de traza y eventos | Añade razonamiento terminal en v0.3.4 y lógica de promoción respaldada por evidencia en v0.3.5. |

Dos componentes de base son idénticos byte a byte desde v0.1.0 hasta v0.3.5: `scanner.py` y el perfil EXFO validado. `normalizer.py` y `trace.py` también permanecen sin cambios entre v0.3.4 y v0.3.5. La nueva entrega amplía el razonamiento terminal sin mover la frontera de datos crudos.

## Instantánea detallada de implementación · v0.3.5

El resto de esta página utiliza **v0.3.5 como la entrega archivada más reciente actualmente analizada para el repositorio**. No es una versión final ni un punto de cierre. Cuando se integren versiones posteriores, esta instantánea puede avanzar sin reescribir el historial arquitectónico previo.

<p align="center">
  <img src="../assets/architecture/system-es-light.svg#gh-light-mode-only" alt="Instantánea de implementación OTDR Inside v0.3.5 en modo claro" width="100%">
  <img src="../assets/architecture/system-es-dark.svg#gh-dark-mode-only" alt="Instantánea de implementación OTDR Inside v0.3.5 en modo oscuro" width="100%">
</p>

## Vista general del sistema en esta instantánea

La implementación v0.3.5 tiene dos rutas de entrada relacionadas y una rama explícita de evidencia terminal:

- **Ruta SOR** — lectura estructural segura → interpretación por perfiles respaldados por evidencia → reconstrucción de traza → análisis de eventos calculados.
- **Ruta EI** — lectura EI defensiva → verificación de contenido/metadatos cuando se suministra un SOR compañero.
- **Rama de evidencia terminal** — parte de una transición terminal/ruido gruesa, evalúa apoyo independiente y produce un candidato no reflectivo calculado o conserva la región como diagnóstico D.

Las tres convergen en un **modelo consciente de procedencia**. SOR almacenado, EI verificado, candidatos calculados y revisión manual permanecen distinguibles.

La interfaz se sirve localmente y las mediciones originales no se reescriben.

## Rutas de procesamiento

### Ruta SOR

1. **Lectura estructural** recorre mapa SOR, límites, revisiones y metadatos crudos conservando valores originales.
2. **Perfil y normalización** aplican semántica específica de fabricante solo cuando la evidencia coincide con un perfil caracterizado.
3. **Reconstrucción de traza** deriva distancia y nivel relativo manteniendo información cruda y límites de muestras.
4. **Análisis de eventos** genera y refina contextualmente candidatos calculados por separado de eventos almacenados.
5. **Evaluación terminal** examina una transición terminal con evidencia independiente de cambio, descenso y ruido posterior antes de promover un final no reflectivo.

### Ruta EI

1. **Lectura EI defensiva** valida el diseño observado y conserva regiones crudas antes de interpretar registros soportados.
2. Un EI abierto de forma independiente no implica silenciosamente una pareja SOR verificada.
3. **El emparejamiento EI/SOR se basa en contenido**, usando la relación de muestras caracterizada y metadatos de adquisición.
4. Los registros EI conservan su origen y no sobrescriben información almacenada en SOR o calculada desde la curva.

### Ruta de presentación

`viewer_model.py` combina lectura, normalización, traza, análisis de eventos, evidencia terminal y pareja EI opcional dentro del modelo local del visor. La capa web lo representa y produce JSON/CSV sin modificar la medición fuente.

## Mapa de implementación

| Capa | Módulo(s) archivados | Responsabilidad | Frontera deliberada |
|---|---|---|---|
| **Aplicación local** | `visor_otdr.py`, `web/*` | Interfaz HTTP local, recepción de archivos, UI y descargas. | La medición fuente no se reescribe. |
| **Resolución de recursos** | `resources.py` | Resuelve recursos del proyecto, web y perfiles independientemente del directorio de lanzamiento. | No interpreta semántica de medición. |
| **Lectura estructural** | `scanner.py` | Recorre estructura SOR, valida límites y expone metadatos crudos. | Legibilidad estructural no equivale a certeza semántica de fabricante. |
| **Interpretación por perfiles** | `normalizer.py`, `profiles/*.json`, `export_signature.py` | Selecciona perfiles caracterizados y registra valores normalizados con evidencia/confianza. | Los valores crudos permanecen disponibles y la semántica no soportada sigue explícita. |
| **Reconstrucción de traza** | `trace.py` | Reconstruye distancia y nivel relativo; distingue muestras útiles del relleno serializado reconocido. | El nivel relativo no se presenta como potencia óptica calibrada universalmente. |
| **Generación base de eventos** | `events_baseline.py` | Produce candidatos deterministas mediante estadística local robusta. | No infiere pérdida, ORL, reflectancia ni final físico certificados. |
| **Análisis contextual** | `events_hybrid.py` | Añade polaridad, persistencia, recuperación, guardias de vecindad terminal y supresión explicable. | Los candidatos suprimidos permanecen trazables. |
| **Evidencia general de eventos** | `event_evidence.py` | Adjunta contexto orientado a evidencia/revisión, incluida evidencia de candidatos terminales. | La evidencia no reescribe procedencia ni datos fuente. |
| **Diagnóstico de localización** | `localization.py` | Evalúa posición local de escalón/rampa en varias escalas. | La dispersión multiescala es sensibilidad del algoritmo, no incertidumbre estadística. |
| **Evidencia terminal** | `terminal.py` | Evalúa cambio de ruido en tres ventanas, ruido posterior persistente y descenso relativo antes de promover un final no reflectivo. | Un candidato respaldado sigue sin ser final físico certificado ni intervalo de incertidumbre calibrado. |
| **Interpretación EI** | `ei_inspection.py`, `ei_reader.py` | Lee defensivamente el diseño EI observado e importa registros soportados. | Las variantes EI desconocidas se rechazan en vez de forzarse. |
| **Modelo de aplicación** | `viewer_model.py` | Combina todas las fuentes, candidatos, evidencia, pareja EI opcional y estado de presentación. | La procedencia permanece explícita en todo el payload. |

## La procedencia forma parte de la arquitectura

| Origen | Significado |
|---|---|
| **SOR almacenado** | Evento o valor serializado en la estructura SOR original, por ejemplo `KeyEvents` cuando existe. |
| **EI importado** | Registro leído de un diseño EI soportado; si se muestra como compañero verificado, las comprobaciones de pareja ya se superaron. |
| **Candidato calculado** | Hipótesis generada desde la curva reconstruida, incluido un final terminal no reflectivo suficientemente respaldado. |
| **Revisión manual** | Anotación o estado de revisión creado durante la sesión; no modifica la medición. |

Dos valores pueden aparecer juntos sin tener el mismo estatus de evidencia. Esa diferencia es intencional y se conserva en las exportaciones.

## Fronteras por fabricante en la instantánea documentada

| Ecosistema | Papel | Consecuencia arquitectónica |
|---|---|---|
| **EXFO FTB-7200D** | Línea base de referencia validada | Eventos almacenados y semántica SOR 2.00 caracterizada fluyen al modelo normalizado para el perfil validado. |
| **Ceyear CE6422** | Línea activa de desarrollo caracterizada | Semántica de traza, candidatos calculados, evidencia terminal y relación EI/SOR usan comprobaciones explícitas de perfil/firma/procedencia. |
| **Yokogawa AQ1000** | Alcance de investigación estructural | La legibilidad estructural no se promueve a perfil semántico sin evidencia suficiente. |

<p align="center"><strong>estructuralmente legible → perfil identificado → caracterizado semánticamente → validado empíricamente</strong></p>

## Invariantes arquitectónicas

- **Fuente de solo lectura.** El análisis no debe reescribir la medición original.
- **Estructura antes que semántica.** El escáner establece qué existe antes de que un perfil asigne significado.
- **Crudo antes que normalizado.** La interpretación añade una capa sin descartar la representación almacenada.
- **Almacenado antes que calculado.** Los candidatos calculados nunca se presentan como eventos serializados.
- **Desconocido antes que adivinado.** Las variantes no soportadas degradan explícitamente.
- **Emparejamiento por evidencia.** EI/SOR se acepta por contenido y metadatos, no por semejanza de nombres.
- **Diagnóstico antes que evento promovido.** Una transición terminal permanece diagnóstica hasta contar con evidencia independiente.
- **Candidato antes que certificación.** Incluso un final no reflectivo respaldado sigue siendo candidato calculado.
- **Análisis determinista.** La misma entrada soportada y parámetros deberían reproducir candidatos y diagnósticos.

## Qué no afirma la arquitectura documentada

La arquitectura documentada no implica compatibilidad universal SOR/EI ni certificación independiente Telcordia. Los candidatos calculados Ceyear no crean pérdidas, reflectancia u ORL certificados. La vía de final no reflectivo de v0.3.5 tampoco convierte el acuerdo algorítmico en tolerancia metrológica ni en prueba del extremo físico de la fibra. Los umbrales observados en interfaces de fabricante durante el desarrollo no se copian al detector mientras su semántica no esté demostrada.

Esos límites forman parte de la arquitectura: versiones futuras pueden ampliar evidencia, pero las afirmaciones históricas permanecen ligadas a lo que cada entrega realmente demostró.

## Dirección de diseño a lo largo de la secuencia

La decisión recurrente es **extender interpretación alrededor de una frontera estable de datos crudos en lugar de acoplar cada regla específica de fabricante directamente al parser**. v0.3.5 mantiene esa dirección: la nueva lógica terminal aparece como una etapa independiente de evidencia, no como una reescritura del escáner estructural o de la reconstrucción de traza.

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
</p>

<p align="center"><sub>Siguiente página independiente de documentación: Procedencia de eventos.</sub></p>