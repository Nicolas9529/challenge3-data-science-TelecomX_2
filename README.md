# 📉 Proyecto Telecom X: Análisis y Predicción de Fuga de Clientes (Churn)

## 🎯 Propósito y Descripción del Proyecto
El objetivo principal de este proyecto de Data Science es desarrollar un modelo predictivo capaz de anticipar la cancelación de servicios (Churn) de los clientes de la empresa "Telecom X". A través de la aplicación del método científico, limpiamos datos históricos, analizamos comportamientos y entrenamos modelos de Machine Learning (optimizados con Validación Cruzada) para identificar a los usuarios con mayor riesgo de abandono. Esto permite a la gerencia diseñar estrategias de retención proactivas basadas en evidencia.

---

## 📂 Estructura del Proyecto
El repositorio está organizado de la siguiente manera para facilitar su reproducibilidad:

```text
📦 challenge3-data-science-TelecomX_2
 ┣ 📂 data
 ┃ ┗ 📜 datos_tratados.csv        # Dataset limpio y preprocesado (obtenido en la Parte 1)
 ┣ 📂 notebooks
 ┃ ┗ 📜 Telecom_X_Modelado.ipynb  # Cuaderno principal con el código de Machine Learning
 ┣ 📂 images                      # Visualizaciones exportadas del EDA y modelos
 ┃ ┣ 🖼️ matriz_correlacion.png
 ┃ ┣ 🖼️ eda_boxplots.png
 ┃ ┗ 🖼️ feature_importance.png
 ┣ 📜 modelo_churn_produccion.pkl # Modelo final serializado listo para producción
 ┗ 📜 README.md                   # Documentación del proyecto

⚙️ Preparación de Datos y Metodología (Enfoque Científico)
Fase 1: Limpieza y Exploración (EDA)
Preprocesamiento: Tratamiento de valores nulos, corrección de formatos y eliminación de variables sin poder predictivo (ej. identificadores únicos).

Insights del EDA: A través de Boxplots y Matrices de Correlación, observamos que los clientes que se fugan tienen una mediana de tiempo en la empresa (Tenure) drásticamente menor (aprox. 10 meses vs 38 meses) y cargos mensuales significativamente más altos.

Fase 2: Ingeniería de Características y Balanceo
Codificación: Transformación de la variable objetivo a binario con LabelEncoder y variables categóricas nominales (ej. Método de Pago) con OneHotEncoder(drop='if_binary').

División (Train/Test Split): Separación en Entrenamiento (80%) y Prueba (20%) estratificando la variable objetivo para evitar la fuga de datos (Data Leakage).

Estandarización y SMOTE: Se aplicó StandardScaler a las variables numéricas y balanceo sintético (SMOTE) exclusivamente al set de entrenamiento para resolver el desbalance de clases y permitir un aprendizaje equitativo.

🧠 Modelado, Experimentación y Justificaciones
Evaluamos de forma progresiva diferentes algoritmos para establecer el valor del Machine Learning:

Línea Base (Dummy Classifier): Predijo siempre la clase mayoritaria (retención). Obtuvo un Recall de 0% para la fuga, demostrando que predecir "al azar" es inútil operativamente.

Random Forest Base: Nuestro modelo inicial no paramétrico. Logró una Exactitud del 79%, pero un Recall del 50%, lo que evidenció un sobreajuste (overfitting) a la clase mayoritaria.

Random Forest Optimizado (GridSearchCV): Aplicamos Validación Cruzada Estratificada (K-Fold). Justificación: Al podar la profundidad del árbol (max_depth=5), forzamos al modelo a generalizar mejor, elevando el Recall al 73%.

Regresión Logística: Modelo lineal paramétrico sensible a la escala (justificando nuestra estandarización previa). Justificación Final: Fue seleccionado como el modelo óptimo al lograr el mejor Recall (79%). En problemas de Churn, es vital minimizar los Falsos Negativos (clientes que se van sin ser detectados).

🎯 Conclusiones y Estrategias Basadas en Evidencia
Basados en la Extracción de Importancia (Feature Importance) y los Coeficientes de la Regresión Logística, propusimos las siguientes estrategias:

Migración de Contratos: El contrato mensual (Month-to-month) es el mayor riesgo. Estrategia: Ofrecer mejoras en el servicio a quienes acepten pasar a un contrato anual.

Fomento del Pago Automático: El pago mediante "Electronic check" presenta alta fricción. Estrategia: Ofrecer un descuento permanente (ej. 5%) en la factura a quienes domicilien su pago a tarjeta de crédito.

Programa de Onboarding: Dado que la retención aumenta con el tiempo acumulado (Tenure), estrategia: Implementar soporte proactivo durante los primeros 6 meses críticos.

Alerta Temprana en Call Center: Estrategia: Desplegar el modelo serializado (.pkl) en los sistemas de atención al cliente para habilitar ofertas de retención en tiempo real cuando se detecte un alto riesgo de fuga.

🚀 Instrucciones para Ejecutar el Proyecto
1. Requisitos e Instalación
Asegúrate de contar con Python 3.8+ e instala las dependencias:

Bash
pip install pandas numpy scikit-learn imbalanced-learn matplotlib seaborn
2. Carga de Datos y Ejecución
No es necesario descargar archivos locales. El cuaderno Telecom_X_Modelado.ipynb está configurado para leer el dataset limpio directamente desde este repositorio. Simplemente abre el cuaderno y ejecuta las celdas de arriba hacia abajo para replicar el EDA, entrenar los modelos y generar el archivo de producción .pkl.

🛠️ Tecnologías Utilizadas
Lenguaje: Python

Manipulación y Análisis: Pandas, Numpy

Visualización: Matplotlib, Seaborn

Machine Learning: Scikit-Learn (Pipelines, GridSearchCV, LogisticRegression, RandomForest)

Manejo de Desbalanceo: Imbalanced-learn (SMOTE)
