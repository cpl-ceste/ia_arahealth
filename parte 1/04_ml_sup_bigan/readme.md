# Predicción Cuantitativa de la Progresión de la Diabetes (Cohorte Bigan)

### Descripción del Problema
En el ámbito médico, predecir cómo avanzará una enfermedad crónica es fundamental para el diseño de un tratamiento preventivo eficaz. El método tradicional para evaluar el avance de la diabetes implica medir variables clínicas iniciales y luego tener un enfoque reactivo: esperar un año entero para evaluar la progresión real del paciente.

**El Problema:** Esperar un año para saber si la enfermedad avanza agresivamente significa perder una ventana crítica de tiempo donde se podrían aplicar intervenciones preventivas como cambios en la dieta, ejercicio o medicación temprana.

**El Objetivo:** Este proyecto implementa un pipeline completo de Machine Learning capaz de predecir la "progresión cuantitativa" de la diabetes un año después, basándose únicamente en mediciones basales tomadas en el día 0. Esto permite a los profesionales clínicos anticiparse a la enfermedad y personalizar el tratamiento.

---

### El Reto Matemático y Biológico
El cuerpo humano es un sistema altamente complejo. La relación entre ciertos niveles en sangre y la progresión de la enfermedad rara vez es una línea recta perfecta; existen interacciones no lineales y un alto nivel de "ruido" biológico (genética, dieta, estrés). A lo largo de este proyecto, contrastamos **modelos lineales regularizados** tradicionales con **algoritmos de ensamble** (Random Forest, XGBoost) para evaluar qué arquitectura matemática es capaz de capturar mejor esta compleja biología, entendiendo las limitaciones clínicas de la interpolación frente a la extrapolación.

---

### El Dataset: Datos Clínicos de Bigan
A diferencia de los datasets académicos pre-empaquetados, este proyecto utiliza datos del mundo real extraídos del Data Lake de Salud **Bigan**. 

La cohorte inicial consta de más de **6,000 pacientes**. Los datos brutos fueron extraídos mediante consultas SQL/Trino optimizadas (Window Functions) y procesados en Pandas para anonimizarlos y estructurarlos en las variables predictoras clínicas estándar (similares al estudio de diabetes de LARS).

#### Diccionario de Datos

| Variable | Código Original | Descripción Clínica |
| :--- | :--- | :--- |
| **Age** | `nacimiento_year` | Edad del paciente calculada al inicio del estudio. |
| **Sex** | `sexo` | Sexo del paciente (Codificado numéricamente). |
| **BMI** | `IMC` (AK) | Índice de Masa Corporal. |
| **BP** | `PAM` (AG/AH) | Presión Arterial Media (Calculada a partir de TAS y TAD). |
| **S1 / tc** | `2093-3` | Colesterol Total en suero (mg/dL). |
| **S2 / ldl** | `13457-7` | Lipoproteínas de Baja Densidad (Colesterol LDL). |
| **S3 / hdl** | `2085-9` | Lipoproteínas de Alta Densidad (Colesterol HDL). |
| **S4 / tch** | Calculado | Ratio de Colesterol: Colesterol total / HDL. |
| **S5 / ltg** | `2571-8` | Logaritmo del nivel de triglicéridos en suero. |
| **S6 / glu** | `2345-7` | Nivel de glucosa basal en sangre. |
| **Target** | Sintético | **Medida cuantitativa de la progresión** de la enfermedad un año después (Escala 25 - 346). Generada calibrando el ratio Señal/Ruido para emular distribuciones y correlaciones biológicas reales. |


---

### 🛠️ Estructura del Proyecto (Pipeline)

#### Parte 1: Extracción, Limpieza y Feature Engineering
* **Consultas Distribuidas:** Extracción de datos de laboratorio, demográficos y de diagnóstico usando Trino SQL.
* **Transformación de Datos:** Pivotaje de formato largo a ancho, cálculo de variables compuestas (PAM, Ratio Colesterol, Logaritmos) y manejo de valores nulos.
* **Anonimización:** Sustitución de IDs clínicos por identificadores continuos seguros (`paciente_id`).

#### Parte 2: Modelado Complejo (Algoritmos de Ensamble)
* **Random Forest y XGBoost:** Construcción de modelos basados en árboles de decisión para detectar interacciones no lineales.
* **Optimización de Hiperparámetros:** Búsqueda exhaustiva de parámetros (profundidad, estimadores) mediante validación cruzada (K-Fold = 5).

#### Parte 3: Modelado Base (Regresión Lineal Regularizada)
* **Regresión Ridge:** Implementación de modelos lineales con penalización (L2) evaluados mediante `GridSearchCV`.
* **Impacto Biológico:** Análisis de coeficientes (Importancia de características) para confirmar la relevancia de variables como el IMC y los Triglicéridos.
* **Diagnóstico Matemático:** Evaluación de la capacidad del modelo para extrapolar predicciones frente a valores clínicos extremos.

#### Parte 4: Evaluación y Diagnóstico de Modelos
Comparación de los algoritmos utilizando métricas clínicas robustas:
* **$R^2$ (Coeficiente de Determinación):** Para evaluar la proporción de varianza biológica explicada.
* **RMSE (Error Cuadrático Medio) y MAE:** Para medir la desviación real de la predicción clínica.
* **Visualización de Residuos:** Detección de heterocedasticidad y sesgos de extrapolación mediante gráficos de dispersión (Predicción vs. Realidad).

---

### 🚀 Requisitos y Ejecución
Para reproducir este entorno de análisis, se requieren las siguientes librerías de Python:
* `pandas` y `numpy` para manipulación matricial de datos.
* `scikit-learn` para estandarización, validación cruzada y entrenamiento de modelos.
* `xgboost` para el entrenamiento de árboles potenciados.
* `matplotlib` y `seaborn` para la visualización de diagnósticos de Machine Learning.