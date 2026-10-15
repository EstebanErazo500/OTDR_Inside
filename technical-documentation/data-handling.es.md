<h1 align="center">Manejo de datos · límites de publicación</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio del proyecto"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="version-history.es.md"><img src="../assets/nav/historial-versiones.svg" alt="Historial de versiones"></a>
  <a href="architecture.es.md"><img src="../assets/nav/arquitectura.svg" alt="Arquitectura"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/procedencia-eventos.svg" alt="Procedencia de eventos"></a>
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
  <img src="../assets/nav/manejo-datos-current.svg" alt="Manejo de datos · página actual">
</p>

<p align="center">
  <a href="data-handling.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="data-handling.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

<p align="center"><strong>Explicar la ingeniería. Mostrar la interfaz. Proteger el código fuente y las mediciones operativas.</strong></p>

## Propósito y autorización vigente

OTDR Inside se documenta públicamente como portafolio de ingeniería e investigación. **El alcance de publicación comunicado actualmente permite capturas revisadas del visor, explicaciones de arquitectura de alto nivel y representaciones de trazas generales/sintéticas, pero no autoriza publicar el código fuente del visor.** Esta distinción es deliberada. El acceso público a GitHub **no** concede permiso para copiar, redistribuir o licenciar la implementación no publicada.

Esta política aplica al repositorio y a todo su historial: ramas, solicitudes de cambio, adjuntos, etiquetas, releases y artefactos automatizados. Cualquier ampliación de permisos deberá confirmarse por separado.

<p align="center">
  <img src="../assets/data-handling/boundary-es-light.svg#gh-light-mode-only" alt="Frontera entre materiales públicos, sujetos a revisión y excluidos, tema claro" width="100%">
  <img src="../assets/data-handling/boundary-es-dark.svg#gh-dark-mode-only" alt="Frontera entre materiales públicos, sujetos a revisión y excluidos, tema oscuro" width="100%">
</p>

## Matriz de clasificación

| Material | Decisión actual | Regla de aplicación |
|---|---|---|
| Objetivos, historial de versiones y metodología agregada | **Publicar** | Explicar la contribución real de cada versión; separar observaciones, afirmaciones y límites pendientes. |
| Arquitectura conceptual y diagramas funcionales/de flujo | **Publicar tras revisar contenido** | Mostrar responsabilidades e interfaces sin revelar implementación propietaria, credenciales o detalles internos confidenciales. |
| Capturas del visor | **Permitidas tras revisar cada imagen** | Eliminar o sustituir nombres, metadatos, rutas, identificadores, anotaciones privadas y contenido sensible de la traza. Permitir la interfaz no implica autorizar todo lo que pueda verse dentro de ella. |
| Gráficos generales y trazas completamente sintéticas | **Publicar** | Crear ejemplos de manera independiente; identificarlos como ilustrativos/sintéticos, nunca como mediciones de campo validadas. |
| Resultados agregados de validación | **Publicar tras revisar** | Indicar tamaño de muestra, condiciones, referencias, omisiones y límites; excluir identidades de archivos, huellas, ubicaciones y magnitudes sensibles. |
| SOR, EI, OTDR, exportaciones CSV/JSON, sesiones, informes y curvas derivadas reales | **No publicar** | Pueden reconstruir mediciones operativas o exponer metadatos identificables aunque cambie el nombre del archivo. |
| Código fuente original o adaptado, pruebas, scripts de construcción, perfiles y configuración interna | **No publicar** | No existe autorización vigente para distribuir la implementación. Cambiar nombres o limpiar rutas no convierte el código en publicable. |
| Software de fabricantes, ejecutables, DLL, manuales propietarios y normas licenciadas | **No redistribuir** | Describir conceptos públicos y enlazar fuentes legítimas cuando corresponda. |
| Clientes, rutas, personal, equipos, seriales, nombres/hashes privados y rutas absolutas | **No publicar** | También pueden estar en tooltips, vistas de impresión, exportaciones, textos ocultos SVG, metadatos e historial de Git. |

**Regla para un material dudoso:** mantenerlo fuera de GitHub hasta comprobar tanto su contenido como el permiso correspondiente. El repositorio público no tiene una zona privada.

## Formatos de demostración permitidos

### 1. Capturas del visor real

Una captura puede enseñar la distribución de la aplicación y sus interacciones: gráfica, comparador, pestañas, distintivos de procedencia, diagnóstico o vista de informes. Antes de publicarla, revisar **toda el área capturada**, incluido navegador, tooltips, rutas, barra de título, barra de tareas, paneles y encabezados de informe. Es preferible preparar una sesión de demostración con información permitida; tapar texto en una imagen no garantiza por sí solo la ausencia de otros datos ocultos.

La captura debe representar una **versión real**. Si se sustituyeron rótulos o curvas, indicarlo en el pie de imagen para no presentarla como medición operativa original.

### 2. Curvas generales o sintéticas

Preferir curvas creadas independientemente a partir de escenarios matemáticos o artificiales (escalón, reflexión, ruido terminal) o un esquema no basado en mediciones. **No** deben ser copias recortadas, reescaladas, remuestreadas o ligeramente modificadas de trazas privadas. La topología de la fibra, la secuencia de eventos o los valores todavía podrían permitir reconocer una medición real.

Identificar las ilustraciones con una nota equivalente a: *«Traza sintética ilustrativa: no corresponde a datos de campo ni es patrón de exactitud física»*.

### 3. Resúmenes de arquitectura y validación

Pueden mostrarse capas conceptuales de lectura SOR/EI, interpretación por perfiles, análisis de trazas, procedencia, espacio de trabajo y comparación. No hace falta publicar fórmulas internas, reglas propietarias, listados de implementación ni módulos fuente completos para explicar el diseño.

La validación puede presentarse mediante resultados agregados y sus limitaciones. Por ejemplo, la concordancia con un archivo de fabricante u otro visor **no** debe llamarse «exactitud física de distancia» sin referencia independiente trazable.

## Revisión previa a publicar

Antes de incorporar o actualizar cualquier material:

1. **Permiso:** ¿este tipo de material está dentro del alcance autorizado? El código fuente no lo está.
2. **Contenido:** comprobar clientes, lugares, rutas, personas, IDs, seriales, nombres reales, hashes, URL internas y rutas del sistema.
3. **Mediciones:** confirmar ausencia de traza operativa, tabla de eventos real, muestras crudas, informes exportados o mediciones reconstruibles.
4. **Datos ocultos:** inspeccionar EXIF y propiedades PDF, adjuntos incrustados, texto de archivos SVG, comentarios, etiquetas accesibles, enlaces y capas alternativas.
5. **Rigor técnico:** rotular ejemplos sintéticos y diferenciar eventos almacenados/importados/calculados, concordancia y exactitud calibrada.
6. **Cambios preparados:** revisar todos los archivos nuevos/modificados y su contenido real, no solo la presentación en GitHub.
7. **Historial:** borrar archivos de la carpeta visible **no elimina** los blobs publicados previamente ni las copias en caché. Ante una exposición, detener la difusión y coordinar contención y saneamiento del historial.
8. **Comprobación visual:** verificar gráficos en temas claro/oscuro, navegación y pies de imagen antes de cerrar la publicación.

El archivo `.gitignore` reduce accidentes habituales, pero **no garantiza confidencialidad** ni impide que se agreguen archivos explícitamente o permanezcan en commits anteriores. Una rama de un repositorio **público** no es un lugar seguro para almacenar código confidencial.

## Qué no ofrece deliberadamente el repositorio

Actualmente GitHub ofrece **documentación, diagramas conceptuales y demostraciones autorizadas**, no una copia descargable o compilable del visor propietario. No se afirma que la regresión privada pueda ejecutarse desde este repositorio. **No deben crearse releases de código, binarios, paquetes ejecutables ni licencias de código abierto del visor sin una nueva autorización expresa y revisión de derechos.**

Las próximas adiciones pueden ser una galería de capturas revisadas y trazas ilustrativas originales. Una futura versión del programa no cambia por sí misma el permiso de publicación del código.

## Control de cambios

Para cada publicación conservar en el ámbito privado un registro breve: tipo de material, versión representada, persona revisora, base del permiso, ajustes de sanitización y fecha. No subir conversaciones privadas de autorización ni archivos operativos como prueba.

Si se publica información por error, suspender nuevas divulgaciones, retirar el acceso público directo cuando sea posible y coordinar una corrección pronta con los responsables del proyecto. Eliminarla en un commit posterior no basta.

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
</p>
