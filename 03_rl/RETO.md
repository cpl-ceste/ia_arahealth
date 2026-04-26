# Ejercicio 2: RL y la Optimización de Tratamientos Médicos

## Resumen Ejecutivo (The "What and the why")

Este proyecto es un simulador clínico computacional basado en Inteligencia Artificial que modela el proceso de asignar tratamientos médicos a pacientes. Utilizando una rama de la IA conocida como **Aprendizaje por Refuerzo** (Reinforcement Learning), el sistema aprende empíricamente qué terapias son más efectivas para una enfermedad. En lugar de estar pre-programado con reglas médicas rígidas o basarse en un modelo predictivo tradicional, este sistema "aprende haciendo", evaluando el resultado de cada tratamiento administrado a lo largo del tiempo.

Para entender cómo funciona, podemos imaginar a un joven médico residente (el **Agente**) que se enfrenta a una nueva enfermedad con cinco posibles curas en el vademécum. La regla lógica o intuición que usa el médico para decidir qué recetar es su **Política**. Cada vez que un paciente se cura, el médico recibe una **Recompensa** (un refuerzo positivo). Aquí surge su gran dilema clínico: ¿Debería recetar siempre el tratamiento que hasta ahora le ha funcionado mejor (**Explotación**), asegurando curas a corto plazo, o debería probar los otros tratamientos menos conocidos por si alguno resulta ser una cura muy superior a largo plazo (**Exploración**)?

El algoritmo navega este dilema de *Exploración vs. Explotación* intentando encontrar el equilibrio matemático perfecto. Si solo explota, podría quedarse estancado prescribiendo un tratamiento mediocre, ignorando uno excelente. Si solo explora, usará a demasiados pacientes como "conejillos de indias" con tratamientos ineficaces. Diferentes estrategias del código (Epsilon-Greedy, Softmax) modelan cómo el agente toma estas decisiones para maximizar la cantidad de pacientes curados globalmente.

En el sector salud, la capacidad de optimizar tratamientos a través del análisis de datos es revolucionaria. La medicina basada en la evidencia tradicional a menudo depende de ensayos clínicos prolongados, costosos y realizados en poblaciones muy controladas y limitadas. Una herramienta basada en RL, si se aplica de manera ética y segura analizando datos anonimizados del mundo real (Real World Data), permitiría a los sistemas de salud identificar rápidamente la eficacia relativa de las terapias a medida que interactúan de verdad con la población.

Además de la eficiencia clínica y el apoyo analítico a los médicos, esto tiene un impacto directo en la sostenibilidad del ecosistema sanitario. Al converger rápidamente mediante IA hacia el tratamiento óptimo, se reducen las complicaciones, los días de hospitalización y se minimizan los recursos gastados en opciones terapéuticas ineficaces, avanzando hacia un modelo de atención médica más veloz y precisa.

---

## Cuestionario de Comprensión (Check-point)

**Funcionamiento Lógico del Proyecto:**

1. **¿Qué representa la "Recompensa" (Reward) en la clase `HealthEnvironment` del código?**
   - a) El pago o margen económico que recibe el hospital por el tratamiento administrado.
   - b) Un valor numérico de `1.0` si el paciente se cura y `0.0` si el tratamiento no tiene éxito.
   - c) La cantidad de pacientes que entran por urgencias cada día.
   - d) El tiempo en milisegundos que tarda el algoritmo en tomar la decisión.

2. **En el agente *Epsilon-Greedy*, ¿qué función exacta cumple el parámetro $\epsilon$ (epsilon)?**
   - a) Determina la temperatura corporal media a la que los medicamentos hacen efecto.
   - b) Fija la probabilidad porcentual (ej. 0.1 o 10%) con la que la IA decidirá "explorar" un tratamiento completamente al azar, en lugar de elegir el mejor conocido.
   - c) Calcula el margen de error estadístico (p-value) del ensayo clínico subyacente.
   - d) Asegura que el Agente recete todos los tratamientos exactamente el mismo número de veces.

3. **¿En qué se diferencia conceptualmente el agente de *Gradiente de Política (Policy Gradient)* de los demás en el código?**
   - a) En lugar de intentar adivinar y calcular la tasa numérica de éxito de cada cura, aprende una preferencia probabilística, "subiendo por la pendiente matemática" de los éxitos relativos.
   - b) Es el único agente que "hace trampa" y lee el array de las probabilidades de éxito reales del entorno.
   - c) Solo recomienda un tratamiento si el paciente tiene una probabilidad de supervivencia mayor al 50%.
   - d) No aprende, simplemente actúa de forma aleatoria todo el tiempo.

**Riesgos Clínicos y Consideraciones Éticas:**

4. **En el mundo real de un hospital, aplicar pura "Exploración" ciega dictada por una máquina sobre pacientes reales implica un riesgo clínico y ético gravísimo. ¿Por qué?**
   - a) Porque la exploración algorítmica consume demasiados recursos de la nube, saturando la red del hospital.
   - b) Porque recetar aleatoriamente un tratamiento subóptimo o desconocido a un paciente cuando el médico ya sabe que existe una cura probada y mejor, viola flagrantemente el principio ético de no maleficencia.
   - c) Porque la IA borraría la historia clínica al fallar un tratamiento.
   - d) Porque los seguros médicos no cubren a los pacientes si un algoritmo toma la decisión.

5. **Si este algoritmo de Aprendizaje por Refuerzo estuviese recomendando medicamentos reales desde el día 1, ¿cuál es un peligro fundamental del enfoque de "Explotación extrema" temprana?**
   - a) La IA se bloquearía por consumir demasiada memoria RAM.
   - b) Si el paciente 1 se cura de pura casualidad con el Peor Tratamiento, la IA podría quedarse matemáticamente "atrapada" recomendando siempre esa mala opción terapéutica, sin atreverse jamás a probar opciones que eran inherentemente mejores.
   - c) Los médicos tendrían que auditar el código Python diariamente por imperativo legal.
   - d) El paciente se curaría demasiado rápido, reduciendo la facturación del hospital.

---

## Dinámica de "Retos por Niveles"

**Reto A (Ajuste de Parámetros): El Experimento de la Curación**
*Objetivo:* Comprender el delicado y arriesgado equilibrio entre probar cosas nuevas y asegurar el resultado.
*Tarea:* Ve a la celda del código donde se configuran los hiperparámetros de los `agents` (alrededor de `EpsilonGreedyAgent(epsilon=0.1)`). 
1. **Modifica `epsilon = 0.0`** (0% exploración, 100% explotación pura o "voracidad absoluta"). Ejecuta la simulación. ¿La IA logra descubrir el mejor tratamiento a la larga, o se estanca rápidamente en uno mediocre por puro azar inicial?
2. **Modifica `epsilon = 0.5`** (50% exploración). Vuelve a simular. ¿Consigue una buena tasa general de pacientes curados (línea de recompensa acumulada) o verás que la curva se resiente por estar sacrificando sistemáticamente a demasiados pacientes probando tratamientos que la IA ya sabe que son peores?

**Reto B (Nuevas Variables): Penalizando el Daño (Primum Non Nocere)**
*Objetivo:* En medicina, el principio ético fundamental es "lo primero, no hacer daño". Lograr curar la enfermedad pero causar un fallo orgánico severo no es un éxito, es una negligencia.
*Tarea:* Actualmente, el método `action(self, action: int)` de la clase `HealthEnvironment` devuelve una recompensa binaria de `1.0` (Cura) o `0.0` (No Cura). 
*Desafío Técnico:* ¿Cómo modificarías el código en Python de ese entorno para introducir una "penalización" severa por efectos adversos? Plantea cómo programar que, con cierta probabilidad oculta, el tratamiento cause toxicidad y devuelva una recompensa de `-5.0`. Reflexiona sobre cómo este cambio en las variables ambientales afectaría drásticamente la curva de aprendizaje y precaución de agentes como el Softmax.

---

## Test de Evaluación de Políticas

Compara críticamente el desempeño de las políticas presentes en el repositorio tras ver la simulación:

1. **El Algoritmo "Tonto" vs. El Algoritmo "Inteligente":** 
   En la gráfica de resultados de recompensa acumulada en el tiempo, ¿por qué la curva del `RandomAgent` (Agente Aleatorio) dibuja una línea recta con poca pendiente, mientras que las de los otros agentes se curvan hacia arriba, ganando pendiente (tasa de curación) a medida que avanzan los pasos?

2. **La Limitación Crónica de Epsilon-Greedy:** 
   Incluso al llegar al paciente número 10,000, cuando la IA ya ha calculado con precisión casi perfecta cuál es el tratamiento que cura en un 80% de las veces, la política `Epsilon-Greedy` (con $\epsilon=0.1$) seguirá tomando decisiones subóptimas deliberadas. ¿Por qué ocurre esto y cómo penaliza el desempeño final en comparación con estrategias que reducen su exploración con el tiempo (Decaying Epsilon)?

3. **Adaptabilidad a la Mutación Médica:** 
   Si introdujéramos código para que en el paso de tiempo (paciente) número 500, el virus "mutase" y las `true_probs` cambiasen repentinamente, haciendo inútil el tratamiento estrella actual. De los algoritmos que evalúan promedios históricos, ¿por qué sufrirían una "inercia cognitiva" y tardarían muchísimo tiempo en corregir el rumbo, provocando una caída temporal drástica en la supervivencia de los pacientes?

---

## Debate Ético: Equidad y Bias Algorítmico

*Para reflexionar en equipo (o de manera individual):*

Imaginad que el algoritmo implementado en este código no parte como una tabla rasa (no empieza con sus matrices de conocimiento en `np.zeros`), sino que inicializamos sus estimaciones previas (`q_values` o preferencias matemáticas) cargándoles el historial clínico de los últimos 10 años del hospital.

Sin embargo, ese historial de datos pertenece en un **90% a hombres de una etnia mayoritaria**, para los cuales la genética hace que el *"Tratamiento A"* sea una opción excelente. Para las **mujeres de una etnia minoritaria**, las variaciones metabólicas dictan que el *"Tratamiento A"* es muy ineficaz, y el *"Tratamiento C"* sería el óptimo.

Basándote en el mecanismo interno de "Explotación" que rige el código que has analizado:
- ¿Cómo afectará este pesado sesgo (bias) de inicialización histórica a las primeras pacientes femeninas de esa minoría que entren al hospital y sean evaluadas por el sistema?
- ¿Tendrá el algoritmo la "agilidad exploratoria" suficiente para darse cuenta de que necesita el "Tratamiento C" para ellas, o la inercia estadística provocará que el sistema actúe "ciego" perjudicándolas sistemáticamente al recetarles el "Tratamiento A"?
- Desde la arquitectura del código, ¿cómo se tendría que modificar el estado (`get_observation()`) para que el algoritmo no trate a los pacientes como un bloque homogéneo y aprenda políticas personalizadas, equitativas y libres de sesgos para cada subgrupo demográfico?
