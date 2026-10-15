<h1 align="center">Evolución de arquitectura · entregas documentadas</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
  <img src="../assets/nav/arquitectura-current.svg" alt="Arquitectura · página actual">
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
</p>

<p align="center">
  <a href="architecture.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="architecture.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

<p align="center"><strong>Primero leer los bytes. Interpretar solo cuando la evidencia lo permite. Comparar sin borrar las fronteras de origen.</strong></p>

> **Alcance.** Esta página reconstruye la arquitectura representada por las entregas archivadas desde **v0.1.0 hasta v0.4.2**. Ese rango es un límite documental, no la arquitectura final del proyecto. Las versiones posteriores deben ampliar la línea temporal sin convertir v0.4.1 en un punto de llegada definitivo.

OTDR Inside evolucionó añadiendo capas alrededor de una frontera estable de datos crudos. Primero separó estructura SOR de semántica de fabricante; después incorporó eventos calculados, evidencia, EI como segunda fuente, razonamiento terminal y finalmente un espacio de trabajo multiarchivo que compara trazas sin fusionar su procedencia.

## Mapa de evolución

<p align="center">
  <img src="../assets/architecture/evolution-es-light.svg#gh-light-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo claro" width="100%">
  <img src="../assets/architecture/evolution-es-dark.svg#gh-dark-mode-only" alt="Evolución de arquitectura de OTDR Inside en modo oscuro" width="100%">
</p>

## Desarrollo arquitectónico por versión

### v0.1.0–v0.1.2 · columna estructural y envolvente de ejecución

v0.1.0 establece la ruta por archivo que se mantiene:

`SOR → scanner → perfil/normalización → traza → visor`

Lectura estructural e interpretación semántica son responsabilidades distintas desde el comienzo. v0.1.1 y v0.1.2 robustecen arranque y despliegue alrededor de ese motor sin rediseñar la ruta de medición.

### v0.2.0 · la evidencia entra en la interpretación

La semántica de curva Ceyear pasa a una capa de perfiles respaldada por evidencia. Los valores crudos permanecen junto a los normalizados, el soporte se expresa por capacidad y leer estructura deja de equivaler a soporte semántico.

### v0.3.0–v0.3.2 · el análisis de eventos aparece después de la traza

Los candidatos calculados se generan **después** de reconstruir la traza. Los eventos almacenados y los calculados no necesitan compartir categoría de origen.

v0.3.1 separa salida del detector, evidencia y revisión humana. v0.3.2 añade refinamiento contextual y conserva motivos de propuestas suprimidas.

### v0.3.3 · segunda ruta de entrada

Un lector EI defensivo introduce una segunda fuente. El emparejamiento EI/SOR pasa a basarse en evidencia y la información EI obtiene su propia procedencia.

### v0.3.4–v0.3.6 · razonamiento terminal y consistencia entre formatos

v0.3.4 incorpora región terminal diagnóstica y experimentos de localización. v0.3.5 añade una compuerta de evidencia para un posible final no reflectivo. v0.3.6 se concentra en discretización consistente EI/SOR, selección de propuestas duplicadas y localización a tres escalas de respuestas débiles.

Esta fase fortalece el análisis sin cambiar el scanner estructural ni las categorías básicas de procedencia.

### v0.4.0 · el espacio de trabajo se vuelve una capa arquitectónica

El cambio principal de 0.4 no es otro parser: es un **espacio de trabajo alrededor de múltiples análisis independientes**.

v0.4.0 añade:

- hasta ocho pestañas por archivo;
- zoom, selección, anotaciones y revisión aislados por pestaña;
- asociación EI iniciada explícitamente desde una pestaña SOR;
- comparación de dos a cuatro trazas;
- CSV de comparación y condiciones de adquisición;
- diagnóstico reorganizado;
- una regla de competencia terminal más estricta en el detector.

Cada archivo sigue pasando por el mismo motor por archivo. El espacio de trabajo conserva y presenta varios modelos; no los aplana dentro de una sola medición.

### v0.4.1 · se corrige la comparación sin cambiar el detector

v0.4.1 es deliberadamente un refinamiento del espacio de trabajo/presentación.

El módulo `events_hybrid.py` es idéntico byte a byte a v0.4.0. Los cambios importantes aparecen en el front-end:

- agrupamiento de cercanía entre cualquier par seleccionado, no solo respecto al primero;
- vistas tramo común/recorrido completo para alcances distintos;
- referencia vertical por mediana 5–50 m en lugar de la primera muestra;
- atenuación del ruido terminal únicamente para el dibujo comparativo.

Esto demuestra una frontera útil: **la política de comparación puede evolucionar independientemente de la detección de eventos**.

### v0.4.2 · auditoría de evidencia y localización reversible

La documentación archivada de v0.4.2 describe **cambios sucesivos bajo un mismo número de entrega**; no aparece otro scanner estructural ni se sustituye el espacio de trabajo multiarchivo.

El comparador incorpora grados de estabilidad/evidencia visibles y acceso a candidatos excluidos cercanos con sus reglas; los excluidos no cuentan como parejas aceptadas. Un ensayo de ±10 % del umbral mínimo de ruido terminal se usa para **advertir** sobre posibles finales no reflectivos inestables, sin redefinir la lista base de eventos.

Un método experimental `edge_precision.py`, seleccionable por separado, puede desplazar posiciones calculadas de clases soportadas y conserva la posición original, clase, procedencia e historial de revisión. El visor abre en la lectura base. Las posiciones importadas de EI no se reescriben. Sesiones e informes conservan el método y las comparaciones.

Esto es un **ensayo de localización**, no una nueva procedencia ni una mejora universal validada: empeora la concordancia retrospectiva frente a EI aunque mejore frente a la referencia EXFO utilizada en el estudio.

<p align="center">
  <img src="../assets/architecture/v042-es-light.svg#gh-light-mode-only" alt="Análisis, evidencia y localización experimental documentados en v0.4.2, tema claro" width="100%">
  <img src="../assets/architecture/v042-es-dark.svg#gh-dark-mode-only" alt="Análisis, evidencia y localización experimental documentados en v0.4.2, tema oscuro" width="100%">
</p>

## Hilo arquitectónico entre versiones

| Aspecto | Estado inicial | Expansión hasta v0.4.1 |
|---|---|---|
| **Estructura** | Lectura SOR segura | Frontera estable de datos crudos en toda la secuencia archivada. |
| **Semántica** | Interpretación por perfiles | Evidencia, estado por capacidad y semántica no soportada explícita. |
| **Eventos** | Eventos EXFO almacenados | Candidatos calculados, evidencia, revisión y razonamiento terminal. |
| **Fuentes** | Solo SOR | Lectura EI y pareja EI/SOR verificada. |
| **Estado de sesión** | Un análisis activo | Hasta ocho pestañas independientes con estado propio. |
| **Comparación** | Alternancia manual | Comparación 2–4 y agrupamiento visual entre cualquier par. |
| **Diagnóstico** | Alertas/detalles básicos | Estado legible, hallazgos, advertencias y auditoría técnica. |

Dos componentes permanecen idénticos byte a byte desde v0.1.0 hasta v0.4.2: `scanner.py` y el perfil EXFO validado. El sistema conserva por tanto un contrato estructural de larga duración mientras interpretación e interacción crecen alrededor.

## Base de implementación · v0.4.1 (ampliada en v0.4.2)

El diagrama siguiente documenta la **base por archivo y espacio de trabajo de v0.4.1 conservada en la línea 4.2**. Las adiciones de 4.2 aparecen por separado arriba; no es una arquitectura final.

<p align="center">
  <img src="../assets/architecture/system-es-light.svg#gh-light-mode-only" alt="Instantánea de implementación OTDR Inside v0.4.1 en modo claro" width="100%">
  <img src="../assets/architecture/system-es-dark.svg#gh-dark-mode-only" alt="Instantánea de implementación OTDR Inside v0.4.1 en modo oscuro" width="100%">
</p>

## Modelo de procesamiento

### 1. Ruta SOR por archivo

1. `scanner.py` valida estructura y expone bloques crudos.
2. `normalizer.py` y los perfiles añaden únicamente semántica soportada.
3. `trace.py` reconstruye distancia y nivel relativo.
4. `events_baseline.py`, `events_hybrid.py`, `event_evidence.py`, `terminal.py` y `localization.py` generan candidatos/diagnósticos conservando origen calculado.
5. `viewer_model.py` construye el modelo por archivo.

### 2. Ruta EI y asociación explícita

`ei_reader.py` y `ei_inspection.py` interpretan de forma defensiva la familia EI observada. Un EI puede abrirse en una pestaña independiente. Para usarlo como compañero, v0.4 exige iniciar la asociación desde una pestaña SOR y superar la verificación por contenido/metadatos.

Una pareja verificada cambia la relación entre archivos, no su procedencia.

### 3. Espacio de trabajo multiarchivo

La capa v0.4 vive principalmente en `web/app.js`:

- hasta ocho análisis pueden coexistir;
- cada pestaña conserva zoom, evento seleccionado, anotaciones y revisión;
- cerrar una pestaña con cambios de sesión sin exportar exige confirmación;
- un archivo rechazado no elimina los análisis que sí pudieron abrirse.

El espacio de trabajo guarda **varios modelos independientes**, no una traza fusionada.

### 4. Capa de comparación

El comparador selecciona de dos a cuatro análisis abiertos.

En v0.4.1:

- agrupa posiciones próximas entre todos los archivos seleccionados;
- conserva eventos sin pareja;
- mantiene como máximo un evento de cada archivo en una fila;
- ofrece tramo común y recorrido completo;
- usa una mediana 5–50 m como referencia visual;
- atenúa el ruido terminal solo en el dibujo.

El comparador no reescribe los resultados por archivo. Construye una **vista de relación** entre resultados existentes.

## Mapa de implementación

| Capa | Módulo(s) archivados | Responsabilidad | Frontera deliberada |
|---|---|---|---|
| **Servidor local** | `visor_otdr.py` | Interfaz HTTP, solicitudes de análisis y recursos estáticos. | Flujo local; no reescribe mediciones fuente. |
| **Lectura estructural** | `scanner.py` | Recorre SOR, valida límites y expone metadatos crudos. | Legibilidad no implica certeza semántica. |
| **Perfiles / normalización** | `normalizer.py`, `profiles/*`, `export_signature.py` | Aplica semántica caracterizada y conserva evidencia/confianza. | Los valores crudos permanecen. |
| **Traza** | `trace.py`, `numerics.py` | Reconstrucción y políticas numéricas. | Nivel relativo ≠ potencia calibrada universal. |
| **Eventos / terminal** | `events_baseline.py`, `events_hybrid.py`, `event_evidence.py`, `terminal.py`, `localization.py` | Genera y audita hipótesis calculadas. | No certifica pérdida, reflectancia, ORL ni final físico. |
| **Interpretación EI** | `ei_reader.py`, `ei_inspection.py` | Lee el diseño EI observado e importa registros soportados. | Las variantes desconocidas no se fuerzan. |
| **Modelo por archivo** | `viewer_model.py` | Combina estructura, semántica, traza, eventos y EI opcional. | La procedencia permanece explícita. |
| **Espacio de trabajo** | `web/app.js`, `web/index.html`, `web/styles.css` | Pestañas, estado de sesión, cambio de archivo, comparación y exportaciones. | Los análisis coexisten sin fusionarse. |
| **Comparador** | funciones v0.4.1 de `web/app.js` | Grupos de cercanía, tramo común/completo y referencia visual. | Cercanía es relación de presentación, no identidad física. |
| **Evidencia/localización v0.4.2** | `edge_precision.py` y cambios de interfaz/informes documentados | Excluidos explicados, sensibilidad terminal y método de posición opcional por clase. | No se sobrescriben silenciosamente el detector base ni los valores EI. |

## Invariantes arquitectónicos

- **Fuentes de solo lectura.**
- **Estructura antes que semántica.**
- **Crudo antes que normalizado.**
- **Almacenado/importado/calculado permanecen separados.**
- **Pareja EI/SOR por evidencia, no por nombre.**
- **Estado aislado por archivo.**
- **La comparación no modifica el análisis.**
- **Cercanía no es identidad.** Los 15 m del comparador no son tolerancia metrológica.
- **Normalización visual no es calibración.** Tramo común/completo y mediana 5–50 m cambian el dibujo, no la medición.

## Qué no afirma esta arquitectura

El espacio de trabajo v0.4.1 facilita inspección cruzada; no hace físicamente comparables archivos tomados bajo cualquier condición. Posiciones semejantes pueden aparecer en enlaces, longitudes de onda, pulsos o índices distintos sin corresponder al mismo elemento. El agrupamiento actual es codicioso y no se presenta como algoritmo óptimo de asociación física.

La concordancia EI, la referencia EXFO y la evidencia terminal siguen siendo evidencia de ingeniería dentro de condiciones documentadas, no calibración universal ni certificación independiente de Telcordia.

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
</p>
