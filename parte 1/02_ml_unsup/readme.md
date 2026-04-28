# Ejercicio: Diagnóstico de Cáncer de Mama mediante Análisis Celular y Clustering

### Descripción del Problema

En oncología, la precisión y rapidez en el diagnóstico patológico son factores determinantes para la supervivencia del paciente. El procedimiento clínico estándar para evaluar una masa mamaria sospechosa implica analizar visualmente la morfología de las células extraídas bajo un microscopio.

**El Problema:** El análisis manual de imágenes citológicas consume mucho tiempo y está sujeto a la variabilidad de interpretación entre profesionales. 

**El Objetivo:** En este ejercicio vamos a crear un flujo de Machine Learning capaz de analizar matemáticamente la geometría de los núcleos celulares. Utilizaremos un **Análisis Exploratorio de Datos (EDA)** profundo y un algoritmo de **Aprendizaje No Supervisado (K-Means)** para descubrir si las características biométricas se agrupan de forma natural en tumores benignos y malignos.

Este enfoque nos permitirá entender la separabilidad de los datos antes de aplicar modelos de clasificación más complejos.

---

### El Reto Matemático y Biológico

Desde la biología, las células tumorales malignas tienden a tener núcleos más grandes, de contornos irregulares y fuertemente asimétricos. 

Desde la matemática, el reto de este ejercicio es evaluar si un algoritmo de agrupamiento basado puramente en distancias geométricas (como K-Means) es capaz de "descubrir" el diagnóstico sin conocer previamente las etiquetas reales. Contrastaremos los clústeres generados por la IA con los diagnósticos reales emitidos por los médicos para evaluar la calidad del modelo mediante métricas de similitud y cohesión.

---

### El Dataset y las Variables

Utilizaremos el **Breast Cancer Wisconsin (Diagnostic) Dataset**, cargado directamente desde la librería `scikit-learn`. El conjunto de datos contiene 569 muestras de pacientes con 30 variables predictoras continuas.

#### Resumen de los Datos
* **Total de muestras:** 569 pacientes.
* **Características (Features):** 30 variables numéricas derivadas de 10 características base del núcleo celular (Radio, Textura, Perímetro, Área, Suavidad, Compacidad, Concavidad, Puntos Cóncavos, Simetría y Dimensión Fractal). Para cada una se calcula la Media (*mean*), el Error Estándar (*error*) y el Peor valor (*worst*).
* **Variable Objetivo (Target):** * **1.0 (Benigno):** 357 casos detectados.
  * **0.0 (Maligno):** 212 casos detectados.

---

### Estructura de la Práctica

#### Parte 1: Análisis Exploratorio de Datos (EDA)
1. **Estadísticas Descriptivas**: Inspección inicial de las medias y desviaciones de las características (ej. el radio medio varía entre 6.98 y 28.11).
2. **Matriz de Correlación (Heatmap)**: Identificación visual de la fuerte multicolinealidad entre variables geométricas (ej. radio, perímetro y área miden el mismo fenómeno biológico).
3. **Distribución de Clases**: Uso de gráficos de barras para entender el balance del dataset (Benignos vs. Malignos).

#### Parte 2: Visualización de Relaciones Biométricas
1. **Análisis de Varianza (ANOVA)**: Comprobación estadística de que variables como el *Mean Radius* separan significativamente las clases (F = 646.98, p = 0.00000).
2. **KDE Plots y Boxplots**: Visualización de cómo la distribución de la concavidad media (*Mean Concavity*) y el tamaño (*Mean Radius*) difieren drásticamente según el tipo de tumor.
3. **Pairplot**: Análisis cruzado de variables clave para observar fronteras de decisión naturales en 2D.

#### Parte 3: Modelado (Clustering con K-Means)
1. **Selección de Características**: Reducción del espacio de trabajo a 2 variables altamente discriminantes: `mean radius` y `mean concavity`.
2. **Agrupamiento (K-Means)**: Entrenamiento del algoritmo con `k=2` (buscando separar en dos grupos) sin proporcionarle las etiquetas de los médicos.
3. **Inferencia**: Clasificación de un nuevo paciente imaginario (ej. Radio 17.0, Concavidad 0.1) para determinar a qué clúster de riesgo pertenece.

#### Parte 4: Evaluación del Rendimiento (Métricas)
Dado que estamos aplicando clustering a un problema con etiquetas conocidas, evaluamos el rendimiento con métricas especializadas:
* **Silhouette Score (0.64):** Indica que los clústeres formados son razonablemente densos y están bien separados entre sí.
* **Adjusted Rand Index - ARI (0.51):** Mide la similitud entre los clústeres descubiertos por K-Means y los diagnósticos reales de los médicos (0 indica azar, 1 indica coincidencia perfecta).
* **Homogeneity (0.41) y Completeness (0.47):** Evalúan si cada clúster contiene únicamente miembros de una sola clase y si todos los miembros de una clase se asignaron al mismo clúster.

---

