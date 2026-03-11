# Predicción de Fuga de Clientes (Churn) - Proyecto Telecom X

## Descripción General
Este proyecto de Machine Learning tiene como objetivo predecir la cancelación de servicios (Churn) en una empresa de telecomunicaciones. A través del entrenamiento y evaluación de modelos de clasificación, el proyecto busca identificar proactivamente a los usuarios con mayor probabilidad de abandonar la compañía, permitiendo la transición de un modelo de retención reactivo a uno predictivo y basado en evidencia.

## Contexto de Negocio y Métrica de Éxito
En la industria de las telecomunicaciones, el costo de adquirir un nuevo cliente es significativamente mayor que el de retener a uno existente. Por lo tanto, el problema de negocio dicta que un **Falso Negativo** (no detectar a un cliente que se va a fugar) tiene un impacto financiero mucho más grave que un **Falso Positivo** (ofrecer un incentivo de retención a un cliente que planeaba quedarse). 

Bajo esta premisa, el desarrollo y la selección del modelo predictivo se optimizó priorizando la métrica de **Recall** para la clase minoritaria (Churn = 1), asegurando la máxima captura de posibles bajas.

## Metodología y Preprocesamiento de Datos
El flujo de trabajo se estructuró bajo estándares rigurosos de Ciencia de Datos para evitar fugas de información (*Data Leakage*) y asegurar la generalización del modelo:

1. **Ingeniería de Características:** Transformación de variables categóricas mediante `Label Encoding` y `One-Hot Encoding`.
2. **División de Datos:** Separación estratificada del conjunto de datos en Entrenamiento (80%) y Prueba (20%).
3. **Balanceo de Clases:** Aplicación de la técnica **SMOTE** (Synthetic Minority Over-sampling Technique) exclusivamente en el conjunto de entrenamiento para abordar el desbalanceo de la variable objetivo.
4. **Escalado:** Estandarización de variables numéricas continuas mediante `StandardScaler` para optimizar la convergencia de modelos paramétricos.

## Modelado y Evaluación
Se entrenaron y contrastaron dos algoritmos de naturalezas distintas para evaluar su rendimiento sobre el conjunto de prueba:

* **Random Forest (Modelo Base):** Alcanzó una exactitud global (Accuracy) del 79%. Sin embargo, presentó un rendimiento deficiente en la detección de la clase minoritaria, obteniendo un Recall de solo el 50%, lo que indica un sesgo inaceptable hacia la clase mayoritaria.
* **Regresión Logística:** Aunque presentó una exactitud global ligeramente inferior (74%), demostró un desempeño muy superior en la métrica crítica, alcanzando un **Recall del 79%** en la detección de fuga.

**Selección de Modelo:** Se seleccionó la Regresión Logística como el modelo final para producción debido a su superioridad en la métrica de Recall y a su alta interpretabilidad a través del análisis de coeficientes.

## Análisis de Importancia de Variables (Feature Importance)
El análisis de los coeficientes del modelo logístico reveló patrones clave en el comportamiento del usuario:

* **Factores de Riesgo (Impulsores de Churn):** Los contratos de renovación mensual (`Contract_Month-to-month`) y los métodos de pago manuales (`PaymentMethod_Electronic check`) presentan la correlación positiva más fuerte con la fuga de clientes.
* **Factores de Retención:** Los contratos a largo plazo (`Contract_Two year`) y la antigüedad acumulada del cliente (`Tenure`) son los principales estabilizadores de la cartera.

## Recomendaciones Estratégicas
Con base en los resultados del modelo, se proponen las siguientes acciones de negocio:

1. **Migración de Contratos:** Diseñar campañas de incentivos (ej. mejora temporal de ancho de banda) enfocadas en trasladar clientes de modalidad mensual a contratos anuales.
2. **Automatización de Pagos:** Ofrecer descuentos permanentes en la facturación a los usuarios que transicionen de pagos manuales a cobro automático mediante tarjeta de crédito.
3. **Sistema de Alerta Temprana:** Integrar las predicciones probabilísticas de la Regresión Logística en el CRM del Call Center, habilitando a los agentes de soporte para ofrecer incentivos de retención en tiempo real cuando un cliente de alto riesgo establezca contacto.

## Estructura del Repositorio
* `data/datos_tratados.csv`: Dataset preprocesado y listo para la ingesta del modelo.
* `notebooks/Telecom_X_Modelado.ipynb`: Jupyter Notebook detallado con el código fuente del entrenamiento, validación y evaluación de los modelos.
* `images/`: Gráficos de evaluación y matrices de confusión exportadas.
* `modelo_churn_produccion.pkl`: Modelo final serializado y optimizado para su despliegue.

## Ejecución y Reproducibilidad
Para reproducir el entorno de desarrollo y ejecutar el análisis:

1. Instalar las dependencias requeridas:
   `pip install pandas numpy scikit-learn imbalanced-learn matplotlib seaborn`
2. Ejecutar el cuaderno `Telecom_X_Modelado.ipynb` para visualizar el paso a paso del modelado y la generación del archivo `.pkl`.

## Stack Tecnológico
* **Lenguaje:** Python 3.8+
* **Procesamiento de Datos:** Pandas, Numpy
* **Machine Learning:** Scikit-Learn, Imbalanced-learn
* **Visualización de Datos:** Matplotlib, Seaborn
