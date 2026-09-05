# CDD-5K3-G3
# 🏎️ Ciencia de Datos: Fórmula 1

**Trabajo Práctico Integrador — Ciencia de Datos**
Ingeniería en Sistemas de Información · UTN — Facultad Regional Córdoba · 2026

Predicción del tiempo de vuelta en Fórmula 1 (temporadas 2021–2023) a partir de condiciones de neumáticos, clima y posición en carrera, aplicando técnicas de aprendizaje automático bajo un enfoque **Data-Driven Scrum**.

---

## 📋 Descripción del proyecto

El proyecto utiliza el **F1 Racing Dataset** (publicado en Kaggle), que ofrece una vista a nivel de vuelta de las carreras de Fórmula 1 correspondientes a las temporadas **2021, 2022 y 2023**.

El objetivo es estudiar los factores que inciden en el rendimiento de un piloto vuelta a vuelta —uso y desgaste de neumáticos, condiciones climáticas, posición en pista— y evaluar la factibilidad de **predecir el tiempo de vuelta (en milisegundos)** mediante modelos predictivos.

### Fuentes de integración

El dataset resulta de combinar tres fuentes distintas:

| Fuente | Aporte |
|--------|--------|
| **Ergast API** | Base de datos histórica de F1: resultados de carrera, pilotos, escuderías y circuitos. |
| **FastF1** | Librería de Python con telemetría oficial: compuestos de neumáticos, stints y tiempos por vuelta. |
| **FastF1 Weather API** | Variables meteorológicas registradas durante cada sesión: temperatura, humedad, lluvia y viento. |

### Tamaño y cobertura

- **69.230** registros y **29** variables
- **3** temporadas (2021–2023)
- **27** circuitos distintos
- **29** pilotos y **10** escuderías

---

## 🎯 Variable objetivo

`milliseconds` — tiempo de vuelta en milisegundos. Es el **target** del modelo predictivo.

---

## 📊 Diccionario de variables

### Contexto de carrera

| Variable | Tipo | Descripción |
|----------|------|-------------|
| `raceId` | int64 | Identificador único de la carrera (clave Ergast que combina temporada y circuito). |
| `year` | int64 | Año / temporada de la carrera (2021, 2022 o 2023). |
| `round` | int64 | Número de fecha (ronda) dentro del calendario de esa temporada (1 a 22). |
| `circuitId` | int64 | Identificador único del circuito (Ergast). |
| `name` | string | Nombre del circuito donde se disputa la carrera (27 circuitos distintos). |
| `lap` | int64 | Número de vuelta dentro de la carrera. |
| `LapNumber` | int64 | Número de vuelta según FastF1. |
| `time` | string | Tiempo de vuelta en formato texto `mm:ss.mmm`. |
| `milliseconds` | int64 | **Tiempo de vuelta en milisegundos. Variable objetivo (target).** |

### Piloto, escudería y resultado

| Variable | Tipo | Descripción |
|----------|------|-------------|
| `driverId` | int64 | Identificador único del piloto (Ergast). |
| `code` | string | Código de 3 letras del piloto (por ej. VER, HAM). |
| `Driver` | string | Código de 3 letras del piloto según FastF1. |
| `constructorId` | int64 | Identificador de la escudería / constructor (10 equipos distintos). |
| `grid` | string | Posición de largada en la parrilla de salida. |
| `position_x` | int64 | Posición del piloto al completar esa vuelta (orden de carrera, 1 a 20). |
| `position_y` | float64 | Posición final del piloto al término de la carrera. |
| `statusId` | int64 | Código numérico Ergast del estado final del piloto en la carrera. |
| `status` | string | Descripción textual del estado final del piloto en la carrera. |

### Neumáticos

| Variable | Tipo | Descripción |
|----------|------|-------------|
| `Compound` | string | Compuesto de neumático utilizado (SOFT, MEDIUM, HARD, etc.). |
| `TyreLife` | float64 | Antigüedad del neumático, en vueltas recorridas (1 a 69). |
| `FreshTyre` | bool | Indica si el neumático era nuevo (True) o usado (False) al inicio del stint. |
| `Stint` | float64 | Número de tramo (stint) con un mismo juego de neumáticos (1 a 8). |

### Variables meteorológicas

| Variable | Tipo | Descripción |
|----------|------|-------------|
| `AirTemp` | float64 | Temperatura del aire durante la vuelta, en grados Celsius. |
| `TrackTemp` | float64 | Temperatura de la superficie de la pista, en grados Celsius. |
| `Humidity` | float64 | Humedad relativa del ambiente, en porcentaje (%). |
| `Pressure` | float64 | Presión atmosférica, en hectopascales (hPa). |
| `Rainfall` | float64 | Indicador binario de lluvia durante la vuelta (0 = no, 1 = sí). |
| `WindSpeed` | float64 | Velocidad del viento, en m/s. |
| `WindDirection` | float64 | Dirección del viento, en grados (0° a 360°). |

---

## 🔍 Líneas de análisis propuestas

- **Predicción del tiempo de vuelta** — estimar `milliseconds` en función de neumáticos, clima y posición.
- **Análisis de estrategia de neumáticos** — impacto del compuesto (`Compound`) y del desgaste (`TyreLife`) sobre el ritmo de carrera, e identificación de la ventana óptima de cambio de neumáticos.
- **Influencia de las condiciones climáticas** — efecto de la temperatura de pista, la humedad y la lluvia sobre la degradación del neumático y el tiempo de vuelta.
- **Comparación de rendimiento entre pilotos y escuderías** — consistencia de ritmo de vuelta, evolución dentro de una carrera y entre temporadas.
- **Detección de vueltas atípicas** — identificación de outliers vinculados a incidentes de carrera (banderas amarillas, safety car, accidentes).
- **Análisis por circuito** — comparación del comportamiento del tiempo de vuelta y de la estrategia de neumáticos entre los 27 circuitos del dataset.

---

## 🗂️ Metodología: Data-Driven Scrum (DDS)

El proyecto adopta un enfoque ágil de Ciencia de Datos basado en capacidades iterativas y estimación de alto nivel (**T-Shirt sizing**: S, M, L), apropiado para proyectos donde es difícil predecir con precisión cuánto requerirá una investigación exploratoria.

### Roles del equipo

| Rol | Integrante |
|-----|-----------|
| **Product Owner (PO)** | Cimatti, Benjamin |
| **Scrum Master** | Steffolani, Nicolas |
| **Developers** | Canaan, Abigail Sara · Morales Demaria, Lucio · Pasolli, Lucas · Tejeda, Emanuel Jesus · Testa, Octavio |

### Backlog del proyecto

| ID | Ítem | Descripción | Estimación |
|----|------|-------------|:----------:|
| DDS-01 | Comprensión del dataset | Analizar estructura, variables y granularidad | S |
| DDS-02 | Calidad de datos | Detectar nulos, duplicados e inconsistencias | S |
| DDS-03 | Análisis del tiempo de vuelta | Estudiar distribución y valores atípicos | M |
| DDS-04 | Análisis de neumáticos | Estudiar Compound, TyreLife, Stint y FreshTyre | M |
| DDS-05 | Análisis meteorológico | Analizar influencia del clima | M |
| DDS-06 | Análisis de variables | Determinar variables potencialmente relevantes | M |
| DDS-07 | Preparación del dataset | Transformar variables para modelado | M |
| DDS-08 | Modelo baseline | Construir primera aproximación predictiva | M |
| DDS-09 | Experimentación | Comparar diferentes modelos | L |
| DDS-10 | Evaluación | Comparar métricas y errores | M |
| DDS-11 | Interpretación | Analizar resultados obtenidos | M |
| DDS-12 | Presentación | Elaborar informe y exposición | S |

### Plan de iteraciones

| # | Descripción | Ítems del backlog | Entrega |
|:-:|-------------|-------------------|---------|
| 1 | Comprender | DDS-01 | 1ra Entrega |
| 2 | Calidad | DDS-02 | 2da Entrega |
| 3 | Rendimiento | DDS-03, DDS-06 | 2da Entrega |
| 4 | Neumáticos | DDS-04 | 2da Entrega |
| 5 | Clima | DDS-05 | 2da Entrega |
| 6 | Preparación del dataset | DDS-07 | 2da Entrega |
| 7 | Modelado | DDS-08, DDS-09 | 3ra Entrega |
| 8 | Evaluación e interpretación | DDS-10, DDS-11 | 3ra Entrega |
| 9 | Presentación final | DDS-12 | 4ta Entrega |

---

## 📁 Estructura del repositorio

```
.
├── README.md
├── docs/
│   ├── Informe_F1_CDD_-_Grupo_3.pdf          # Informe de la Entrega 1
│   └── F1_Data_Science_Presentation.pdf      # Presentación de la Entrega 1
├── data/
│   ├── raw/                                  # Dataset original (F1 Racing Dataset)
│   └── processed/                            # Dataset transformado para modelado
├── notebooks/                                # Jupyter notebooks por iteración
└── src/                                      # Código fuente / utilidades
```

> **Nota:** los PDFs de la Entrega 1 (informe y presentación) van en la carpeta `docs/`.

---

## 👥 Equipo — Grupo 3

- Canaan, Abigail Sara — 85860
- Cimatti, Benjamin — 94312
- Morales Demaria, Lucio — 94289
- Pasolli, Lucas — 94250
- Steffolani, Nicolas — 94196
- Tejeda, Emanuel Jesus — 401000
- Testa, Octavio — 94177

**Docentes:** Gualpa, Mariano Martín · Calle, Manuel Rodrigo · Rustan, Silvina
**Curso:** 5K3 · **Año:** 2026
