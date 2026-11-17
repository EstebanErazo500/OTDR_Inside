<h1 align="center">Casos de estudio · Decisiones de ingeniería</h1>

<p align="center">
  <a href="../README.es.md"><img src="../assets/nav/inicio-proyecto.svg" alt="Inicio"></a>
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
  <a href="how-it-works.es.md"><img src="../assets/nav/como-funciona.svg" alt="Recorrido guiado"></a>
  <a href="compatibility-matrix.es.md"><img src="../assets/nav/compatibilidad.svg" alt="Compatibilidad"></a>
  <img src="../assets/nav/casos-estudio-current.svg" alt="Casos de estudio">
  <a href="validation.es.md"><img src="../assets/nav/validacion.svg" alt="Validación"></a>
</p>

<p align="center">
  <a href="case-studies.md"><img src="../assets/nav/lang-en.svg" alt="English"></a>
  <a href="case-studies.es.md"><img src="../assets/nav/lang-es-selected.svg" alt="Español"></a>
</p>

> **Alcance:** v0.4.5 consolidada el 5 de noviembre de 2026. Estos casos conectan los resultados retrospectivos ya documentados en Validación con ejemplos didácticos inventados. No son nuevas pruebas del visor, capturas, trazas operativas ni registros individuales.

<p align="center">
  <img src="../assets/case-studies/examples-es-light.svg#gh-light-mode-only" width="100%" alt="Dos ejemplos sintéticos">
  <img src="../assets/case-studies/examples-es-dark.svg#gh-dark-mode-only" width="100%" alt="Dos ejemplos sintéticos">
</p>

## Caso A · ¿Por qué discrepan las posiciones de eventos?

### Problema y decisión de ingeniería

Dos OTDR pueden localizar una misma transición en posiciones diferentes por el origen de distancia, el índice de grupo, el pulso, el muestreo o el criterio de localización. Una pareja propuesta no demuestra identidad física.

1. Verificar enlace, onda, pulso y relación entre adquisiciones; no contar una copia SOR de SOZ como repetición independiente.
2. Conservar las posiciones originales y su procedencia (nativa, EI o calculada).
3. Revisar IOR y origen; reexpresar condicionalmente el eje cuando los índices sean válidos, sin modificar los originales.
4. Revisar correspondencias uno a uno, registros sin pareja y diferencias tanto de posición absoluta como de separación.
5. Conservar las discrepancias entre referencias y casos desfavorables, incluso cuando una media mejore.

EXFO y Yokogawa actúan como referencias documentales pares en el montaje estudiado; JDSU aporta corroboración y EI el registro de su adquisición Ceyear. Ninguna referencia constituye automáticamente verdad física calibrada.

### Ejemplo sintético · desplazamiento común

Supónganse dos eventos ya asociados, sobre ejes comparables:

| Magnitud | Referencia A | Traza B | B − A |
|---|---:|---:|---:|
| Evento 1 | 1000 m | 1006 m | +6 m |
| Evento 2 | 4000 m | 4006 m | +6 m |
| Separación 2 − 1 | 3000 m | 3000 m | 0 m |

La separación cancela el desplazamiento aditivo común en este ejemplo, pero no prueba que proceda del origen ni que las distancias sean físicamente exactas.

Para una misma demora de ida y vuelta, la relación ideal es d = c·t/(2·n_g). Reexpresar a un índice de referencia válido implica d_ref = d_original·n_original/n_ref, si el origen y la interpretación temporal son compatibles. Es física conceptual, no una exposición del código; tampoco determina el índice verdadero.

### Evidencia retrospectiva · v0.4.4

| Referencia (72 contrastes de eventos) | Diferencia absoluta media base | Borde adaptativo |
|---|---:|---:|
| EXFO | 4,55 m | 2,88 m |
| JDSU | 4,54 m | 3,88 m |
| EI emparejado | 3,78 m | 5,57 m |

En 14 contrastes frente a EXFO la diferencia empeoró. En 4.5 también se ensayó un selector alternativo de posición con dos referencias, que no se activó porque empeoraba separaciones y algunos peores casos.

**Conclusión:** mejorar una media no basta para elegir un método universal. Las cifras representan concordancia retrospectiva del montaje estudiado, no errores frente a distancias trazables ni 72 enlaces independientes.

## Caso B · ¿Qué hacer cuando discrepan la pérdida LSA y la nativa?

### Problema y decisión de ingeniería

En una transición no reflectiva, ventanas afectadas por ruido, recuperación u otra transición pueden alterar la pérdida estimada. La morfología sugerida por IA tampoco constituye una medición.

En 4.5 se conserva la pérdida nativa y se añade una estimación local por mínimos cuadrados (LSA) para transiciones aptas: conceptualmente se ajustan niveles a ambos lados y se compara la diferencia en una posición común. Seis combinaciones de ventanas permiten revisar sensibilidad y apoyo; no son seis mediciones independientes ni un intervalo de incertidumbre calibrado. La asesoría Random Forest ayuda a revisar forma y contexto, sin sobrescribir el dato nativo.

### Ejemplo sintético · sensibilidad de ventanas

Los valores son inventados; no proceden del visor, no representan su regla interna ni fijan tolerancias.

| Variante | Ajustes próximos | Ajustes sensibles |
|---|---:|---:|
| 1 | 0,19 dB | 0,06 dB |
| 2 | 0,20 dB | 0,10 dB |
| 3 | 0,21 dB | 0,23 dB |
| 4 | 0,20 dB | 0,31 dB |
| 5 | 0,18 dB | 0,44 dB |
| 6 | 0,22 dB | 0,15 dB |
| Máx. − mín. | 0,04 dB | 0,38 dB |

Si la pérdida nativa hipotética fuera 0,20 dB, el primer caso mostraría resultados próximos; el segundo requeriría revisar ventanas y forma de señal. Ni una dispersión pequeña ni una coincidencia con el dato nativo garantizan exactitud.

### Evidencia retrospectiva · v0.4.5

De 54 estimaciones locales Ceyear, 49 tuvieron apoyo y 5 quedaron para revisión.

| Referencia documental | Diferencia absoluta media, 1310 nm | 1550 nm |
|---|---:|---:|
| EXFO agrupado | 0,0087 dB | 0,0202 dB |
| Yokogawa, regiones cubiertas | 0,0025 dB | 0,0064 dB |

Las regiones cubiertas no son idénticas y los registros son dependientes del montaje. Las cifras no son incertidumbre calibrada ni demuestran superioridad de un fabricante. En otro lote, la revisión numérica señaló 75 casos y el flujo IA/contexto 167, incluidos 92 adicionales; son conteos de revisión, no precisión ni sensibilidad de clasificación.

**Conclusión:** mantener pérdida nativa, LSA, evidencia y revisión separadas permite auditar discrepancias sin corregir automáticamente por un puntaje RF.

## Estado de desarrollo

| Estado al corte documental | Elementos |
|---|---|
| Incorporado en 4.5 | SOR/EI/MSOR/SOZ caracterizados, asociaciones revisables, IOR condicional, separaciones, LSA y asesoría RF contextual, informes. |
| Experimental en la línea 4.x | Alternativas de localización con resultados dependientes de la referencia. |
| Evaluado sin activar | Selector de posición con doble referencia y estudios de modelos externos. |
| Ensayo separado del 6 de noviembre | Parche de centrado vertical/aviso IOR, no empaquetado en 4.5. |
| Pendiente según el registro | Lectura directa STDR, nuevas referencias independientes y verificación real en Windows/navegador. |

El corte no implica haber inspeccionado versiones posteriores. Los resultados archivados se amplían en [Validación](validation.es.md) e [Historial de versiones](version-history.es.md); la publicación se rige por [Manejo de datos](data-handling.es.md).

---

<p align="center">
  <a href="README.es.md"><img src="../assets/nav/documentacion-tecnica.svg" alt="Documentación técnica"></a>
</p>
