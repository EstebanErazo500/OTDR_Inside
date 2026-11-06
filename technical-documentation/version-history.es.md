<h1 align="center">Historial de versiones · registro de ingeniería reconstruido</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones · página actual">
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
  <a href="data-handling.es.md"><img src="../assets/nav/manejo-datos.svg" alt="Manejo de datos"></a>
</p>

<p align="center">
  <a href="version-history.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="version-history.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

> Este historial público se reconstruye a partir de entregas archivadas del desarrollo. Documenta la secuencia técnica real, pero **no** pretende que esas entregas hayan sido originalmente commits de Git. La secuencia es documental: con el permiso vigente no se prevén etiquetas del código fuente ni entregas descargables.

## Evolución general

| Versión | Enfoque de ingeniería | Cambio principal |
|---|---|---|
| **v0.1.0** | Rebanada vertical funcional | Lectura SOR segura, interpretación por perfiles, reconstrucción de traza, visor local, eventos EXFO almacenados y exportación JSON/CSV. |
| **v0.1.1** | Robustez de ejecución | Diagnóstico de inicio, comprobación de Python y puertos alternativos sin cambiar el núcleo de análisis. |
| **v0.1.2** | Arranque compatible con seguridad | Se retiran lanzadores de shell y se adopta una tarea de VS Code compatible con Smart App Control. |
| **v0.2.0** | Semántica Ceyear respaldada por evidencia | Caracterización de curva, soporte por capacidad y separación explícita crudo/normalizado. |
| **v0.3.0** | Candidatos calculados | Eventos derivados de curva como fuente separada de los eventos almacenados. |
| **v0.3.1** | Evidencia y revisión humana | Ventanas de evidencia independientes y estado pendiente/aceptado/rechazado. |
| **v0.3.2** | Detector híbrido contextual | Persistencia, polaridad, recuperación y exclusiones explicables. |
| **v0.3.3** | Análisis multifuente EI/SOR | Lectura EI defensiva, verificación de pareja por contenido y procedencia EI separada. |
| **v0.3.4** | Diagnóstico terminal | Región terminal navegable y experimentos de localización multiescala. |
| **v0.3.5** | Evidencia terminal | Posible final no reflectivo solo cuando coinciden varias condiciones de evidencia. |
| **v0.3.6** | Consistencia EI/SOR | Consistencia numérica, corrección de propuestas duplicadas y localización a tres escalas para respuestas débiles. |
| **v0.4.0** | Espacio de trabajo multiarchivo | Hasta ocho pestañas, asociación EI explícita, comparación 2–4, diagnóstico reorganizado y competencia terminal más estricta. |
| **v0.4.1** | Corrección del comparador | Agrupamiento entre cualquier par, tramo común/completo y referencia visual robusta 5–50 m; detector idéntico a v0.4.0. |
| **v0.4.2** | Evidencia y localización experimental | Auditoría de excluidos, sensibilidad terminal, borde por clase opcional e informes mejorados. |
| **v0.4.3** | Yokogawa y asesoría morfológica | Interpretación de eventos SOR mediante perfil, procedencia confirmada separada de identificación declarada y revisión morfológica Random Forest. |
| **v0.4.4** | JDSU MSOR y asociaciones revisables | Lector JDSU de dos longitudes de onda, eventos nativos con procedencia, comparación revisable y localización adaptativa evaluada. |
| **v0.4.5** | SOZ, comparación y revisión contextual | SOZ multitraza, comparación consciente de fuentes, eje IOR seleccionable, separaciones entre eventos, pérdida local LSA y revisión RF/LSA. |

## Fase 1 · base estructural estable

### v0.1.0

La primera entrega archivada ya era una rebanada vertical completa: apertura SOR de solo lectura, lectura estructural, selección de perfiles, EXFO como referencia validada, reconstrucción de traza, visor interactivo, eventos almacenados y exportación.

La decisión arquitectónica establecida aquí sobrevive en las versiones posteriores:

`SOR → escáner estructural → perfil/normalización → traza → visor`

La entrega registra 23 pruebas automáticas aprobadas.

### v0.1.1

El núcleo de análisis se mantiene. El trabajo se concentra en diagnóstico de arranque, comprobación de Python, fallos visibles y selección de puertos locales alternativos.

### v0.1.2

El cambio es de despliegue. Se retiran lanzadores de shell y el inicio pasa a una tarea de VS Code compatible con Smart App Control sin debilitar la seguridad del sistema.

## Fase 2 · la semántica de fabricante se vuelve explícita

### v0.2.0

Ceyear pasa de soporte estructural preliminar a un perfil de curva caracterizado empíricamente. Aparecen soporte por capacidad, evidencia/confianza en campos normalizados y una separación deliberada entre alcance nominal y extensión muestreada.

Aquí el proyecto formaliza la diferencia entre **leer bytes** y **afirmar significado**.

## Fase 3 · eventos calculados con evidencia y revisión

### v0.3.0

La línea 0.3 introduce un detector experimental sobre la curva reconstruida. Los candidatos calculados quedan separados de los eventos almacenados en el SOR y las magnitudes diagnósticas siguen siendo relativas.

### v0.3.1

En vez de reemplazar el detector base solo porque otros umbrales producen resultados distintos, se conserva el generador y se añade una capa independiente de evidencia y un estado explícito de revisión humana.

Quedan separadas tres preguntas:

- qué propuso el detector;
- qué evidencia rodea la propuesta;
- qué decidió una persona.

### v0.3.2

El detector se vuelve contextual: polaridad, persistencia, recuperación y lógica terminal/ruido. Los candidatos suprimidos conservan la regla y el contexto que explica la decisión.

La comparación de desarrollo archivada muestra mejor recuperación de las referencias disponibles, manteniendo explícito que no eran datos ciegos de evaluación.

## Fase 4 · formatos emparejados y razonamiento terminal

### v0.3.3

Un lector `.EI` defensivo crea una segunda ruta de entrada. La pareja EI/SOR deja de depender del nombre y exige la relación de muestras esperada y metadatos compatibles para la familia caracterizada.

Los registros EI adquieren procedencia propia.

### v0.3.4

Se incorpora una región terminal de diagnóstico navegable y experimentos de localización multiescala. La dispersión multiescala se documenta como sensibilidad del algoritmo, no como intervalo de confianza.

### v0.3.5

El razonamiento terminal añade una etapa separada de evidencia. Una transición puede convertirse en **posible final no reflectivo** solo si coinciden las condiciones de borde/caída/ruido implementadas. Si no, continúa como diagnóstico.

La procedencia no cambia: el resultado promovido sigue siendo calculado.

### v0.3.6

Esta versión estabiliza la línea 0.3 en lugar de introducir una nueva categoría de procedencia. Se concentra en:

- discretización consistente entre curvas EI y SOR equivalentes;
- selección de propuestas duplicadas/suplementarias;
- alternativa de tres escalas para respuestas débiles inconsistentes;
- conservación del detector de final no reflectivo de v0.3.5.

La validación archivada registra **80 pruebas de liberación**, 18 parejas EI/SOR controladas, 72/72 asociaciones de eventos posteriores al inicio e índices/clases coincidentes entre formatos. La diferencia media de posición SOR frente a EI cambia de forma marginal; por eso el beneficio documentado es **consistencia entre formatos y mejora puntual de localización**, no una afirmación global de exactitud.

En 27 SOR privados de campo, 22 conservaron índices/clases y 5 cambiaron. Esos cambios siguen requiriendo revisión porque no existe referencia EI independiente para cada traza de campo.

## Fase 5 · espacio de trabajo y comparación multiarchivo

### v0.4.0

v0.4.0 cambia la forma de utilizar el analizador sin descartar el motor por archivo.

Incorpora:

- hasta **ocho pestañas independientes**;
- zoom, evento seleccionado, anotaciones y revisión por pestaña;
- asociación explícita **EI a este SOR**, con verificación por contenido/metadatos;
- comparación de **dos a cuatro trazas**;
- tabla de condiciones de adquisición, superposición de curvas, cercanía de eventos y CSV de comparación;
- diagnóstico reorganizado en datos leídos, hallazgos accionables, advertencias y detalle técnico;
- competencia terminal más estricta: una subida tardía solo se propone como final reflectivo si su respuesta local a dos escalas supera tres veces su ruido local.

La validación de la entrega ejecutó el detector sobre 27 SOR privados de campo y 18 parejas EI/SOR controladas. Las 18 parejas mantuvieron 72/72 asociaciones sin candidatos calculados extra sin pareja. Frente a v0.3.6, solo una traza privada cambió de clase terminal con la nueva regla.

El comparador 0.4.0 todavía tenía dos debilidades de presentación: buscaba cercanía respecto al primer archivo seleccionado y usaba la primera muestra dibujada como referencia vertical.

### v0.4.1

v0.4.1 corrige el comparador y, de manera deliberada, **no cambia el detector**.

La comparación pasa a:

- agrupar posiciones próximas entre **cualquier par** de archivos seleccionados;
- mantener como máximo un evento por archivo en cada fila y conservar eventos sin pareja;
- ofrecer vistas **Tramo común** y **Recorrido completo**;
- usar como referencia visual la mediana del nivel recuperado entre **5 y 50 m**;
- atenuar visualmente la región terminal de ruido solo para la gráfica comparativa.

El umbral de 15 m sigue siendo una ayuda visual, no una tolerancia física ni prueba de identidad. El agrupamiento es codicioso y no afirma asociación física óptima.

La validación archivada de v0.4.1 registra **54 pruebas Python aprobadas y 6 pruebas dependientes de corpus externo omitidas**, además de una prueba DOM/canvas simulada para la nueva lógica de comparación. En ese momento todavía quedaba pendiente una inspección visual final del comparador revisado en navegador Windows.

### v0.4.2 · revisiones documentadas de una misma entrega

**29 de septiembre — auditoría del comparador.** Los eventos visibles incorporan grado de evidencia y se pueden consultar candidatos excluidos cercanos con sus reglas. Los excluidos no cuentan como parejas de eventos. Un diagnóstico con ±10 % del umbral mínimo de ruido advierte de posibles finales no reflectivos inestables, sin cambiar clases ni posiciones del detector base.

**1–8 de octubre — localización experimental por borde y clase.** Se introduce un método opcional que estudia transiciones locales según la clase del evento; mantiene las posiciones base y permite compararlas y restaurarlas. Está **desactivado por defecto**. Sobre 72 eventos de 18 parejas EI/SOR controladas, la diferencia absoluta media retrospectiva mejora frente a la referencia EXFO del estudio (4,399 → 2,726 m), pero empeora frente al EI de la misma adquisición (3,782 → 5,532 m). Son resultados de concordancia, no errores físicos de distancia; no está demostrada la identidad del montaje EXFO/Ceyear.

**8 de octubre — refinamiento de presentación.** Las sesiones conservan método e historial; se reorganizan controles de sesión e informes y mejora el PDF vectorial. El informe archivado registra 92 pruebas Python aprobadas con los corpus disponibles y verificaciones de interfaz simulada. No se efectuó una inspección en navegador Windows real.

v0.4.2 no se presenta como versión final ni como mejora certificada de posición. Su aporte es explicabilidad, ensayo reversible y mejor presentación de evidencia.

## Fase 6 · nuevos formatos y revisión orientada a mediciones

### v0.4.3 · perfil Yokogawa e identidad respaldada por evidencia

La línea 4.3 amplía Yokogawa más allá de la lectura estructural: permite representar `KeyEvents` nativos con posiciones y magnitudes ópticas caracterizadas. También introduce una **asesoría de morfología Random Forest** sobre ventanas de señal cercanas a eventos; sugiere categorías de forma sin reescribir posiciones, pérdidas ni procedencia almacenadas. Sus puntuaciones **no son probabilidades calibradas ni certificación física**.

La **revisión 1 del 15 de octubre** distingue 24 SOR que declaran explícitamente Yokogawa AQ1000 y otros 24 cuya procedencia confirmó el usuario del proyecto, pero cuyo modelo **no aparece declarado**. La atribución del segundo grupo solo corresponde a esos originales verificados; no se deduce de software genérico ni de nombres parecidos. La regresión archivada comprende **48 SOR y 125 registros nativos** con metadatos originales conservados. Esta revisión 4.3 todavía no incluye el lector JDSU MSOR.

### v0.4.4 · JDSU MSOR y comparación verificable

v0.4.4 introduce lectura defensiva de la familia **JDSU T-BERD 6000A / 8126 MSOR VSRE 2.10** observada. Un contenedor puede presentar dos trazas por longitud de onda, cada una con sus muestras, eventos nativos y procedencia; los campos no demostrados permanecen no disponibles. Las referencias TXT externas se aplican únicamente a originales verificados, no a cualquier archivo recibido.

Amplía la comparación entre dispositivos con selección de referencia, **asociación uno a uno revisable**, eventos sin pareja, condiciones de adquisición, CSV/PDF y sesiones. Las propuestas de correspondencia dentro de **50 m** sirven para revisión humana: no son tolerancias instrumentales ni demuestran identidad física. Tampoco se agregan longitudes de onda distintas como si fueran una misma adquisición.

La localización de borde adaptativo de 4.4 aparece expresamente **en evaluación**. En el contraste retrospectivo controlado mejoró la concordancia frente a EXFO y JDSU, pero empeoró frente a EI emparejado y retrocedió en 14 casos frente a EXFO. Estas diferencias se mantienen, sin llamar al experimento una calibración física.

### v0.4.5 · espacio multiformato y pérdida local estimada

La entrega archivada 4.5, consolidada el **5 de noviembre de 2026**, conserva el enfoque de posición de 4.4 y amplía las funciones analíticas y de presentación:

- Admite **SOR, EI, MSOR y SOZ** para los diseños caracterizados. Un SOZ multitraza aparece en una pestaña de contenedor con selector de 1310/1550 nm; conserva muestras, eventos nativos y procedencia por subtraza.
- Reconoce copias SOR/SOZ por datos de adquisición y muestras/eventos estándar, **sin contarlas como adquisiciones independientes**.
- EXFO y Yokogawa son **referencias documentales del mismo nivel** en el montaje controlado; JDSU aporta corroboración adicional. Seleccionar una referencia define el eje de comparación, no declara infalible al instrumento.
- Permite comparar ejes IOR originales o reexpresar condicionalmente con el IOR de referencia. Si un archivo no declara un índice válido, no se aplica la conversión silenciosamente. Esto no descubre el índice físico verdadero ni cambia los originales.
- Añade **diferencias de separación entre eventos** además de diferencias de posición, indicando las dependencias entre intervalos.
- Incorpora **LSA local** para estimar pérdidas en transiciones no reflectivas: seis combinaciones de ventanas anteriores/posteriores. La pérdida nativa, la LSA calculada, ventanas, sensibilidad y discrepancias permanecen separadas en visor, CSV/JSON y PDF.
- Conserva los pesos del **Random Forest morfológico**. La revisión consultiva considera ventanas, forma y desacuerdos de signo; un puntaje alto no anula una ventana inestable ni modifica el dato nativo.
- Las sesiones e informes conservan método, referencia/eje, identidad del contenedor y subtraza, revisión y contexto. La consolidación mejora presentación y un **registro orientado por IEC**, sin afirmar certificación normativa.

**Evidencia archivada y límites.** Nueve SOZ proporcionaron 18 trazas por longitud de onda y 69 eventos nativos. En el corpus Ceyear controlado se obtuvieron 54 estimaciones locales de pérdida de empalme, **49 con apoyo y 5 para revisión**. La diferencia absoluta media retrospectiva con referencias EXFO agrupadas fue de **0,0087 dB a 1310 nm** y **0,0202 dB a 1550 nm**; con Yokogawa (zonas cubiertas de 5 y 8 km) fue **0,0025 dB** y **0,0064 dB**. Son comparaciones dependientes del mismo montaje, no incertidumbre calibrada ni demostración de generalización. Se preservan los datos nativos contradictorios.

La revisión final archivada del 5 de noviembre registra **123 pruebas Python aprobadas con los datos disponibles** y comprobaciones DOM simuladas. La comprobación separada del paquete recién extraído pasó 25 pruebas dirigidas, verificó manifiesto y permitió renderizar informes de ejemplo. **No se verificó una ejecución real en Windows/navegador** en ese entorno. El selector RF alternativo de posición con doble referencia fue ensayado, pero **no se activó**: mejoró medias de posición y empeoró separaciones y determinados peores casos.

**El trabajo del 6 de noviembre se documenta aparte como propuestas/experimentos aislados**, no funciones incorporadas a la entrega 4.5: parche del centrado vertical y aviso IOR, lectura directa EXFO `.stdr`, estudios de calidad/comparabilidad y futuras mejoras de asociación. El parche del comparador pasó regresiones aisladas, pero no se empaquetó en la entrega 4.5 original.

## Qué permaneció estable

Dos componentes son **idénticos byte a byte en todas las entregas archivadas desde v0.1.0 hasta v0.4.1**:

- `scanner.py` — escáner estructural SOR;
- `exfo_ftb7200_sor2.json` — perfil EXFO de referencia validada.

Esto demuestra que el proyecto creció añadiendo semántica, evidencia, EI, análisis de eventos/terminal y comportamiento de espacio de trabajo alrededor de un contrato estructural estable.

Además, `events_hybrid.py` es idéntico entre v0.4.0 y v0.4.1: la corrección de 4.1 pertenece a la capa de espacio de trabajo/presentación, no al detector.

## Política vigente de publicación pública

La secuencia anterior documenta **entregas de ingeniería**. No implica que existan tags públicos del código ni que puedan crearse GitHub Releases instalables. La autorización vigente permite capturas revisadas del visor, arquitectura conceptual, ilustraciones generales/sintéticas y resúmenes metodológicos; **no permite publicar el código fuente de la aplicación**, aunque esté anonimizado o renombrado.

Las mediciones originales (`.SOR`, `.EI`, `.otdr`), curvas operativas derivadas, informes de campo, nombres/hashes privados, rutas internas y programas de fabricante permanecen fuera de GitHub. Las demostraciones públicas requieren ejemplos sintéticos independientes o capturas revisadas. Las cifras de validación archivadas son evidencia documental, **no una suite pública ejecutable**.

Si cambia el permiso para distribuir código, será necesaria una nueva autorización expresa y revisión de publicación antes de incorporar fuentes, tags de código, binarios o licencias de software libre. Mientras tanto, la evolución se representa mediante documentación, no snapshots públicos del código. Véase la <a href="data-handling.es.md">política de manejo de datos</a>.

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
</p>
