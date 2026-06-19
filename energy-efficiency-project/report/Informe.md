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


