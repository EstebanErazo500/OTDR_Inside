<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/otdr-hero.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/otdr-hero-light.svg">
    <img src="assets/otdr-hero-light.svg" alt="OTDR Inside" width="100%">
  </picture>
</p>

<h1 align="center">OTDR Inside</h1>

<p align="center">
  <strong>Lee la traza. Sigue la evidencia.</strong><br>
  Herramienta local de análisis SOR que separa lo que la traza almacena, lo que puede interpretarse y lo que se calcula a partir de ella.
</p>

<p align="center">
  <a href="README.md">English</a> · <strong>Español</strong>
</p>

---

## ¿Qué es OTDR Inside?

Graficar una curva OTDR es sencillo. **Saber qué información puede considerarse realmente confiable no lo es.**

Los archivos SOR reales mezclan estructuras estándar con extensiones propietarias, metadatos reescritos, escalas ambiguas y distintas representaciones de eventos. OTDR Inside aborda ese problema desde la evidencia: lee la estructura de forma segura, reconstruye la traza, aplica reglas específicas de perfil solo cuando están respaldadas y conserva el origen y la confianza de cada interpretación.

> **Desconocido es un resultado válido.** El analizador no debe convertir incertidumbre en un número solo porque exista un campo.

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/architecture-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/architecture-light.svg">
    <img src="assets/architecture-light.svg" alt="Cadena de análisis de OTDR Inside" width="92%">
  </picture>
</p>

## Qué lo diferencia

| Principio | En la práctica |
|---|---|
| **Lectura binaria segura** | Se verifican bloques, tamaños y offsets antes de confiar en el archivo. |
| **Interpretación trazable** | Valor crudo, valor normalizado, regla, fuente y confianza permanecen vinculados. |
| **Procedencia de eventos** | Los eventos almacenados y los candidatos calculados nunca se presentan como equivalentes. |
| **Local y de solo lectura** | Las trazas operativas se analizan localmente sobre copias temporales; los originales no se sobrescriben. |

El proyecto mantiene visibles varias distinciones fundamentales:

<p align="center">
  <code>estructura ≠ semántica</code> ·
  <code>equipo ≠ escritor del archivo</code> ·
  <code>almacenado ≠ calculado</code> ·
  <code>desconocido ≠ corrupto</code>
</p>

## Alcance actual · v0.3.x

| Ecosistema | Estado | Papel actual |
|---|---|---|
| **EXFO** | **Referencia validada** | Lectura SOR 2.00, parámetros normalizados, eventos almacenados y visualización de trazas. |
| **Ceyear CE6422** | **Desarrollo activo** | Interpretación de curva y detección de eventos calculados cuando no existe `KeyEvents`. |
| **Yokogawa AQ1000** | **Estructural** | Estructura caracterizada; la normalización semántica específica del fabricante sigue pendiente. |

El soporte se entiende como una progresión y no como una etiqueta binaria: **legible → identificado → caracterizado → validado**.

## Análisis de eventos sin ocultar su origen

La línea actual hace explícita la procedencia de cada evento. Si el SOR contiene `KeyEvents`, esos eventos permanecen como **almacenados**. Si la curva se analiza para proponer eventos adicionales, permanecen como **candidatos calculados**.

Esto es especialmente importante en archivos Ceyear: que no exista `KeyEvents` significa *“no hay tabla de eventos almacenada”*, no *“no existen eventos”.*

## Validación

El proyecto combina pruebas sintéticas o anonimizadas con regresión sobre trazas privadas de referencia:

**pruebas de límites → regresión de perfiles → reconstrucción de traza → comparación con herramientas de referencia → validación manual del visor**

Las trazas operativas empleadas durante la validación permanecen fuera del repositorio público.

## Hoja de ruta

- Mejorar la detección de eventos y sus criterios de confianza.
- Comparar longitudes de onda emparejadas y comportamiento entre trazas.
- Añadir análisis por lotes, detección de duplicados y revisión de anomalías.
- Ampliar la matriz de compatibilidad únicamente cuando nuevas interpretaciones estén validadas de forma reproducible.

<details>
<summary><strong>Salvaguardas metodológicas</strong></summary>

- Las estructuras incompletas o malformadas producen diagnósticos, no corrupción silenciosa.
- Los bloques desconocidos o propietarios se conservan en lugar de descartarse automáticamente.
- Una sola cadena de fabricante o marcador propietario no basta para establecer procedencia.
- OTDR Inside no afirma compatibilidad universal con SOR ni certificación independiente de conformidad con Telcordia SR-4731.

</details>

<details>
<summary><strong>Política de datos del repositorio</strong></summary>

Este repositorio público no distribuye mediciones operativas reales `.sor`, `.ei` u `.otdr`, identificadores de clientes o rutas, ejecutables propietarios de fabricantes, manuales comerciales, estándares licenciados ni archivos derivados que expongan metadatos confidenciales. Los ejemplos y pruebas públicas deben utilizar datos sintéticos o explícitamente anonimizados.

</details>

## Autor

**Esteban Erazo**  
Ingeniería Mecatrónica · Universidad Nacional de Colombia  
GitHub: [@EstebanErazo500](https://github.com/EstebanErazo500)

<p align="center">
  <sub>Cuando el archivo es ambiguo, el software debe decirlo.</sub>
</p>
