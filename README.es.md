<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero-light.svg">
    <img src="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero-light.svg" alt="OTDR Inside" width="100%">
  </picture>
</p>

<p align="center">
  <strong>Análisis de trazas OTDR SOR con procedencia y evidencia explícitas.</strong><br>
  Lectura binaria segura, interpretación multifabricante, reconstrucción de trazas y análisis de eventos sin perder el origen de los datos.
</p>

<p align="center">
  <a href="#descripción-general">Descripción</a> ·
  <a href="#arquitectura">Arquitectura</a> ·
  <a href="#capacidades-actuales">Capacidades</a> ·
  <a href="#alcance-actual">Alcance</a> ·
  <a href="#validación">Validación</a> ·
  <a href="#hoja-de-ruta">Hoja de ruta</a>
</p>

<p align="center">
  <a href="README.md">English</a> · <strong>Español</strong>
</p>

---

<h2 id="descripción-general" align="center">Descripción general</h2>

Los archivos OTDR SOR están diseñados para almacenar mediciones de reflectometría óptica en el dominio del tiempo, pero en condiciones reales no siempre son semánticamente uniformes. Extensiones de fabricante, metadatos reescritos, escalas ambiguas y distintas representaciones de eventos pueden hacer que un archivo sea estructuralmente legible sin que todos sus valores tengan el mismo nivel de confianza.

OTDR Inside aborda este problema separando **estructura**, **interpretación**, **cálculo** y **confianza**. El objetivo no es forzar cada traza dentro de un modelo universal, sino mostrar qué se conoce, cómo se obtuvo y qué permanece sin resolver.

<h2 id="arquitectura" align="center">Arquitectura</h2>

<p align="center">
  <img src="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/architecture.svg" alt="Cadena de análisis de OTDR Inside" width="92%">
</p>

La cadena de procesamiento se divide deliberadamente en capas:

| Capa | Responsabilidad |
|---|---|
| **Lectura estructural** | Lee bloques SOR, revisiones, tamaños, offsets y regiones de muestras con comprobaciones explícitas de límites. |
| **Perfil y normalización** | Aplica reglas específicas de fabricante únicamente cuando la evidencia disponible las respalda. |
| **Análisis de traza** | Reconstruye el eje de distancia y la curva, y mantiene separados los eventos almacenados y los calculados. |
| **Presentación** | Expone parámetros, estructura, procedencia y eventos mediante un visor local y exportaciones JSON/CSV. |

Esta separación evita confundir legibilidad estructural con certeza semántica.

<h2 id="enfoque-de-ingeniería" align="center">Enfoque de ingeniería</h2>

Cuatro distinciones guían la implementación:

- **La estructura no es la semántica.** Un archivo puede leerse correctamente mientras algunas magnitudes permanecen sin interpretar.
- **El equipo no es necesariamente quien escribió el archivo.** Los bloques propietarios pueden identificar un linaje de software sin demostrar qué OTDR adquirió la traza.
- **Lo almacenado no es lo calculado.** Los valores o eventos añadidos por software de análisis permanecen diferenciados de la información presente en el SOR original.
- **Desconocido no significa corrupto.** Una semántica no soportada debe permanecer explícita en lugar de ser adivinada silenciosamente.

### Normalización con procedencia

Los campos normalizados conservan la evidencia necesaria para explicar el valor mostrado:

```python
normalized_field = {
    "value": ...,
    "raw_value": ...,
    "unit": ...,
    "source": ...,
    "rule_id": ...,
    "confidence": ...,
    "evidence": ...,
}
```

Así, una conversión empírica, una escala específica de fabricante o una interpretación inferida permanecen diferenciadas de un valor almacenado explícitamente en el archivo.

<h2 id="capacidades-actuales" align="center">Capacidades actuales</h2>

La línea de desarrollo actual integra:

- inspección segura de la estructura y metadatos SOR 2.00;
- selección de perfiles soportados basada en evidencia;
- reconstrucción de trazas a partir de las muestras almacenadas;
- visualización local e interactiva de la curva OTDR;
- presentación de parámetros, estructura y procedencia de interpretación;
- separación entre eventos almacenados y candidatos de evento calculados;
- exportación JSON del modelo de análisis y CSV de eventos;
- manejo seguro de variantes parcialmente soportadas sin modificar la traza original.

<h2 id="alcance-actual" align="center">Alcance actual · v0.3.x</h2>

| Ecosistema | Estado | Papel dentro del proyecto |
|---|---|---|
| **EXFO** | **Referencia validada** | Lectura SOR 2.00, parámetros normalizados, eventos almacenados y visualización de trazas. |
| **Ceyear CE6422** | **Desarrollo activo** | Interpretación de curva y detección de eventos calculados cuando `KeyEvents` no está presente. |
| **Yokogawa AQ1000** | **Estructural** | Estructura del archivo caracterizada; la normalización semántica específica del fabricante sigue pendiente. |

<p align="center">
  <strong>estructuralmente legible → perfil identificado → caracterizado semánticamente → validado empíricamente</strong>
</p>

<h2 id="procedencia-de-eventos" align="center">Procedencia de eventos</h2>

El manejo de eventos es una de las principales diferencias entre el lector inicial y la cadena actual de análisis.

Una tabla `KeyEvents` se trata como **información de eventos almacenada**. Los eventos propuestos a partir de la curva reconstruida se tratan como **candidatos calculados** y conservan ese origen en el modelo y en las exportaciones.

Esta distinción es especialmente importante en los archivos Ceyear utilizados durante el desarrollo: la ausencia de `KeyEvents` significa que el SOR **no contiene una tabla de eventos almacenada**; no demuestra que la traza óptica no contenga eventos.

<h2 id="validación" align="center">Validación</h2>

La validación se realiza en varios niveles en lugar de reducirse a un único criterio de aprobado o fallido:

1. comprobaciones estructurales y de límites;
2. pruebas sintéticas o anonimizadas para regresión pública;
3. regresión de perfiles contra trazas privadas de referencia;
4. verificación de reconstrucción de traza;
5. comparación con software de referencia cuando corresponde;
6. validación manual del visor local.

Las mediciones operativas empleadas durante la validación de ingeniería permanecen fuera del repositorio público.

<h2 id="manejo-de-datos" align="center">Manejo de datos</h2>

OTDR Inside está diseñado como un flujo **local y de solo lectura**. Las trazas fuente se analizan desde copias temporales y el visor no sobrescribe los archivos originales.

Este repositorio no distribuye mediciones operativas reales `.sor`, `.ei` u `.otdr`, identificadores de clientes o rutas, ejecutables propietarios de fabricantes, manuales comerciales, estándares licenciados ni archivos derivados que expongan metadatos confidenciales. Los ejemplos y pruebas públicas deben usar datos sintéticos o explícitamente anonimizados.

<h2 id="limitaciones-conocidas" align="center">Limitaciones conocidas</h2>

- El visor actual trabaja con `.SOR`; `.EI` y `.otdr` aún no forman parte del flujo normal de procesamiento.
- La interpretación específica de fabricante se basa en perfiles y no debe entenderse como compatibilidad universal con SOR.
- El nivel vertical mostrado de la traza es relativo y no se presenta como potencia óptica calibrada universalmente.
- OTDR Inside no afirma certificación independiente de conformidad con Telcordia SR-4731.
- La semántica no soportada permanece explícitamente sin resolver en lugar de recibir valores especulativos.

<h2 id="hoja-de-ruta" align="center">Hoja de ruta</h2>

- mejorar la detección de eventos y sus criterios de confianza;
- comparar longitudes de onda emparejadas y comportamiento entre múltiples trazas;
- añadir procesamiento por lotes, detección de duplicados y revisión de anomalías;
- ampliar el soporte semántico para nuevos perfiles de fabricante;
- construir una matriz de compatibilidad basada en evidencia de validación reproducible.

<h2 id="autor" align="center">Autor</h2>

<p align="center">
  <strong>Esteban Erazo</strong><br>
  Ingeniería Mecatrónica · Universidad Nacional de Colombia<br>
  <a href="https://github.com/EstebanErazo500">@EstebanErazo500</a>
</p>
