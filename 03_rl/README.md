# Ejercicio 3: Optimización de Tratamientos Médicos con Aprendizaje por Refuerzo

## Descripción del Problema
En este ejercicio vamos a explorar cómo aplicar algoritmos de **Aprendizaje por Refuerzo (Reinforcement Learning - RL)**, específicamente el problema clásico de los **Bandidos Multi-Brazo (Multi-Armed Bandits)**, para optimizar la selección de tratamientos médicos en un entorno de incertidumbre.

Imagina que eres un médico o un sistema de IA encargado de asignar tratamientos a pacientes que llegan con una enfermedad específica. Tienes a tu disposición 5 tratamientos diferentes, pero desconoces la probabilidad real de éxito (cura) de cada uno. Tu objetivo a largo plazo es curar a la mayor cantidad posible de pacientes. 

Esto plantea el dilema fundamental del Aprendizaje por Refuerzo:
- **Exploración**: Probar tratamientos sobre los que tienes poca información para descubrir si son más efectivos de lo que crees.
- **Explotación**: Utilizar el tratamiento que, basándote en tu experiencia previa, ha demostrado ser el mejor para asegurar la cura del paciente actual.

El enfoque del ejercicio es implementar y testear en Python diferentes "Agentes" (estrategias lógicas de toma de decisiones) y comparar su rendimiento en un entorno clínico simulado a través del notebook interactivo `rl_health_bandits.ipynb`.

---

## Parte 1: El Entorno Médico y la Línea Base

Abre el notebook `rl_health_bandits.ipynb` y ejecuta las celdas secuencialmente para establecer la simulación:

1) **Definición del Entorno (`HealthEnvironment`)**: 
Se inicializa un entorno que simula las interacciones con los pacientes. El entorno tiene probabilidades de éxito fijas pero ocultas para el agente (por ejemplo, `[0.1, 0.5, 0.8, 0.2, 0.3]`). En este caso hipotético, el tratamiento en la posición 2 es el óptimo (80% de éxito).

2) **Dinámica de Recompensas**: 
Por cada paciente (paso de tiempo), el agente receta un tratamiento. El entorno evalúa la acción basándose en sus probabilidades y devuelve una recompensa: `1.0` si el paciente se cura, y `0.0` si el tratamiento fracasa.

3) **El Agente Aleatorio**: 
Implementamos un `RandomAgent` que simplemente receta tratamientos al azar, ignorando las recompensas pasadas. Este agente sirve como nuestra "Línea Base" para evaluar qué tan inteligentes son realmente los algoritmos posteriores.


| Concepto Técnico | Traducción Clínica | Por qué importa |
| :--- | :--- | :--- |
| Agente (Agent) | El Sistema de Recomendación Médica | Es quien toma la decisión. |
| Entorno (Environment) | El Paciente / La Enfermedad | Es lo que reacciona al tratamiento. |
| Regret (Arrepentimiento) | Salud perdida por no elegir el tratamiento óptimo | Es la métrica que queremos minimizar en medicina. |


---

## Parte 2: Implementación de Estrategias Inteligentes

En esta fase, definimos a los agentes que "aprenden" de la experiencia. Estos agentes guardan un historial de los éxitos y fracasos para guiar sus decisiones futuras.

1) **Agente Epsilon-Greedy**: 
Una estrategia clásica y robusta. Con una probabilidad muy pequeña definida por `epsilon` (ej. 10%), el agente decide "explorar" un tratamiento al azar. El 90% restante del tiempo, "explota" el mejor tratamiento descubierto hasta ahora calculando la tasa de éxito histórica (Q-values) de cada opción.

2) **Agente Softmax (Temperatura)**:
A diferencia del enfoque binario del Epsilon-Greedy, Softmax asigna probabilidades relativas a cada tratamiento en función de su valor estimado. Se introduce el concepto de "temperatura": a mayor temperatura, el agente es más atrevido y explora de forma más aleatoria; a menor temperatura, se vuelve más conservador e intensifica la explotación del mejor tratamiento.

3) **Agente de Gradiente de Política (Policy Gradient)**:
Un enfoque más avanzado que no intenta adivinar la tasa de éxito numérica de cada tratamiento. En su lugar, el algoritmo aprende una "preferencia". Si recetar un tratamiento resulta en una cura (una recompensa superior a la media histórica), la preferencia matemática por recetar ese tratamiento en el futuro aumenta.

---

## Parte 3: Simulación y Comparación de Resultados

Finalmente, pondremos a competir a los agentes en nuestro hospital virtual para ver quién cura a más pacientes.

1) **Ejecución del Experimento Múltiple**: 
Dirígete a la sección de "Comparación y Resultados" en el notebook. Al ejecutar la celda de simulación, el código instanciará los 4 agentes y los someterá a 1000 iteraciones (pacientes). Para obtener datos estadísticamente significativos, este proceso se repite 100 veces y se promedian las recompensas obtenidas.

2) **Análisis de Métricas**: 
Observa los logs de salida. Podrás identificar qué agente logró descubrir y aferrarse al tratamiento óptimo de forma más eficiente, superando ampliamente al agente aleatorio.

3) **Modificación de Hiperparámetros (Reto)**: 
Como dinámica final, vuelve a las celdas donde se instancian los agentes en el experimento y modifica los parámetros clave:
- Cambia la matriz de probabilidades ocultas (`true_probs`). ¿Qué pasa si dos tratamientos son casi igual de buenos (ej. 80% y 82%)?
- Sube o baja el valor de `epsilon` y la `temperatura`. ¿Cómo cambia la velocidad de aprendizaje versus la ganancia total? Observa cómo demasiada exploración es mala, pero muy poca puede estancar a la IA en un tratamiento subóptimo.
