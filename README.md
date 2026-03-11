# Proyecto Telecom X: Análisis y Predicción de Fuga de Clientes (Churn)

## Descripción del Proyecto
Este proyecto de Data Science tiene como objetivo principal predecir la cancelación de servicios (Churn) en la empresa de telecomunicaciones "Telecom X". A través de la aplicación del método científico, analizamos el comportamiento de los clientes, limpiamos los datos históricos y entrenamos modelos de Machine Learning para identificar proactivamente qué usuarios tienen mayor probabilidad de abandonar la compañía, permitiendo así diseñar estrategias de retención basadas en evidencia.

El proyecto se divide en dos grandes fases: **Análisis Exploratorio de Datos (EDA)** y **Modelado Predictivo**.

---

## Metodología (Enfoque Científico)

### Fase 1: Observación y Preprocesamiento de Datos (Parte 1)
En la primera etapa, trabajamos con los datos crudos para entender la naturaleza del problema.
* **Limpieza de Datos:** Tratamiento de valores nulos, corrección de formatos y eliminación de variables sin poder predictivo. El resultado de esta fase fue exportado en el archivo `datos_tratados.csv`.
* **Análisis Exploratorio (EDA):** Identificamos visualmente patrones preliminares. Observamos que variables como el tiempo en la empresa (`Tenure`) y los costos (`Charges`) presentaban distribuciones distintas entre los clientes que se fugaban y los que se quedaban.

### Fase 2: Experimentación y Modelado (Parte 2)
Con los datos limpios, formulamos la hipótesis de que podíamos predecir el Churn utilizando algoritmos de clasificación.
* **Preparación:** * Se aplicó codificación (Label Encoding y One-Hot Encoding) para transformar variables categóricas en numéricas.
  * Los datos se dividieron en conjuntos de Entrenamiento (80%) y Prueba (20%) para evitar Data Leakage.
  * Se aplicó **SMOTE** únicamente al conjunto de entrenamiento para balancear la clase minoritaria (Churn=1), permitiendo a los modelos aprender de forma equitativa.
  * Se estandarizaron las variables numéricas con `StandardScaler`.
* **Modelos Entrenados:** Evaluamos dos algoritmos con naturalezas distintas:
  1. **Random Forest:** Modelo de ensamble basado en árboles (no requiere normalización estricta). Actuó como nuestro modelo base.
  2. **Regresión Logística:** Modelo lineal paramétrico sensible a la escala (justificando la estandarización previa).

---

## Resultados de la Experimentación

La evaluación de los modelos en el conjunto de prueba arrojó los siguientes resultados:

1. **Random Forest:** Logró la mejor Exactitud global (Accuracy: 79%). Sin embargo, presentó problemas para detectar la clase minoritaria, obteniendo un Recall del 50% para la clase Churn. Esto sugiere un ligero sesgo hacia la clase mayoritaria (retención).
2. **Regresión Logística:** Aunque su Exactitud global fue menor (74%), demostró un desempeño muy superior en la métrica más crítica para el negocio: obtuvo un **Recall del 79%** en la detección de fuga.

**Selección del Modelo:** Bajo la premisa de negocio de que un "Falso Positivo" (ofrecer un descuento a quien se iba a quedar) es menos costoso que un "Falso Negativo" (perder a un cliente por no detectarlo), se selecciona la **Regresión Logística** como el modelo óptimo.

---

## Análisis de Variables (Feature Importance)
Tanto la extracción de importancia de Random Forest como el análisis de coeficientes de la Regresión Logística confirmaron los mismos patrones:
* **Variables que impulsan la fuga:** El contrato mes a mes (`Contract_Month-to-month`) y el pago con cheque electrónico (`PaymentMethod_Electronic check`). Adicionalmente, los altos cargos totales (`ChargesTotal`) empujan al cliente a la salida.
* **Variables que retienen al cliente:** Contratos a largo plazo (`Contract_Two year`) y el tiempo acumulado en la empresa (`Tenure`). La fidelidad se construye con el tiempo.

---

## Conclusiones y Estrategias Basadas en Evidencia
Basados estrictamente en los datos analizados y el modelo seleccionado, proponemos a la gerencia las siguientes estrategias de retención:

1. **Migración de Contratos:** El modelo indica que el contrato mensual es el principal riesgo. Estrategia: Ofrecer descuentos agresivos en los primeros meses o mejoras en el servicio (ej. mayor velocidad) a clientes que acepten pasar de formato mensual a contrato anual.
2. **Fomento del Pago Automático:** El pago mediante "Electronic check" presenta una alta correlación con la cancelación. Estrategia: Eliminar esta fricción ofreciendo un descuento permanente (ej. 5%) en la factura a quienes domicilien su pago a una tarjeta de crédito o transferencia bancaria.
3. **Programa de Onboarding (Cuidado de los nuevos):** Dado que la retención aumenta significativamente con el `Tenure`, los primeros meses son críticos. Estrategia: Implementar soporte proactivo y llamadas de control de calidad durante los primeros 6 meses.
4. **Sistema de Alerta Temprana en Call Center:** Estrategia: Implementar el modelo de Regresión Logística en los sistemas de atención al cliente. Si el modelo arroja una alta probabilidad de fuga en tiempo real, habilitar a los agentes de soporte para ofrecer incentivos de retención de inmediato.
   
---

## Estructura del Proyecto
El repositorio está organizado de la siguiente manera para facilitar su reproducibilidad:
* **data/datos_tratados.csv**: Dataset limpio y preprocesado.
* **notebooks/Telecom_X_Modelado.ipynb**: Cuaderno principal con el código.
* **images/**: Visualizaciones exportadas del EDA (matriz de correlación, boxplots).
* **modelo_churn_produccion.pkl**: Modelo final serializado y listo para usar.
* **README.md**: Documentación del proyecto.

---

## Instrucciones para Ejecutar el Proyecto
**1. Requisitos e Instalación**
Asegúrate de tener Python 3.8+ e instala las dependencias en tu terminal:
`pip install pandas numpy scikit-learn imbalanced-learn matplotlib seaborn`

**2. Ejecución**
Abre el cuaderno `Telecom_X_Modelado.ipynb` en Jupyter o Colab y ejecuta las celdas secuencialmente. El código cargará los datos, entrenará los modelos y generará el archivo final `.pkl`.

---

## Tecnologías Utilizadas
* **Lenguaje:** Python
* **Manipulación de Datos:** Pandas, Numpy
* **Visualización:** Matplotlib, Seaborn
* **Machine Learning:** Scikit-Learn (Modelado, Preprocesamiento, Métricas)
* **Balanceo de Datos:** Imbalanced-learn (SMOTE)
