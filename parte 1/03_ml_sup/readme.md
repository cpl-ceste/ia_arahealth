# Ejercicio: Predicción Cuantitativa de la Progresión de la Diabetes (Machine Learning)

### Descripción del Problema

En el ámbito médico, predecir cómo avanzará una enfermedad crónica es fundamental para el diseño de un tratamiento preventivo eficaz. El método tradicional para evaluar el avance de la diabetes implica medir variables clínicas iniciales y luego tener un enfoque reactivo: esperar un año entero para evaluar la progresión real del paciente.

**El Problema:** Esperar un año para saber si la enfermedad avanza agresivamente significa perder una ventana crítica de tiempo donde se podrían aplicar intervenciones preventivas como cambios en la dieta, ejercicio o medicación temprana.

**El Objetivo:** En este ejercicio vamos a crear un modelo de Machine Learning capaz de predecir la "progresión cuantitativa" de la diabetes un año después, basándose únicamente en mediciones basales tomadas en el día 0. Esto permitirá a los profesionales clínicos anticiparse a la enfermedad y personalizar el tratamiento.

El enfoque de esta práctica consiste en explorar los datos clínicos, establecer una línea base mediante modelos tradicionales y escalar hacia modelos más complejos para capturar la biología del paciente a través de un notebook interactivo.

### El Reto Matemático y Biológico

El cuerpo humano es un sistema altamente complejo. La relación entre ciertos niveles en sangre y la progresión de la enfermedad rara vez es una línea recta perfecta; existen interacciones no lineales entre las variables. A lo largo del ejercicio, contrastaremos **modelos lineales** tradicionales con **algoritmos de ensamble** (basados en árboles de decisión) para evaluar qué arquitectura matemática es capaz de capturar mejor esta biología compleja.

---

### El Dataset y las Variables

Utilizaremos el dataset clínico de Diabetes (`load_diabetes` de la librería *scikit-learn*), que contiene datos validados de 442 pacientes.

> **Nota importante sobre los datos:** Las variables predictoras de este conjunto ya se entregan **estandarizadas** (escaladas para tener una media de 0 y una suma de cuadrados igual a 1). Este es un paso común en el Machine Learning médico, ya que permite al algoritmo comparar variables que tienen distintas unidades de medida de forma equitativa.

#### Diccionario de Datos

| Variable | Descripción Clínica |
| :--- | :--- |
| **Age** | Edad del paciente. |
| **Sex** | Sexo del paciente. |
| **BMI** | Índice de Masa Corporal. |
| **BP** | Presión Arterial Media. |
| **S1 / tc** | Células T / Colesterol Total en suero. |
| **S2 / ldl** | Lipoproteínas de Baja Densidad (Colesterol "malo"). |
| **S3 / hdl** | Lipoproteínas de Alta Densidad (Colesterol "bueno"). |
| **S4 / tch** | Ratio: Colesterol total / HDL. |
| **S5 / ltg** | Logaritmo del nivel de triglicéridos en suero. |
| **S6 / glu** | Nivel de azúcar (glucosa) en sangre. |
| **Target (Objetivo)** | **Medida cuantitativa de la progresión** de la enfermedad un año después de las pruebas iniciales. |

---

### Estructura de la Práctica

#### Parte 1: Análisis Exploratorio y Carga de Datos
1. **Carga del dataset**: Uso de `scikit-learn` para obtener las características (Features) y el vector objetivo (Target).
2. **Visualización de la Estandarización**: Comprobación estadística del escalado previo de los datos.
3. **División de datos (Train/Test Split)**: Separación de pacientes en conjuntos de entrenamiento y prueba para una validación objetiva.

#### Parte 2: Modelado Complejo (Algoritmos de Ensamble)
1. **Random Forest Regressor**: Uso de un bosque aleatorio para capturar relaciones no lineales sin asumir proporcionalidad estricta.
2. **Gradient Boosting**: Construcción secuencial de árboles para corregir errores clínicos de modelos anteriores.

#### Parte 3: Modelado Base (Regresión Lineal)
1. **Regresión Lineal Múltiple**: Intento de trazar un hiperplano que relacione proporcionalmente las variables clínicas con la progresión.
2. **Análisis de Coeficientes**: Evaluación de qué variables (como el IMC o la edad) tienen más peso en el modelo.
3. **Métricas de error**: Cálculo del Error Cuadrático Medio (MSE) y el coeficiente $R^2$.

#### Parte 4: Comparación de Resultados
* **Análisis de Métricas**: Comparación final para determinar si los algoritmos de ensamble reducen el error respecto a la regresión lineal.
* **Importancia de Características (Feature Importance)**: Identificación de las variables médicas más críticas para determinar el avance de la enfermedad.

