# neuralNetwork
# Análisis de Arquitecturas Convolucionales para Clasificación Binaria de Imágenes

## 1. Descripción del Problema

En este laboratorio se estudia el papel de las capas convolucionales como componentes arquitectónicos fundamentales en redes neuronales, analizando cómo sus decisiones de diseño influyen en el aprendizaje, la eficiencia y la capacidad de generalización.

A diferencia de tratar las redes neuronales como cajas negras, el objetivo es comprender explícitamente el **sesgo inductivo** que introducen las convoluciones y compararlo con un modelo base sin capas convolucionales.

El problema abordado es una **clasificación binaria de imágenes**, lo que permite aislar y analizar con mayor claridad el impacto de la arquitectura convolucional.

---

## 2. Dataset

### Dataset seleccionado
**Fashion-MNIST** (subconjunto binario)

- Tipo de datos: Imágenes en escala de grises
- Resolución: 28 × 28 píxeles
- Canales: 1
- Clases utilizadas:
  - Clase 0: T-shirt / Top
  - Clase 1: Sneaker

### Justificación del uso de CNN
El dataset contiene patrones espaciales claros como bordes, texturas y formas, lo que lo hace especialmente adecuado para modelos convolucionales que explotan la localidad espacial y la compartición de pesos.

Reducir el problema a dos clases permite centrarse en el análisis arquitectónico sin introducir complejidad innecesaria propia de la clasificación multiclase.

---

## 3. Exploración del Dataset (EDA)

Se realizó una exploración básica con los siguientes objetivos:

- Verificar el tamaño del dataset y la distribución de clases
- Confirmar la dimensión y estructura de las imágenes
- Visualizar ejemplos representativos de cada clase
- Definir el preprocesamiento necesario

### Observaciones principales
- El dataset se encuentra balanceado entre ambas clases.
- Las imágenes tienen dimensiones fijas de 28×28 píxeles.
- Se realizó normalización de los valores de píxel al rango [0,1].

El objetivo del EDA fue comprender la estructura de los datos, no realizar un análisis estadístico exhaustivo.

---

## 4. Modelo Base (Sin Capas Convolucionales)

### Arquitectura
El modelo base está compuesto únicamente por capas totalmente conectadas:
Flatten
Dense(128, ReLU)
Dense(64, ReLU)
Dense(1, Sigmoid)


### Características
- Alto número de parámetros debido a la conexión completa entre píxeles y neuronas.
- No preserva la estructura espacial de las imágenes.
- Sirve como punto de referencia para evaluar el impacto real de las capas convolucionales.

### Limitaciones observadas
- Incapacidad para capturar patrones locales como bordes o formas.
- Sensibilidad a variaciones espaciales.
- Generalización limitada frente a la CNN.

---

## 5. Arquitectura Convolucional Propuesta

### Diseño de la CNN
Se diseñó una arquitectura convolucional simple pero intencional:
Conv2D(32, 3×3) + ReLU
MaxPooling(2×2)

Conv2D(64, 3×3) + ReLU
MaxPooling(2×2)

Flatten
Dense(64, ReLU)
Dense(1, Sigmoid)


### Justificación de decisiones
- **Kernels 3×3**: capturan patrones locales de forma eficiente.
- **Incremento progresivo de filtros**: aprendizaje jerárquico de características.
- **Pooling**: reduce la dimensionalidad e introduce invariancia a traslaciones.
- **Sigmoid**: adecuada para clasificación binaria.

Esta arquitectura introduce sesgos inductivos como localidad y compartición de pesos, fundamentales para el procesamiento de datos visuales.

---

## 6. Experimento Controlado

### Variable estudiada
**Profundidad de la red convolucional** (número de capas Conv2D)

### Configuraciones evaluadas
- CNN con 1 capa convolucional
- CNN con 2 capas convolucionales
- CNN con 3 capas convolucionales

Todos los demás hiperparámetros se mantuvieron constantes.

### Resultados y observaciones
- Una sola capa convolucional mostró signos de subajuste.
- Dos capas convolucionales ofrecieron el mejor equilibrio entre desempeño y complejidad.
- Agregar una tercera capa produjo mejoras marginales a costa de mayor complejidad.

Incrementar la profundidad mejora la representación hasta cierto punto, después introduce complejidad innecesaria.

---

## 7. Interpretación y Razonamiento Arquitectónico

### ¿Por qué la CNN supera al modelo base?
La CNN preserva la estructura espacial de las imágenes y aprende patrones locales mediante filtros compartidos, lo que reduce el número de parámetros y mejora la generalización.

### Sesgo inductivo introducido por la convolución
- Localidad espacial
- Equivarianza a traslaciones
- Aprendizaje jerárquico de características

### ¿Cuándo no es apropiado usar convoluciones?
Las capas convolucionales no son adecuadas para datos sin estructura espacial, como datos tabulares o problemas donde la posición de las variables no tiene significado.

Cuando el supuesto de localidad no se cumple, el sesgo inductivo de la convolución puede degradar el desempeño del modelo.

---

## 8. Entrenamiento y Despliegue en SageMaker

El modelo convolucional fue entrenado utilizando Amazon SageMaker y posteriormente desplegado como un endpoint de inferencia.

Este paso valida que el modelo no solo aprende correctamente, sino que puede integrarse en un entorno productivo.

---

## 9. Conclusiones

Este laboratorio demuestra que las capas convolucionales no son solo una optimización computacional, sino una decisión arquitectónica que introduce conocimiento previo sobre la estructura de los datos, resultando en modelos más eficientes y con mejor capacidad de generalización para problemas visuales.




