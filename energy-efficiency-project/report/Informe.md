# Informe

## 1. Descripción del problema

El objetivo de este proyecto consiste en analizar la relación entre las características arquitectónicas de edificios residenciales y su comportamiento energético.

A partir de variables como compacidad relativa, área de superficie, área de pared, área de techo, altura total, orientación y características de acristalamiento, se busca estudiar la influencia de estas características sobre dos variables objetivo:

* Y1: Heating Load (Carga de calefacción)
* Y2: Cooling Load (Carga de refrigeración)

El propósito de esta fase es preparar el conjunto de datos para su utilización para las Fases 2 y 3.

---

## 2. Descripción del dataset

Se utilizó el dataset Energy Efficiency, disponible en el repositorio UCI Machine Learning Repository.

Características principales:

* Total de registros: 768
* Variables predictoras: 8 (X1 – X8)
* Variables objetivo: 2 (Y1 y Y2)
* Tipo de variables: Numéricas

### Variables predictoras

| Variable | Descripción                      |
| -------- | --------------------------------- |
| X1       | Compacidad relativa               |
| X2       | Área de superficie               |
| X3       | Área de pared                    |
| X4       | Área de techo                    |
| X5       | Altura total                      |
| X6       | Orientación                      |
| X7       | Área de acristalamiento          |
| X8       | Distribución del acristalamiento |

### Variables objetivo

| Variable | Descripción            |
| -------- | ----------------------- |
| Y1       | Carga de calefacción   |
| Y2       | Carga de refrigeración |

---

## 3. Metodología de preprocesamiento

### 3.1 Selección de datos

Se seleccionaron las variables X1–X8 como variables de entrada para el modelado.

Las variables Y1 y Y2 se conservaron como variables objetivo debido a que representan los valores que posteriormente se desean predecir.

---

### 3.2 Análisis exploratorio de datos

Se hace uso de la función describe para hacer una exploración del conjunto de datos mediante estadísticas descriptivas y análisis de correlación.

Donde se obtuvieron los siguiente resultados:

* La altura total (X5) influye de manera fuerte con las variables objetivo.
* La compacidad relativa (X1) y el área de pared (X3) también muestran asociaciones importantes con las cargas energéticas.
* La orientación (X6) su inlfuencia es relativamente baja sobre las variables de salida.

---

### 3.3 Identificación de problemas de calidad

Se revisaron diferentes aspectos relacionados con la calidad de los datos:

#### Valores faltantes

No se identificaron valores nulos en ninguna de las variables del dataset.

#### Registros duplicados

No se encontraron registros duplicados.

#### Variables inconsistentes

No se identificaron inconsistencias en tipos de datos ni en los valores registrados.

#### Valores atípicos

La revisión estadística no evidenció valores extremos que requirieran tratamiento o eliminación.

---

### 3.4 Limpieza de datos

Debido a que el conjunto de datos no presentó problemas de calidad relevantes, no fue necesario eliminar registros ni realizar correcciones sobre los datos originales.

---


### 3.5 Estandarización de variables

Las variables predictoras presentaban escalas diferentes.

Por ejemplo, algunas variables tomaban valores cercanos a 1, mientras que otras superaban los 800.

Para evitar que las variables con magnitudes mayores influyeran de forma desproporcionada en futuros modelos de aprendizaje automático, se aplicó estandarización mediante StandardScaler.

Como resultado, las variables predictoras quedaron con media cercana a 0 y desviación estándar cercana a 1.

---

### 3.6 División del dataset

Finalmente, el conjunto de datos fue dividido en:

* 80 % para entrenamiento.
* 20 % para pruebas.

Esta división permite entrenar y evaluar los modelos en fases posteriores utilizando datos no vistos durante el entrenamiento.

---

## 4. Fase 2 – Aprendizaje no supervisado (Clustering)

### 4.1 Objetivo

Identificar patrones naturales en los datos mediante clustering, agrupando edificios según sus características arquitectónicas para descubrir segmentos con comportamientos energéticos similares.

### 4.2 Algoritmo utilizado

Se aplicó K-Means, evaluando k desde 2 hasta 6 clusters. La métrica de silueta (silhouette score) determinó que k=2 es el valor óptimo (0.3905).

### 4.3 Resultados del clustering

| Cluster | Muestras | Y1 medio | Y2 medio | Etiqueta |
|---------|----------|----------|----------|----------|
| 0 | 384 | 13.34 | 16.07 | Baja demanda energética |
| 1 | 384 | 31.28 | 33.10 | Alta demanda energética |

Los clusters se distribuyen equitativamente (384 muestras cada uno) y presentan diferencias significativas en las cargas energéticas medias.

---

## 5. Fase 3 – Aprendizaje supervisado (Regresión)

### 5.1 Objetivo

Construir modelos predictivos para estimar la carga de calefacción (Y1) a partir de las 8 variables arquitectónicas.

### 5.2 Modelos evaluados

#### Regresión Lineal Múltiple

| Métrica | Valor |
|---------|-------|
| MAE | 2.22 |
| RMSE | 3.12 |
| R² | 0.9067 |

#### Red Neuronal Artificial (MLPRegressor)

Arquitectura: 2 capas ocultas (64 y 32 neuronas), activación ReLU, optimizador Adam, tasa de aprendizaje 0.001.

| Métrica | Valor |
|---------|-------|
| MAE | 0.4505 |
| RMSE | 0.6039 |
| R² | 0.9965 |

### 5.3 Selección del modelo final

Se seleccionó el MLPRegressor por su rendimiento superior (R²=0.9965 frente a 0.9067 de regresión lineal).

---

## 6. Fase 4 – Desarrollo de la aplicación

### 6.1 Objetivo

Integrar el modelo entrenado (MLPRegressor) en una aplicación funcional que permita realizar predicciones de carga de calefacción (Y1) a partir de nuevas mediciones de características arquitectónicas.

### 6.2 Archivo de la aplicación

| Archivo | Descripción |
|---------|-------------|
| `notebooks/fase4_app.ipynb` | Notebook con la implementación completa de la aplicación |

### 6.3 Funcionalidades

La aplicación (implementada en el notebook) permite:

1. **Ingreso manual de datos**: El usuario introduce las 8 variables arquitectónicas (X1–X8) y la aplicación devuelve la predicción de Heating Load (Y1).
2. **Carga de ejemplo**: Selecciona una muestra aleatoria del dataset original para demostrar el funcionamiento.
3. **Predicción por lotes**: Evalúa todo el dataset y compara las predicciones con los valores reales.
4. **Estandarización automática**: Los valores ingresados se estandarizan automáticamente usando el mismo StandardScaler de la Fase 1, asegurando consistencia.

### 6.4 Funcionamiento interno

El notebook carga los datos originales, ajusta el StandardScaler, entrena el MLPRegressor con la misma configuración de la Fase 3 (64, 32 neuronas, ReLU, Adam), y ejecuta predicciones sobre ejemplos individuales o lotes completos, mostrando el resultado junto con una clasificación del nivel de demanda energética.

### 6.5 Ejemplo de predicción

```
VALORES DE ENTRADA
-------------------------------------------------------
  X1: Compacidad relativa: 0.75
  X2: Área de superficie: 650.0
  X3: Área de pared: 310.0
  X4: Área de techo: 180.0
  X5: Altura total: 3.5
  X6: Orientación: 3
  X7: Área de acristalamiento: 2
  X8: Distribución del acristalamiento: 3

-------------------------------------------------------
RESULTADO DE LA PREDICCIÓN
-------------------------------------------------------
  Heating Load (Y1) estimado: 22.07
  Clasificación: Baja demanda energética
-------------------------------------------------------
```

### 6.6 Resultado esperado

Una aplicación funcional implementada en Jupyter Notebook que utiliza el modelo de Machine Learning (MLPRegressor) entrenado en la Fase 3 para realizar predicciones precisas de la carga de calefacción a partir de nuevas características arquitectónicas, demostrando la aplicabilidad práctica del modelo desarrollado.


