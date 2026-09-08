<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero-light.svg">
    <img src="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero-light.svg" alt="OTDR Inside" width="100%">
  </picture>
</p>

# OTDR Inside

**Análisis de trazas OTDR SOR con procedencia explícita.**  
Prototipo local de ingeniería para lectura binaria segura, interpretación multifabricante, reconstrucción de trazas y análisis de eventos sin perder el origen de los datos utilizados.

[English](README.md) · **Español**

---

## Descripción general

Los archivos OTDR SOR están diseñados para almacenar mediciones de reflectometría óptica en el dominio del tiempo, pero en la práctica no todos los archivos son semánticamente uniformes. Extensiones propietarias, metadatos reescritos, escalas ambiguas y distintas representaciones de eventos pueden hacer que un archivo sea estructuralmente legible sin que todos sus valores sean igual de confiables.

OTDR Inside aborda este problema separando **estructura**, **interpretación**, **cálculo** y **confianza**. El objetivo no es forzar cada traza a un modelo universal, sino mostrar qué se conoce, cómo fue obtenido y qué sigue sin resolverse.

## Arquitectura

<p align="center">
  <img src="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/architecture.svg" alt="Cadena de análisis de OTDR Inside" width="94%">
</p>

La cadena de procesamiento se divide deliberadamente por responsabilidades:

| Capa | Responsabilidad |
|---|---|
| **Lectura estructural** | Recorre bloques SOR, revisiones, tamaños, offsets y regiones de muestras con comprobaciones explícitas de límites. |
| **Perfil y normalización** | Aplica reglas específicas de fabricante únicamente cuando la evidencia disponible las respalda. |
| **Análisis de traza** | Reconstruye el eje de distancia y la curva, manteniendo separados los eventos almacenados y los calculados. |
| **Presentación** | Expone parámetros, estructura, procedencia y eventos mediante un visor local y exportaciones JSON/CSV. |

Esta separación evita confundir legibilidad estructural con certeza semántica.

## Enfoque de ingeniería

Cuatro distinciones guían la implementación:

- **La estructura no equivale a la semántica.** Un archivo puede recorrerse de forma segura aunque algunas magnitudes sigan sin interpretarse.
- **El equipo no equivale necesariamente al escritor del archivo.** Un bloque propietario puede identificar un linaje de software sin demostrar qué OTDR adquirió la traza.
- **Almacenado no equivale a calculado.** Los valores o eventos añadidos por software de análisis permanecen diferenciados de la información presente en el SOR original.
- **Desconocido no equivale a corrupto.** Una semántica no soportada debe quedar explícita en lugar de ser adivinada silenciosamente.

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

Así, una conversión empírica, una escala específica de fabricante o una interpretación inferida no se vuelve indistinguible de un valor almacenado explícitamente en el archivo.

## Capacidades actuales

La línea de desarrollo actual integra las siguientes funciones:

- inspección segura de la estructura y metadatos de archivos SOR 2.00;
- selección de perfiles soportados basada en evidencia;
- reconstrucción de la traza a partir de las muestras almacenadas;
- visualización local interactiva de la curva OTDR;
- presentación de parámetros, estructura del archivo y procedencia de las interpretaciones;
- separación entre eventos almacenados y candidatos calculados;
- exportación JSON del modelo de análisis y CSV de información de eventos;
- degradación segura ante variantes parcialmente soportadas sin modificar la traza original.

## Alcance actual · v0.3.x

| Ecosistema | Estado | Papel dentro del proyecto |
|---|---|---|
| **EXFO** | **Referencia validada** | Lectura SOR 2.00, parámetros normalizados, eventos almacenados y visualización de trazas. |
| **Ceyear CE6422** | **Desarrollo activo** | Interpretación de curva y detección de eventos calculados cuando `KeyEvents` no está presente. |
| **Yokogawa AQ1000** | **Estructural** | Estructura caracterizada; la normalización semántica específica del fabricante sigue pendiente. |

El soporte se trata como una progresión y no como una etiqueta binaria:

**estructuralmente legible → perfil identificado → caracterizado semánticamente → validado empíricamente**

## Procedencia de eventos

La gestión de eventos es una de las principales diferencias entre el lector inicial y la cadena de análisis actual.

Una tabla `KeyEvents` se considera **información de eventos almacenada**. Los eventos propuestos a partir de la curva reconstruida se consideran **candidatos calculados** y conservan ese origen dentro del modelo y las exportaciones.

Esta distinción es especialmente importante en archivos Ceyear utilizados durante el desarrollo: que no exista `KeyEvents` significa que el SOR **no contiene una tabla de eventos almacenada**; no demuestra que la traza óptica carezca de eventos.

## Validación

La validación se realiza en varios niveles en lugar de depender de un único criterio de aprobación:

1. comprobaciones estructurales y de límites;
2. pruebas sintéticas o anonimizadas para la regresión pública;
3. regresión de perfiles contra trazas privadas de referencia;
4. comprobaciones de reconstrucción de traza;
5. comparación con software de referencia cuando corresponde;
6. validación manual del visor local.

Las mediciones operativas utilizadas para la validación de ingeniería permanecen fuera del repositorio público.

## Manejo de datos

OTDR Inside está diseñado como un flujo **local y de solo lectura**. Las trazas fuente se analizan a partir de copias temporales y el visor no sobrescribe los archivos originales.

Este repositorio no distribuye mediciones operativas reales `.sor`, `.ei` u `.otdr`, identificadores de clientes o rutas, ejecutables propietarios de fabricantes, manuales comerciales, estándares licenciados ni archivos derivados que expongan metadatos confidenciales. Los ejemplos y pruebas públicas deben emplear datos sintéticos o explícitamente anonimizados.

## Limitaciones conocidas

- El visor actual trabaja con `.SOR`; `.EI` y `.otdr` todavía no forman parte del flujo normal de procesamiento.
- La interpretación específica de fabricante depende de perfiles y no debe entenderse como compatibilidad universal con SOR.
- El nivel vertical mostrado de la traza es relativo y no se presenta como potencia óptica calibrada de forma universal.
- OTDR Inside no afirma certificación independiente de conformidad con Telcordia SR-4731.
- Las semánticas de fabricante no soportadas permanecen explícitamente sin resolver en lugar de recibir valores especulativos.

## Hoja de ruta

- mejorar la detección de eventos y sus criterios de confianza;
- comparar longitudes de onda emparejadas y comportamiento entre múltiples trazas;
- añadir procesamiento por lotes, detección de duplicados y revisión de anomalías;
- ampliar el soporte semántico de nuevos perfiles de fabricante;
- construir una matriz de compatibilidad basada en evidencia reproducible de validación.

## Autor

**Esteban Erazo**  
Ingeniería Mecatrónica · Universidad Nacional de Colombia  
GitHub: [@EstebanErazo500](https://github.com/EstebanErazo500)
