# Sistema de Predicción de Precios de Pasajes Aéreos

Sistema de Machine Learning para predecir precios de vuelos basado en características históricas, utilizando datos de aerolíneas indias.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.0+-orange.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

## Tabla de Contenidos
- [Descripción del Proyecto](#descripción-del-proyecto)
- [Problema de Negocio](#problema-de-negocio)
- [Conjunto de Datos](#conjunto-de-datos)
- [Metodología](#metodología)
- [Resultados](#resultados)
- [Instalación](#instalación)
- [Uso](#uso)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Tecnologías Utilizadas](#tecnologías-utilizadas)
- [Autor](#autor)

## Descripción del Proyecto

Este proyecto implementa un sistema de predicción de precios de pasajes aéreos utilizando técnicas de Machine Learning. El objetivo es ayudar tanto a aerolíneas como a usuarios finales a:

- **Aerolíneas**: Optimizar estrategias de precios dinámicos
- **Usuarios**: Identificar los mejores momentos para comprar pasajes
- **Plataformas**: Maximizar ingresos mediante identificación de patrones de precios

## Problema de Negocio

### Objetivo Principal
Predecir con precisión el precio final de un boleto de avión basándose en características históricas de vuelos.

### Valor Agregado
- Reducción del error de predicción en 15-20%
- Recomendaciones de mejores oportunidades de precio
- Incremento potencial en reservas de pasajes

## Conjunto de Datos

### Características del Dataset
- **Registros totales**: ~300,000 vuelos
  - Business Class: 93,487 registros
  - Economy Class: 206,774 registros
- **Período**: Febrero - Marzo 2022
- **Rutas**: Principales ciudades de India (Delhi, Mumbai, Bangalore, Kolkata, Hyderabad, Chennai)

### Variables Principales
| Variable | Descripción | Tipo |
|----------|-------------|------|
| `airline` | Aerolínea (Air India, Vistara, Indigo, etc.) | Categórica |
| `date` | Fecha del vuelo | Temporal |
| `dep_time` | Hora de salida | Temporal |
| `arr_time` | Hora de llegada | Temporal |
| `time_taken` | Duración del vuelo | Numérica |
| `stop` | Número de escalas | Categórica |
| `from` / `to` | Origen y destino | Categórica |
| `price` | **Variable objetivo** - Precio del pasaje | Numérica |

## Metodología

### Framework: CRISP-DM
El proyecto sigue la metodología estándar de la industria para proyectos de Data Mining:

```
1. Comprensión del Negocio
2. Comprensión de los Datos
3. Preparación de Datos
4. Modelado
5. Evaluación
6. (Implementación - fuera del alcance actual)
```

### Pipeline de Procesamiento

#### 1. Análisis Exploratorio de Datos (EDA)
- Análisis de distribuciones de precios
- Identificación de outliers
- Análisis de correlaciones
- Visualizaciones comparativas entre clases Business y Economy

#### 2. Preprocesamiento
- **Limpieza**: Eliminación de 108 valores nulos en variable `price`
- **Transformación temporal**:
  - Extracción de día y mes de `date`
  - Extracción de hora y minutos de `dep_time` y `arr_time`
  - Conversión de duración (`time_taken`) a horas y minutos
- **Codificación**: 
  - Label Encoding para variable `stop`
  - One-Hot Encoding para variables categóricas (aerolínea, origen, destino)
- **Escalado**: StandardScaler para variables numéricas

#### 3. Feature Engineering
Variables generadas:
- `date_day`, `date_month`
- `dep_time_hour`, `dep_time_min`
- `arr_time_hour`, `arr_time_min`
- `duration_hours`, `duration_mins`

#### 4. Modelado

**Modelos implementados:**

| Modelo | Hiperparámetros Optimizados |
|--------|----------------------------|
| Decision Tree | `max_depth`, `max_features` |
| Random Forest | `n_estimators`, `max_depth`, `max_features` |
| Linear Regression | `fit_intercept`, `positive` |

**Técnica de optimización**: GridSearchCV con validación cruzada (5-folds)

**División de datos**: 80% entrenamiento / 20% prueba

## Resultados

### Métricas de Evaluación

#### Dataset Business Class
| Modelo | R² | MAE | RMSE |
|--------|-----|-----|------|
| Decision Tree (baseline) | 0.857 | $2,604 | $4,843 |
| **Random Forest (optimizado)** | **0.858** | **$2,625** | **$4,831**  |
| Linear Regression | 0.473 | $7,085 | $9,336 |

#### Dataset Economy Class
| Modelo | R² | MAE | RMSE |
|--------|-----|-----|------|
| Decision Tree (baseline) | 0.694 | $1,073 | $2,059 |
| **Random Forest (optimizado)** | **0.708** | **$1,086** | **$2,013**  |
| Linear Regression | 0.492 | $1,777 | $2,655 |

### Hallazgos Principales

**Random Forest es el mejor modelo** en ambas clases, con diferencias mínimas respecto a Decision Tree simple

**Business Class**: Mayor capacidad predictiva (R²=0.858) pero errores absolutos más altos debido a mayor rango de precios

**Economy Class**: Menor R² (0.708) pero errores más bajos, sugiere mayor volatilidad de precios

**Linear Regression tiene bajo desempeño**, confirmando relaciones no lineales entre variables y precio

**Variables más influyentes**: Duración del vuelo, número de escalas, aerolínea

### Visualizaciones Clave

El proyecto incluye:
- Distribuciones de precios por clase
- Boxplots comparativos por aerolínea, origen, destino
- Matrices de correlación
- Gráficos de comparación de métricas entre modelos


## Librerías Utilizadas

- **Python 3.12**: Lenguaje principal
- **Pandas**: Manipulación y análisis de datos
- **NumPy**: Computación numérica
- **Scikit-learn**: Machine Learning (modelos, preprocessing, métricas)
- **Matplotlib & Seaborn**: Visualización de datos
- **Jupyter Notebook**: Desarrollo interactivo

## Conclusiones

### Fortalezas del Proyecto
- Pipeline completo de Data Science implementado
- Comparación rigurosa de múltiples modelos
- Optimización de hiperparámetros con validación cruzada
- Métricas de evaluación comprehensivas

### Limitaciones Identificadas
- Dataset limitado a 2 meses (febrero-marzo 2022)
- Solo rutas indias
- Posible sesgo estacional (datos de temporada baja)

### Trabajo Futuro
- [ ] Incorporar datos de todo el año para capturar estacionalidad
- [ ] Incluir variables externas (precio del combustible, demanda turística)
- [ ] Implementar modelos de ensemble más sofisticados (XGBoost, LightGBM)
- [ ] Análisis de series temporales para tendencias de precios

## Autor

**Gastón Esteban González Ovalle**

- Email: ggo@hotmail.cl
- LinkedIn: [Gastón González Ovalle](https://www.linkedin.com/in/gastón-gonzález-ovalle-5290a179/)
- GitHub: [@tu-usuario](https://github.com/tu-usuario)

---

## Agradecimientos

- Dataset original disponible en [Kaggle](https://www.kaggle.com/)
- Inspirado en necesidades reales de optimización de precios en la industria aérea
- Desarrollado como proyecto práctico durante formación en Data Science en Academia Desafío Latam
