# Predicción de Victoria en League of Legends con Redes Bayesianas

*(Basado en el proyecto académico: "Predicció de la victòria al League of Legends mitjançant Xarxes Bayesianes")*

> **Consulta la presentación y los detalles visuales del modelo en:** [`project_presentation.pdf`](project_presentation.pdf)

## Objetivo del Proyecto
El objetivo de este estudio es predecir la probabilidad de victoria del equipo azul en una partida de League of Legends **exactamente en el minuto 10**. Para ello, se analizan los factores más determinantes del juego temprano (early game) como la economía (oro y experiencia), el control de objetivos neutrales (dragones/heraldos) y la presión en el mapa.

## Conjunto de Datos
* **Origen:** Datos extraídos de Kaggle mediante la API oficial de Riot Games.
* **Contexto:** Partidas clasificatorias de alto rango (Diamante y Master).
* **Preprocesamiento:** Se partió de 9.879 partidas. Tras la limpieza de valores nulos y el filtrado de outliers (eliminación de las colas mediante cuantiles en variables de visión), la muestra final limpia consta de **9.131 observaciones**.

## Metodología
* **Discretización:** Dado que las Redes Bayesianas requieren variables discretas, se aplicó un método de cuantiles (n=6) para variables continuas (oro, experiencia, kills) garantizando categorías equilibradas, y codificación lógica para eventos binarios y torres.
* **Arquitecturas Evaluadas:** Se compararon cuatro variaciones de modelos bayesianos:
  * *Naive Bayes (NB)* vs *Augmented Naive Bayes (ANB)* (para capturar sinergias y el efecto snowball).
  * Uso de todas las variables (Full) vs selección de variables mediante **Markov Blanket (MB)**.
* **Validación Estadística:** Los modelos se evaluaron usando 10-fold Cross Validation y comparando métricas (Accuracy, AUC, F1, Brier Score, BIC). La significación estadística de las diferencias se comprobó mediante tests de Shapiro-Wilk y Paired t-tests.

## Resultados y Selección del Modelo
El modelo seleccionado como óptimo fue el **ANB_MB (Augmented Naive Bayes con Markov Blanket)** por los siguientes motivos:
* **Parsimonia:** El Markov Blanket logró reducir las variables de 23 a solo 10 (con 18 arcos), manteniendo una alta capacidad predictiva y eliminando el ruido.
* **Calibración Probabilística:** Logró el mejor (menor) Brier Score (0.189), siendo estadísticamente superior al resto de modelos (p-valor < 0.001). Esto significa que las probabilidades que devuelve el modelo son altamente fiables y no sufren de "exceso de confianza".
* **Métricas:** Alcanzó una Accuracy en torno al 71.7% y un AUC de 0.794, con el mejor criterio de información bayesiana (BIC) entre los modelos reducidos.

## Casos de Estudio (Inferencias)
El modelo demostró estar perfectamente alineado con la teoría y estrategia real del juego:
1. **Dominio Total:** Ventaja alta en oro/experiencia + dragón + primera torre otorga un **93.0% de probabilidad de victoria**.
2. **Ventaja Parcial:** Oro igualado pero con control del primer dragón otorga un **67.6%**.
3. **Farmeo Pasivo:** Buen nivel de oro pero sin control de objetivos neutrales ni muertes relevantes castiga la probabilidad, reduciéndola a un **35.3%**.
4. **Desventaja Severa:** El equipo rival domina oro, torres y monstruos épicos, dejando la probabilidad de remontada en solo un **8.0%**.

## Autores
Proyecto de Modelización de Datos Complejos realizado en la Universitat Autònoma de Barcelona (UAB) por:
* Pol Ortiz
* Pau Verdejo