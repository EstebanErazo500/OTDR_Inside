<h1 align="center">Validación · jerarquía de evidencia y límites documentados</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <img src="../assets/nav/validacion-current.svg" alt="Validación · página actual">
</p>

<p align="center">
  <a href="validation.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="validation.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

<p align="center"><strong>La validación es una jerarquía de evidencia, no una única puntuación de exactitud.</strong></p>

> **Alcance.** Esta página resume el modelo de validación documentado por las entregas archivadas hasta **v0.4.1**. Las trazas operativas y los corpus privados permanecen fuera del repositorio, por lo que se publican métodos y resultados agregados sin exponer rutas, clientes, nombres de archivos privados ni hashes.

<p align="center">
  <img src="../assets/validation/layers-es-light.svg#gh-light-mode-only" alt="Jerarquía de validación de OTDR Inside en modo claro" width="100%">
  <img src="../assets/validation/layers-es-dark.svg#gh-dark-mode-only" alt="Jerarquía de validación de OTDR Inside en modo oscuro" width="100%">
</p>

## Capas de validación

| Capa | Qué se comprueba | Qué permite respaldar |
|---|---|---|
| **Seguridad estructural** | Límites de bloques, revisiones, tamaños, rutas y entradas inválidas. | El archivo puede inspeccionarse sin aceptar estructura malformada como datos válidos. |
| **Regresión semántica de perfiles** | Reglas conocidas, valores crudos/normalizados y estados no soportados. | Un significado concreto está respaldado para un perfil caracterizado. |
| **Reconstrucción de traza** | Muestras, eje de distancia, nivel relativo y determinismo. | La curva mostrada es reproducible bajo la interpretación documentada. |
| **Regresión de eventos / terminal** | Posiciones/clases, motivos de exclusión, lógica terminal y salida determinista. | El algoritmo se comporta consistentemente en los casos probados. |
| **Consistencia de formatos emparejados** | Relación EI/SOR, metadatos y asociación de eventos. | Dos formatos relacionados se manejan consistentemente dentro de la familia caracterizada. |
| **Regresión de espacio de trabajo / comparación** | Pestañas, estado aislado, asociación EI, agrupamiento y reglas de dibujo. | La interacción multiarchivo preserva resultados por archivo y aplica la lógica de presentación documentada. |
| **Revisión manual / referencia** | Renderizado, software de referencia e inspección de campo. | La interfaz e interpretación pueden contrastarse externamente conservando sus límites. |

Ninguna capa por sí sola convierte el proyecto en un estándar OTDR calibrado.

## Evidencia pública y evidencia privada

| Tipo de evidencia | Repositorio público | Uso |
|---|---|---|
| Fixtures sintéticos o explícitamente anonimizados | **Aptos para publicación** | Parser, normalización, lógica determinista y CI/regresión pública. |
| Mediciones operativas SOR/EI reales | **Privadas** | Regresión de ingeniería y comportamiento de campo. |
| Nombres, hashes, rutas o seriales de corpus privados | **Privados** | Reproducibilidad dentro del entorno autorizado. |
| Ejecutables/manuales de fabricante y estándares licenciados | **No se redistribuyen** | Referencia externa cuando su uso es legal y operativo. |
| Resultados agregados de validación | **Documentables públicamente** | Explicar qué se comprobó sin exponer datos operativos. |

La migración pública debe reproducir todo lo posible con datos sintéticos/anonimizados y mantener la regresión privada fuera de GitHub.

## Evidencia por versión

### v0.1.0 · base automatizada

La primera rebanada vertical archivada registra **23 pruebas automáticas aprobadas** alrededor del parser, perfiles, traza y visor.

### v0.3.2 · punto de control del detector

El material archivado de 0.3.2 registra **59 pruebas** con material controlado Ceyear y EXFO. También documenta mejor recuperación de referencias intermedias, dejando claro que esas referencias **no eran datos ciegos**.

La concordancia de desarrollo es evidencia de progreso, no una estimación independiente de exactitud.

### v0.3.4 y v0.3.5 · crece la regresión terminal

Los resultados archivados registran **71 pruebas** en v0.3.4 y **76 pruebas** en v0.3.5 a medida que se incorporan diagnóstico y evidencia terminal. El aumento de conteo refleja cobertura adicional, no una puntuación de calidad directamente comparable.

### v0.3.6 · consistencia entre formatos emparejados

La validación de liberación de v0.3.6 registra:

| Comprobación | Resultado documentado |
|---|---|
| Validación de liberación | **80 pruebas aprobadas** |
| Parejas EI/SOR controladas | **18** |
| Muestras útiles complementarias comprobadas | 18 × 16.384 |
| Registros EI posteriores al inicio asociados con candidatos | **72/72** |
| Candidatos calculados extra en ese corpus de parejas | **0** |
| Índices/clases entre análisis EI/SOR | Coinciden en las 18 parejas |
| SOR privados de campo examinados | **27** |
| Trazas con índices/clases sin cambio frente a v0.3.5 | **22** |
| Trazas con cambios que requieren revisión | **5** |

El informe archivado también registra una diferencia absoluta media SOR frente a EI de **3,782 m** y máxima de **17,154 m** en el conjunto controlado. Esas cifras se presentan como concordancia bajo condiciones estudiadas, **no como exactitud del OTDR ni incertidumbre**.

La versión mejora de forma puntual la localización de una respuesta débil, pero el propio informe la clasifica como validación retrospectiva, no independiente.

### v0.4.0 · espacio de trabajo y competencia terminal más estricta

v0.4.0 vuelve a ejecutar el detector sobre:

- **27 SOR privados de campo**;
- **18 parejas EI/SOR controladas**.

Las parejas mantienen **72/72 asociaciones**, sin candidatos calculados adicionales sin pareja. Frente a v0.3.6, solo una traza privada cambia de clase terminal bajo la regla reflectiva más estricta.

La conclusión es acotada: la regla afecta el caso terminal esperado y conserva el comportamiento del corpus controlado. No certifica el extremo físico.

### v0.4.1 · regresión del comparador

v0.4.1 conserva el detector de v0.4.0 y valida la capa de comparación corregida.

La validación archivada registra:

- **54 pruebas Python aprobadas**;
- **6 pruebas de corpus externo omitidas** en el descubrimiento estándar;
- JavaScript de producción con DOM/canvas simulado;
- agrupamiento entre todos los pares en el caso probado;
- cambio entre tramo común y recorrido completo;
- referencia visual por mediana 5–50 m en el caso atípico probado;
- conservación del estado por pestaña y asociación EI explícita.

En el momento de esa validación quedaba pendiente una inspección visual final del comparador revisado en un navegador Windows real. Esa comprobación pendiente se conserva como parte del registro.

## Por qué el número de pruebas no es una puntuación

Los conteos cambian porque las suites evolucionan:

- aparecen módulos y pruebas nuevas;
- algunas versiones usan corpus externos/privados no disponibles en el descubrimiento estándar;
- ciertas pruebas de navegador o corpus son condicionales;
- una entrega puede concentrarse en detector y otra en interacción multiarchivo.

Por tanto, **80 frente a 54 no significa que una versión esté “más validada” que otra**. Importa qué afirmaciones cubre cada suite y bajo qué datos.

## Límites específicos del comparador

### Umbral de cercanía de 15 m

Es una ayuda visual. No es:

- tolerancia calibrada;
- regla de identidad de eventos;
- prueba de identidad de enlace;
- intervalo de confianza;
- especificación de exactitud.

### Agrupamiento entre todos los pares

v0.4.1 mejora 0.4.0 al revisar cercanía entre todos los archivos seleccionados y no solo contra el primero. El agrupamiento sigue siendo codicioso y puede no ser óptimo cuando hay varios eventos muy cercanos.

### Referencia de nivel y escala horizontal

La mediana 5–50 m, la atenuación del ruido terminal y el selector tramo común/completo son **reglas de dibujo**. No modifican muestras, posiciones, pérdidas ni procedencia.

## Qué haría falta para afirmar exactitud física

Un estudio metrológico requeriría al menos:

- puntos de referencia con distancia trazable e incertidumbre conocida;
- definición explícita del origen;
- índice de grupo y parámetros de adquisición verificados;
- adquisiciones repetidas por onda y pulso;
- etiquetas independientes por clase de evento;
- suficientes repeticiones para estimar variabilidad;
- material de validación ciego no usado para ajustar el detector.

Hasta entonces, las distancias deben describirse como **concordancia algoritmo/referencia bajo condiciones concretas**, no como exactitud universal del OTDR.

## Reglas que deben sobrevivir a la migración pública

- conservar crudo y normalizado por separado;
- mantener procedencia almacenada/importada/calculada/manual;
- mantener la regresión operacional privada fuera de GitHub;
- usar fixtures públicos sintéticos/anonimizados;
- indicar cuándo una prueba depende de corpus externo;
- separar regresión automatizada de inspección manual de navegador;
- separar cercanía visual de tolerancia física;
- conservar evidencia negativa, fallida o pendiente en lugar de publicar solo éxitos.

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
</p>

<p align="center"><sub>Siguiente página independiente: Manejo de datos.</sub></p>
