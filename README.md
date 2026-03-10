# 📉 Proyecto Telecom X: Análisis y Predicción de Fuga de Clientes (Churn)

## 📖 Propósito y Descripción del Proyecto
El objetivo principal de este proyecto es desarrollar un modelo predictivo capaz de anticipar la cancelación de servicios (Churn) de los clientes de la empresa "Telecom X". A través del método científico, limpiamos datos históricos, analizamos comportamientos y entrenamos modelos de Machine Learning para identificar proactivamente a los usuarios con mayor riesgo de abandono. Esto permite a la gerencia diseñar estrategias de retención basadas en evidencia.

---

## 📂 Estructura del Proyecto
El repositorio está organizado de la siguiente manera para facilitar su reproducibilidad:

📦 challenge3-data-science-TelecomX_2
 ┣ 📂 data
 ┃ ┗ 📜 datos_tratados.csv        # Dataset limpio y preprocesado
 ┣ 📂 notebooks
 ┃ ┗ 📜 Telecom_X_Modelado.ipynb  # Cuaderno principal con el código
 ┣ 📂 images                      # Visualizaciones exportadas del EDA
 ┃ ┣ 🖼️ matriz_correlacion.png
 ┃ ┣ 🖼️ eda_boxplots.png
 ┃ ┗ 🖼️ feature_importance.png
 ┣ 📜 modelo_churn_produccion.pkl # Modelo final serializado
 ┗ 📜 README.md                   # Documentación del proyecto

---

## 🔬 Metodología (Enfoque Científico)

### Fase 1: Observación y Preprocesamiento (EDA)
* **Limpieza de Datos:** Tratamiento de valores nulos, corrección de formatos y eliminación de variables sin poder predictivo (ej. CustomerID).
* **Análisis Exploratorio (EDA):** Identificamos visualmente patrones preliminares. Observamos mediante Boxplots que variables como el tiempo en la empresa (`Tenure`) y los costos (`Charges`) presentan distribuciones drásticamente distintas entre los clientes que se fugan y los que se quedan.

### Fase 2: Experimentación y Modelado
* **Preparación de Datos:** * Se aplicó codificación (`LabelEncoder` y `OneHotEncoder`) para transformar variables categóricas en numéricas.
  * Los datos se dividieron en Entrenamiento (80%) y Prueba (20%) de forma estratificada.
  * Se aplicó **SMOTE** únicamente al conjunto de entrenamiento para balancear la clase minoritaria (fugas).
  * Se estandarizaron las numéricas con `StandardScaler`.
* **Modelos Entrenados:** Evaluamos una progresión algorítmica desde una línea base hasta modelos complejos.

---

## 📊 Resultados y Justificaciones de Modelado

La evaluación en el conjunto de prueba arrojó la siguiente evolución:

1. **Línea Base (Dummy Classifier):** Obtuvo un Recall de 0% para la fuga. Predijo que nadie se iría, demostrando que predecir la clase mayoritaria es inútil para el negocio.
2. **Random Forest Base:** Logró una Exactitud global (79%), pero presentó un Recall bajo (50%), evidenciando un ligero sobreajuste (overfitting).
3. **Random Forest Optimizado (GridSearchCV):** Al aplicar Validación Cruzada (K-Fold) y limitar la profundidad del árbol (`max_depth=5`), forzamos al modelo a generalizar mejor, elevando el Recall al **73%**.
4. **Regresión Logística:** Aunque su Exactitud global fue menor (74%), demostró un desempeño superior en la métrica más crítica, obteniendo un **Recall del 79%** en la detección de fuga.

**Selección Final:** Bajo la premisa de negocio de que un "Falso Negativo" (perder a un cliente por no detectarlo) es más costoso que un "Falso Positivo", se seleccionó y serializó la **Regresión Logística** como el modelo óptimo.

---

## 🎯 Conclusiones y Estrategias Basadas en Evidencia

El análisis de importancia de variables confirmó los siguientes impulsores de fuga y retención, sobre los cuales proponemos 4 estrategias:

1. **Migración de Contratos:** El contrato mensual (`Month-to-month`) es el principal riesgo. *Estrategia:* Ofrecer mejoras de servicio a clientes que acepten pasar a formato anual.
2. **Fomento del Pago Automático:** El pago con cheque electrónico presenta alta correlación con la cancelación. *Estrategia:* Ofrecer un descuento permanente en la factura a quienes domicilien su pago a tarjeta de crédito.
3. **Programa de Onboarding:** La fidelidad se construye con el tiempo (`Tenure`). *Estrategia:* Implementar soporte proactivo y llamadas de control durante los primeros 6 meses.
4. **Alerta Temprana en Call Center:** *Estrategia:* Implementar el modelo serializado (`.pkl`) en los sistemas de atención al cliente para habilitar ofertas de retención en tiempo real.

---

## 🚀 Instrucciones para Ejecutar el Proyecto

**1. Requisitos e Instalación**
Asegúrate de tener un entorno de Python 3.8+ y ejecuta en tu terminal:
pip install pandas numpy scikit-learn imbalanced-learn matplotlib seaborn

**2. Ejecución**
No necesitas descargar archivos locales. Abre el cuaderno `Telecom_X_Modelado.ipynb` en Jupyter o Colab y ejecuta las celdas secuencialmente. El código descargará los datos limpios de este repositorio, entrenará los modelos y generará el archivo final `.pkl`.

---

## 🛠️ Tecnologías Utilizadas
* **Lenguaje:** Python
* **Manipulación y Visualización:** Pandas, Numpy, Matplotlib, Seaborn
* **Machine Learning:** Scikit-Learn
* **Balanceo de Datos:** Imbalanced-learn (SMOTE)
