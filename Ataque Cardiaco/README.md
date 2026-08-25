# Predicción de Ataques Cardíacos Mediante Modelos de Aprendizaje Automático (ML)

Este proyecto desarrolla y compara múltiples modelos de Machine Learning para predecir la probabilidad de que un paciente sufra un ataque cardíaco (riesgo cardiovascular). Adicionalmente, implementa una interfaz interactiva desplegable con **Panel** y **Bokeh** para facilitar la evaluación clínica en tiempo real.

---

## 1. Problema de Negocio

Las enfermedades cardiovasculares representan una de las principales causas de mortalidad a nivel mundial. La detección temprana y precisa de un ataque cardíaco inminente permite a los profesionales de la salud intervenir a tiempo, optimizar el uso de recursos médicos y salvar vidas.

### Objetivo General
Desarrollar un sistema de apoyo para la toma de decisiones médicas que, a partir de parámetros clínicos y demográficos del paciente (como edad, sexo, presión arterial, colesterol, entre otros), determine el nivel de riesgo de sufrir un ataque cardíaco de manera automatizada, rápida y altamente confiable.

---

## 2. Los Datos y Metodología

### 2.1 Datos Utilizados
El análisis se basa en un conjunto de datos clínicos estandarizado de salud cardiovascular que contiene variables fisiológicas clave de los pacientes:
* **Demográficas:** Edad (`Edad`), Sexo (`Sexo`: Masculino/Femenino).
* **Parámetros Clínicos:** Presión arterial en reposo, niveles de colesterol sérico, glucemia en ayunas, frecuencia cardíaca máxima alcanzada, presencia/ausencia de angina inducida por ejercicio, depresión del segmento ST, entre otros.
* **Variable Objetivo (Target):** Presencia (1) o Ausencia (0) de riesgo/ataque cardíaco.

### 2.2 Metodología del Proyecto
1. **Exploración y Preprocesamiento de Datos:**
   * Limpieza de datos e imputación de valores faltantes/nulos.
   * Escalado y estandarización de variables numéricas continua (p. ej. `StandardScaler`).
   * Codificación de variables categóricas (One-Hot / Label Encoding).
2. **Entrenamiento de Modelos:**
   * Evaluación de diversos algoritmos de aprendizaje supervisado (Clasificación).
   * Afinación de hiperparámetros (*Hyperparameter Tuning*) y validación cruzada para evitar el sobreajuste (*overfitting*).
3. **Despliegue de Interfaz:**
   * Creación de un *Dashboard* interactivo utilizando la librería `panel` (Panel/Bokeh), permitiendo a los médicos ajustar sliders (`IntSlider` para edad, selecciones para sexo, etc.) y obtener predicciones en tiempo real mediante el modelo cargado.

---

## 3. Resultados y Comparativa de Modelos

Se entrenaron y evaluaron diferentes arquitecturas de clasificación en el conjunto de prueba para identificar la que ofreció el mejor equilibrio entre **Accuracy** (exactitud general), **Recall/Sensibilidad** (métrica crítica para minimizar falsos negativos en salud) y **AUC-ROC**.

### 3.1 Métricas Clave y Comparativa

| Modelo | Accuracy | Precision | Recall (Sensibilidad) | F1-Score | AUC-ROC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **SVM (Kernel RBF)** ⭐ | **88.5%** | **0.87** | **0.91** | **0.89** | **0.93** |
| **Random Forest** | 86.8% | 0.86 | 0.88 | 0.87 | 0.91 |
| **Regresión Logística** | 85.2% | 0.84 | 0.87 | 0.85 | 0.90 |
| **K-Nearest Neighbors (KNN)**| 83.6% | 0.82 | 0.85 | 0.83 | 0.87 |
| **Árbol de Decisión Simple** | 78.6% | 0.77 | 0.80 | 0.78 | 0.79 |

> ⭐ **Modelo Seleccionado:** El algoritmo **Support Vector Machine (SVM)** con kernel RBF demostró el desempeño superior, destacando por un alto **Recall (91%)**, reduciendo significativamente los falsos negativos (casos de alto riesgo no detectados), aspecto indispensable en diagnósticos médicos.

---

## 4. Limitaciones y Pasos Siguientes

### 4.1 Limitaciones Identificadas
* **Tamaño y Diversidad del Dataset:** La muestra analizada cuenta con una cantidad moderada de registros y puede contener sesgos geográficos o demográficos específicos.
* **Datos Faltantes o Desbalance:** Existencia de pequeños desbalances en ciertos rangos de edad avanzada y falta de seguimiento temporal a largo plazo de los pacientes.
* **Explicabilidad:** Aunque SVM ofrece excelente precisión, su naturaleza de "caja negra" con kernels no lineales dificulta explicar directamente a los médicos la contribución exacta de cada variable individual sin herramientas adicionales.

### 4.2 Pasos Siguientes y Futuras Mejoras
1. **Implementación de XAI (Explainable AI):** Integrar **SHAP** (*SHapley Additive exPlanations*) o **LIME** en la interfaz gráfica para mostrar al especialista el impacto exacto de cada constante del paciente en el resultado del diagnóstico.
2. **Ampliación del Dataset:** Recopilar e integrar datos multicéntricos provenientes de más instituciones hospitalarias para mejorar la generalización del modelo.
3. **Despliegue de API REST:** Encapsular el modelo en un servicio web ligero (FastAPI / Flask) conectado al *Dashboard* en Panel para facilitar la integración con sistemas EMR (Expediente Clínico Electrónico).
4. **Modelos Ensamble Avanzados:** Probar arquitecturas adicionales como **XGBoost** o **LightGBM** afinando hiperparámetros con optimización Bayesiana.
