
# titulo

## 1.  Introducción

Este repositorio contiene un proyecto completo de análisis y modelado predictivo aplicado al rendimiento académico de un grupo de estudiantes, con el objetivo de identificar los factores más relevantes asociados a la obtención de resultados positivios, y así poder desarrollar dos modelos que sean capaces, de predecir la calificación final y clasificar si el estudiante aprueba o suspende. 

El conjunto de datos incluye variables relacionadas con hábitos, contexto del alumno y varias características vinculadas a la forma de estudio. Las variables objetivo del proyecto son `nota_final` para el ejercicio de regresión y `aprobado` para el ejercicio de clasificación, donde **aprobado** toma el valor de **1** si la nota es mayor o igual a 60 y **suspenso** donde toma el valor de **0** cuando la nota es inferior a 60 .

El proyecto se desarrolla de forma estructurada siguiendo un flujo de trabajo profesional. En primer lugar, se realiza un análisis exploratorio de datos para comprender la estructura del dataset, revisar la calidad del dato y estudiar el comportamiento de las variables mediante análisis univariante y bivariante, incluyendo visualizaciones y relaciones con las variables objetivo. 

A continuación, se realiza una fase de preprocesamiento basada en **Pipelines**, donde se define el tratamiento de valores nulos, el escalado de variables numéricas y la codificación de variables categóricas, preparando el dataset para el entrenamiento de modelos sin que se prodizca fuga de información.

Posteriormente, se aborda el modelado en dos bloques. Por un lado, se entrena y valida un modelo de **regresión** para predecir `nota_final`, comparando un baseline con modelos lineales y regularizados, y seleccionando el modelo final en base a métricas y validación cruzada. 

Por otro lado, se entrena y valida un modelo de **clasificación** para predecir `aprobado`, evaluando el impacto del desbalance de clases mediante métricas adecuadas y analizando el efecto del ajuste de umbral para controlar el compromiso entre detectar suspensos y minimizar falsos resultados. Finalmente, se guardan los modelos entrenados para su reutilización y se procede a documentar las conclusiones y decisiones tomadas a lo largo del proceso.

## **2. Objetivos del Análisis**

### Objetivo general

Realizar un análisis completo del rendimiento académico de un grupo de estudiantes que permita comprender la estructura y calidad del conjunto de datos, estudiando la distribución y relación de las variables con el resultado obtenido, así como desarrollar modelos predictivos para estimar la `nota_final` y clasificar si a `aprobado`, obteniendo una base sólida y reproducible para la toma de decisiones y evaluación de resultados.

### Objetivos específicos

- ***Análisis preliminar***: Se realiza una revisión inicial del dataset para identificar su estructura, tamaño, tipos de variables, rangos y primeras señales de posibles incoherencias, además de revisar la presencia de valores duplicados o nulos que puedan afectar al análisis posterior.

- ***Limpieza y transformación***: Se prepara el dataset para el modelado garantizando la coherencia y calidad de los datos, revisando los rangos, definiendo el tratamiento de valores nulos y dejando el conjunto de datos en condiciones adecuadas para el análisis y el preprocesamiento posterior.

- ***Análisis exploratorio de datos (EDA)***: Se estudia de forma detallada el comportamiento del dataset mediante un análisis univariante y bivariante, incorporando visualizaciones y medidas descriptivas. Se analiza la relación de las variables predictoras con las variables objetivo `nota_final` y `aprobado`, incluyendo las correlaciones y comparativas por grupos para detectar señales útiles.

- ***Preprocesamiento para modelado***: construir un flujo de preprocesamiento reproducible basado en Pipelines que incluya imputación, escalado de variables numéricas y codificación de variables categóricas, asegurando que el ajuste se realiza únicamente con el conjunto de entrenamiento para evitar fuga de información.

- ***Modelado de regresión***: Se entrenan, validan y comparan los modelos para predecir la varibale `nota_final`, estableciendo un baseline, evaluando modelos lineales y regularizados, y seleccionando el modelo final según métricas y validación cruzada.

- ***Modelado de clasificación***: Se entrenan, validan y comparan los modelos para predecir el `aprobado`, considerando el desbalance de clases mediante métricas adecuadas, además se evalua el comportamiento del modelo con curvas ROC/PR, matriz de confusión y, si es necesario, se ajuste eñ umbral para analizar el compromiso entre detectar suspensos y minimizar falsas alarmas.

## 3. Estructura del Proyecto

```
|------ data # Carpeta que contiene los datos utilizados en el proyecto.
  |---- 1.raw # Datos originales sin procesar, tal y como se proporcionan para el análisis.
    |--- dataset_estudiantes.csv # Dataset base con información del grupo de estudiantes.
|------ models # Modelos entrenados y guardados para reutilización y despliegue.
    |--- 0.1_modelo_regresion_ridge.pkl # Modelo final de regresión Ridge para predecir `nota_final`, incluye el Pipeline de preprocesamiento.
    |--- 0.2_modelo_clasificacion_logreg.pkl # Modelo final de clasificación con Regresión Logística para predecir `aprobado`, incluye el Pipeline.
    |--- 0.3_modelo_clasificacion_logreg_threshold.pkl # Versión del modelo de clasificación que guarda modelo y umbral final seleccionado.
|------ notebook # Notebooks con el desarrollo del análisis.
    |--- 0.1_EDA.ipynb # Análisis exploratorio del dataset, calidad del dato, visualizaciones y relaciones con los objetivos.
    |--- 0.2_Preprocesamiento.ipynb # Definición de variables, Pipelines de transformación y preparación de train/test para regresión y clasificación.
    |--- 0.3_Entrenamiento_y_validacion_modelo_regresion.ipynb # Entrenamiento, validación y selección del modelo final de regresión, con métricas y gráficos.
    |--- 0.4_Entrenamiento_y_validacion_modelo_clasificacion.ipynb # Entrenamiento, validación y selección del modelo final de clasificación, incluyendo métricas, curvas y ajuste de umbral.
|------ README.md # Documento principal con la descripción del proyecto, estructura, ejecución y resultados principales.
|------ requirements.txt # Listado de dependencias necesarias para instalar el entorno y reproducir el proyecto.    
```

## 4. Descripción del Conjunto de Datos

El conjunto de datos `dataset_estudiantes.csv` recoge información del rendimiento académico de un grupo de estudiantes junto con variables relacionadas con sus hábitos y entorno de estudio, con el objetivo de analizar qué factores se asocian con el desempeño y apoyar la construcción de modelos predictivos. A continuación se describen las variables incluidas en el análisis exploratorio.

- **horas_estudio_semanal**: Número de horas de estudio a la semana.

- **nota_anterior**: Nota que obtuvo el alumno en la convocatoria anterior.

- **tasa_asistencia**: Tasa de asistencia a clase en porcentaje.

- **horas_sueno**: Promedio de horas que duerme el alumno al día.

- **edad**: Edad del alumno.

- **nivel_dificultad**: Dificultad del alumno para el estudio.

- **tiene_tutor**: Indica si el alumno tiene tutor o no.

- **horario_estudio_preferido**: Horario de estudio preferido por el alumno.

- **estilo_aprendizaje**: Forma de estudio que emplea el alumno.

#### **Variables objetivo**

- Para regresión: **nota_final** (variable continua entre 0 y 100)

- Para clasificación: **aprobado** (variable binaria: 1 si la nota es ≥ 60, y 0 en caso contrario)

## 5. Instalación, requisitos y reproducción del proyecto

#### **5.1 Requisitos**

- Versión de Python recomendada: 3.12

- Entorno probado en: Windows

- Gestor de paquetes: pip

- Ejecución del proyecto: Jupyter Notebook

#### **5.2 Dependencias principales**

- Pandas y NumPy: carga, manipulación, limpieza y transformación de datos.

- Matplotlib y Seaborn: visualizaciones descriptivas del EDA (distribuciones, boxplots, comparativas).

- scikit-learn: preprocesamiento con Pipelines, entrenamiento y validación de modelos, métricas y validación cruzada.

- joblib: guardado y carga de modelos entrenados.

#### **5.3 Instalación**

```
pip install -r requirements.txt
```
#### **5.4 Reproducir el proyecto**

**1. Datos originales**

- `data/1.raw/dataset_estudiantes.csv`

**2. Ejecuta los notebooks en este orden**

1. `notebooks/01_EDA.ipynb`

- Análisis preliminar, calidad del dato y EDA univariante y bivariante con respecto a `nota_final` y `aprobado`.

2. `notebooks/02_Preprocesamiento.ipynb`

- Definición de variables objetivo y predictoras, creación de Pipelines de transformación, imputación, escalado y one-hot encoding y preparación del split de train y test para regresión y clasificación.

3. `notebooks/03_Regresion.ipynb`

- Entrenamiento, validación y comparación de modelos de regresión para `nota_final`. Selección y guardado del modelo final.

4. `notebooks/04_Clasificacion.ipynb`

- Entrenamiento, validación y comparación de modelos de clasificación para `aprobado`, incluyendo matriz de confusión, curvas ROC/PR y ajuste de umbral. Selección y guardado del modelo final.

**3. Archivos generados**

Como resultado del proceso se generan:

- `models/modelo_regresion_ridge.pkl`

- `models/modelo_clasificacion_logreg.pkl`

- `models/modelo_clasificacion_logreg_threshold.pkl`

## 6. Recap Sesiones

**Sesión 1**

- Creación del repositorio en GitHub.

- Definición y creación de la estructura de carpetas del proyecto.

- Creación del archivo **README.md** como base de documentación del repositorio.

- Incorporación del dataset original `dataset_estudiantes.csv` en la carpeta **data/1.raw/**.

**Sesión 2**

- Creación del notebook `'0.1_EDA.ipynb'`.

- Carga del dataset y revisión inicial de estructura: dimensiones, columnas y tipos de dato.

- Evaluación de calidad del dato: duplicados, valores nulos y visualización de nulos.

- Análisis univariante: variables numéricas y categóricas con estadísticos y gráficos.

- Análisis bivariante: relación de variables con `nota_final` y `aprobado` mediante boxplots, scatterplots y barplots.

- Análisis de correlación: matriz de correlación y heatmap para variables numéricas.

- Conclusiones del EDA: variables más relevantes y decisiones preliminares para el preprocesamiento.

**Sesión 3**

- Creación del notebook `'0.2_Preprocesamiento_ipynb'`.

- Definición de objetivos `nota_final` y `aprobado` y separación de predictores evitando fuga de información.

- Identificación de columnas numéricas y categóricas.

- Construcción del pipeline numérico: imputación por mediana y escalado con **StandardScaler**.

- Construcción del pipeline categórico: imputación con valor constante y **OneHotEncoder**.

- Integración con **ColumnTransformer** para aplicar cada transformación a sus columnas.

- Preparación del split train/test para regresión y clasificación, con estratificación en clasificación.

- Verificación final: shapes de train/test y comprobación de proporciones de clase en `aprobado`.

**Sesión 4**

- Creación del notebook `0.3_Entrenamiento_y_validacion_modelo_regresion.ipynb`.

- Redefinición de variables, preprocesamiento y split para ejecutar el notebook de forma independiente.

- Creación de función **eval_regression** para calcular MAE, RMSE y R².

- Modelo baseline: entrenamiento y evaluación con **DummyRegressor**.

- Modelo de regresión lineal: **LinearRegression** con pipeline completo y comparación train/test.

- Validación cruzada: evaluación de **LinearRegression** con KFold y métricas medias.

- Ajuste de hiperparámetros: **Ridge** más **GridSearchCV** para seleccionar el mejor **alpha**.

- Evaluación final en test del mejor **Ridge** y selección del modelo final.

- Visualizaciones de diagnóstico: real vs predicho y residuos.

- Guardado del modelo final en **models/** como `0.1_modelo_regresion_ridge.pkl`.

**Sesión 5**

- Creación del notebook `0.4_Entrenamiento_y_validacion_modelo_clasificacion.ipynb`.

- Redefinición de variables, preprocesamiento y split para clasificación.

- Creación de función **eval_classification** para accuracy, precision, recall, F1 y ROC-AUC.

- Modelo baseline: entrenamiento y evaluación con **DummyClassifier**.

- Modelo principal: **LogisticRegression** con pipeline y **class_weight="balanced"**.

- Validación cruzada: **StratifiedKFold** con métricas medias **incluyendo ROC-AUC**.

- Ajuste de hiperparámetros: **GridSearchCV** para seleccionar **C** y comparación L1 vs L2.

- Evaluación del modelo final: matriz de confusión, classification report y curva ROC/PR.

- Ajuste de umbral: tabla de métricas por umbral para clase 0 y 1 y selección del umbral final.

- Evaluación final con el umbral elegido y documentación del compromiso entre métricas.

- Guardado del modelo final de clasificación en **models/** como `0.2_modelo_clasificacion_logreg.pkl`.

- Guardado de la versión del modelo con umbral final en **models/** como `0.3_modelo_clasificacion_logreg_threshold.pkl`.

**Sesión 6**

- Creación del archivo **requirements.txt** con las dependencias necesarias para reproducir el proyecto.

- Redacción final del **README.md**, documentando objetivos, estructura del proyecto, dependencias, ejecución y resultados principales.

- Inclusión del apartado de instalación y reproducción, indicando el orden de ejecución de los notebooks y los archivos generados.

- Revisión final de formato y coherencia del repositorio para la entrega en GitHub.

## 7. Resultados y Conclusiones



## 8. Contribuciones

Cualquier contribucion es bien venida, si quiere colaborar en el proyecto, abre un pull request.

## 9 Autores

Carlos Hernando

https://github.com/C4rl0s1515