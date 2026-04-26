# Ejercicio 1: Detección de Tumores Cerebrales con YOLO (Deep Learning)

## Descripción del Problema
En este ejercicio vamos a explorar la aplicación de modelos de visión artificial de última generación (YOLO) al análisis de imágenes médicas. Realizaremos una prueba de concepto estructurada en varias fases directamente desde un entorno de experimentación (Jupyter Notebook):

1. **Fase de Preparación y Contexto**: Comprenderemos la arquitectura de YOLO y cómo se estructura un conjunto de datos médicos para el entrenamiento supervisado.

2. **Fase de Entrenamiento e Inferencia**: Utilizaremos librerías modernas para entrenar el modelo en la detección de tumores cerebrales y validaremos su eficacia con imágenes reales.

3. **Fase de Exportación**: Prepararemos el modelo final para que pueda ser desplegado e integrado en sistemas hospitalarios.

El enfoque es crear una experiencia práctica y guiada a través de un notebook interactivo, permitiendo tanto a perfiles técnicos como clínicos comprender el ciclo de vida completo de un modelo de Deep Learning en salud.

## Entorno de Experimentación (YOLO26)

Para este ejercicio utilizaremos el modelo **YOLO26** (You Only Look Once), desarrollado por Ultralytics. YOLO es reconocido por su altísima velocidad y precisión en la detección de objetos en tiempo real, lo que lo hace ideal para el análisis rápido y asistido de Resonancias Magnéticas (RM).

Todo el flujo de trabajo se ha unificado en el notebook de Python incluido en esta carpeta: `dl__yolo_deteccion_tumores.ipynb`.


## Parte 1: Configuración y Datos

Para comenzar la práctica, abre el archivo `dl__yolo_deteccion_tumores.ipynb`. En las primeras secciones del documento:

1) Analizaremos la **Estructura del Dataset**. Observaremos cómo los datos médicos se dividen en un conjunto de entrenamiento (*Training set*) para que el modelo aprenda y un conjunto de validación (*Testing set*) para evaluar su rendimiento de forma objetiva.

2) Ejecutaremos la instalación de la librería `ultralytics` y verificaremos que el hardware (especialmente la GPU) esté correctamente configurado para procesar el aprendizaje profundo de forma eficiente.

3) Revisaremos el archivo de configuración `brain-tumor.yaml`, que actúa como un mapa de instrucciones para que el modelo identifique correctamente los directorios y las clases a diagnosticar (imágenes positivas con tumor y negativas).


## Parte 2: Entrenamiento del Modelo (Train)

Avanzando en el notebook, llegaremos a la fase central de entrenamiento:

1) Ejecutaremos la celda de entrenamiento donde inicializamos un modelo ligero pre-entrenado base (`yolo26n.pt`). Esto nos permite acelerar el proceso de aprendizaje general de patrones visuales (*Transfer Learning*).

2) Instruiremos a la red neuronal para que analice nuestro dataset específico durante varias **épocas** (iteraciones completas sobre todos los datos). Durante este proceso, el modelo ajustará sus conexiones internas para aprender a identificar con precisión geométrica las firmas visuales de un tumor cerebral.


## Parte 3: Predicción e Inferencia (Predict)

Una vez que el modelo "ha aprendido", llega el momento de la verdad y lo pondremos a prueba:

1) Ejecutaremos la sección de inferencia, cargando los mejores pesos matemáticos (*weights*) que nuestro modelo obtuvo durante las pruebas de la fase de entrenamiento (`best.pt`).

2) Pasaremos al modelo una imagen radiológica completamente nueva (que no ha visto nunca) y analizaremos su diagnóstico. El modelo generará las coordenadas para dibujar un cuadro delimitador (*bounding box*) sobre la zona exacta donde identifica el tumor.


## Parte 4: Exportación para Producción (Export)

El paso final para cualquier equipo de desarrollo o ingeniería es preparar el prototipo para el mundo real:

1) Utilizaremos los comandos integrados de Ultralytics para exportar nuestro modelo recién entrenado a un formato estandarizado y optimizado de la industria, como **ONNX**.

2) Este formato intermedio garantiza que el modelo pueda ser consumido e integrado posteriormente en servidores hospitalarios, herramientas PACS o incluso servirse como una API hiper-rápida utilizando motores de inferencia locales, cerrando el ciclo que va desde la investigación hasta la puesta en producción.
