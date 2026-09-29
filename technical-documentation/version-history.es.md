<h1 align="center">Historial de versiones · registro de ingeniería reconstruido</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones · página actual">
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
</p>

<p align="center">
  <a href="version-history.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="version-history.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

> Este historial público se reconstruye a partir de entregas archivadas del desarrollo. Documenta la secuencia técnica real, pero **no** pretende que esas entregas hayan sido originalmente commits de Git. Las etiquetas públicas se añadirán únicamente a medida que avance la migración sanitizada del código.

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

## Qué permaneció estable

Dos componentes son **idénticos byte a byte en todas las entregas archivadas desde v0.1.0 hasta v0.4.1**:

- `scanner.py` — escáner estructural SOR;
- `exfo_ftb7200_sor2.json` — perfil EXFO de referencia validada.

Esto demuestra que el proyecto creció añadiendo semántica, evidencia, EI, análisis de eventos/terminal y comportamiento de espacio de trabajo alrededor de un contrato estructural estable.

Además, `events_hybrid.py` es idéntico entre v0.4.0 y v0.4.1: la corrección de 4.1 pertenece a la capa de espacio de trabajo/presentación, no al detector.

## Política de migración pública

1. las mediciones operativas `.SOR`, `.EI` y `.otdr` nunca se incorporan a Git;
2. identificadores privados de rutas/clientes, nombres de corpus, seriales, hashes y rutas internas se eliminan o generalizan;
3. no se redistribuyen programas propietarios, estándares licenciados ni manuales comerciales;
4. la regresión pública usa fixtures sintéticos o explícitamente anonimizados;
5. las etiquetas representan estados históricos sanitizados, no fechas de commit inventadas.

Secuencia pública prevista:

`v0.1.0 → v0.1.1 → v0.1.2 → v0.2.0 → v0.3.0 → v0.3.1 → v0.3.2 → v0.3.3 → v0.3.4 → v0.3.5 → v0.3.6 → v0.4.0 → v0.4.1`

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
</p>
