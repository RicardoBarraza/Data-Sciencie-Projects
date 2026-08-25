# Predicción de Salarios de Beisbolistas de la MLB usando Machine Learning

Este proyecto tiene como objetivo analizar y predecir el salario de jugadores de béisbol de las Grandes Ligas (MLB) en función de sus métricas de rendimiento deportivo individuales y acumuladas a lo largo de su carrera.

---

## 🎯 Problema de Negocio

En el ámbito deportivo profesional, especialmente en la Major League Baseball (MLB), la determinación del salario de un jugador representa una decisión financiera crítica tanto para las directivas de los equipos como para los agentes deportivos. 

El problema central radica en **evaluar objetivamente si la compensación económica de un beisbolista está alineada con su desempeño real en el campo**. 

Las razones principales para realizar este proyecto incluyen:
- **Optimización de Presupuesto:** Proveer a los directivos herramientas cuantitativas para negociar contratos justos y evitar sobrepagar por rendimiento decreciente.
- **Identificación de Talento:** Determinar cuáles métricas individuales (hits, home runs, carreras impulsadas, experiencia) influyen con mayor peso en la valoración económica del jugador.
- **Modelado Predictivo Comparativo:** Evaluar y comparar desde modelos lineales básicos y regularizados hasta algoritmos no lineales y métodos de ensamblado (*Ensemble Learning*), determinando cuál ofrece la mayor precisión predictiva.

---

## 📊 Los Datos y Metodología

### 1. Fuente de Información
Se utilizó el dataset clásico `Hitters` (`hitters.csv`), el cual contiene información sobre el desempeño deportivo de jugadores de béisbol de la MLB durante una temporada regular, así como sus estadísticas acumuladas a lo largo de su carrera.

* **Total de registros iniciales:** 322 jugadores.
* **Atributos (20 variables):**
  * **Estadísticas de la temporada:** `AtBat` (Turnos al bate), `Hits` (Hits logrados), `HmRun` (Home Runs), `Runs` (Carreras anotadas), `RBI` (Carreras impulsadas), `Walks` (Bases por bola).
  * **Estadísticas defensivas:** `PutOuts` (Outs realizados), `Assists` (Asistencias), `Errors` (Errores cometidos).
  * **Experiencia:** `Years` (Años en las Grandes Ligas).
  * **Métricas acumuladas de carrera:** `CAtBat`, `CHits`, `CHmRun`, `CRuns`, `CRBI`, `CWalks`.
  * **Variables categóricas:** `League` (Liga), `Division` (División), `NewLeague` (Liga de la siguiente temporada).
  * **Variable Objetivo (Target):** `Salary` (Salario del jugador en miles de dólares).

---

### 2. Preprocesamiento de Datos y Análisis Exploratorio

1. **Tratamiento de Valores Faltantes:**
   * La variable objetivo `Salary` presentaba 59 valores nulos (`NaN`).
   * Se filtró el dataset para trabajar únicamente con los **263 registros válidos** donde se conoce el salario real.

2. **Análisis y Remoción de Outliers (Valores Atípicos):**
   * Un análisis de distribución mediante diagramas de caja (*Boxplot*) reveló una fuerte asimetría positiva en los salarios, con atípicos extremos (salarios de hasta $2,460,000 USD).
   * Se aplicó la regla del **Rango Intercuartílico (IQR)** para remover datos fuera del límite superior ($Q3 + 1.5 \times IQR$), estableciendo un umbral máximo de truncamiento de $1,590.00$ miles de dólares. Esto redujo la variabilidad extrema y estabilizó la varianza.

3. **Codificación de Variables Categóricas y Escalamiento:**
   * Aplicación de codificación binaria (*Label Encoding*) en variables categóricas (`League`, `Division`, `NewLeague`).
   * Estandarización de características numéricas mediante `StandardScaler` para asegurar la correcta convergencia de los modelos linealmente regularizados (Ridge, Lasso, ElasticNet).

---

## 🤖 Modelos Evaluados

Para abordar la predicción del salario se implementaron y compararon **7 algoritmos de Machine Learning**, divididos en tres familias principales:

### 1. Modelos Lineales y Regularizados
* **Linear Regression (OLS):** Modelo lineal por mínimos cuadrados ordinarios. Sirve como línea base (*baseline*), pero sufre ante problemas de multicolinealidad entre las métricas acumuladas de carrera.
* **Ridge Regression (Regularización L2):** Introduce una penalización proporcional al cuadrado de los coeficientes ($\lambda \sum  eta_j^2$). Reduce la varianza sacrificando un ligero sesgo, manejando eficientemente las variables correlacionadas sin eliminarlas.
* **Lasso Regression (Regularización L1):** Aplica penalización basada en el valor absoluto de los coeficientes ($\lambda \sum | eta_j|$). Realiza selección automática de características al forzar los coeficientes de variables menos relevantes a ser exactamente cero.
* **ElasticNet Regression (L1 + L2):** Combina ambas penalizaciones (L1 y L2) mediante un parámetro de proporción `l1_ratio`. Equilibra la capacidad de selección de variables de Lasso con la estabilidad ante multicolinealidad de Ridge.

### 2. Modelos Basados en Árboles
* **Decision Tree Regressor (CART):** Modelo no lineal que divide el espacio de características en regiones mediante umbrales óptimos. Aunque captura no linealidades sin necesidad de escalar datos, presenta alta varianza y tendencia al sobreajuste (*overfitting*) si no se limita su profundidad.

### 3. Modelos de Ensamblado (Ensemble Learning)
* **AdaBoost Regressor (Adaptive Boosting):** Construye iterativamente una secuencia de estimadores débiles (árboles someros). En cada iteración, repondera las observaciones ajustando el peso de las instancias con mayor error de predicción.
* **Gradient Boosting Regressor (GBM):** Algoritmo secuencial de optimización de funciones de pérdida mediante descenso de gradiente. Cada nuevo árbol ajusta los residuos o errores cometidos por la combinación de árboles anteriores, ofreciendo alta precisión y robustez.

---

## ⚖️ Comparativa de Modelos y Resultados

La evaluación de los modelos se realizó aplicando **Validación Cruzada ($K$-Fold / Repeated K-Fold)** y optimización de hiperparámetros mediante `GridSearchCV`. Las métricas principales evaluadas fueron el **Error Cuadrático Medio (MSE / RMSE)** y el **Coeficiente de Determinación ($R^2$)**.

| Modelo | Tipo de Algoritmo | Manejo de Multicolinealidad | Captura de No Linealidad | Complejidad / Riesgo Overfitting | Rendimiento Predictivo ($R^2$) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Linear Regression** | Lineal Baseline | ❌ Sensible a correlación | ❌ Únicamente relaciones lineales | Bajo sesgo / Alta varianza | Regular / Baseline |
| **Ridge (L2)** | Lineal Regularizado | 🟢 Excelente (Encoge coeficientes) | ❌ Relaciones lineales únicamente | Bajo / Regularización previene overfitting | Bueno |
| **Lasso (L1)** | Lineal Regularizado | 🟡 Regular (Elimina variables) | ❌ Relaciones lineales únicamente | Bajo / Hace selección de datos | Bueno |
| **ElasticNet** | Lineal Regularizado | 🟢 Muy Bueno (Balance L1/L2) | ❌ Relaciones lineales únicamente | Bajo / Control con hiperparámetros | Bueno |
| **Decision Tree** | No Lineal | 🟢 Insensible | 🟢 Alta capacidad no lineal | 🔴 Alto riesgo de sobreajuste | Deficiente a Regular |
| **AdaBoost** | Ensamblado (Boosting) | 🟢 Robusto | 🟢 Alta | 🟡 Moderado | Muy Bueno |
| **Gradient Boosting** | Ensamblado (Boosting) | 🟢 Excelente | 🟢 Muy Alta | 🟡 Requiere ajuste de hiperparámetros | 🏆 **Excelente (Mejor)** |

---

## 📈 Conclusiones Clave

1. **Superioridad del Ensamblado:** El modelo **Gradient Boosting Regressor** obtuvo el mejor desempeño global, superando a los modelos lineales y al árbol de decisión simple al reducir tanto el sesgo como la varianza.
2. **Importancia de la Regularización:** Entre los modelos lineales, **Ridge** y **ElasticNet** superaron significativamente a la Regresión Lineal ordinaria gracias al control de multicolinealidad entre métricas acumuladas (`CAtBat`, `CHits`, `CRuns`, `CRBI`).
3. **Factores Relevantes del Salario:** Las variables de trayectoria y carrera (`CRBI`, `CRuns`, `Years`) mostraron mayor poder predictivo sobre el salario que las métricas obtenidas en una única temporada aislada.

---

## ⚠️ Limitaciones y Pasos Siguientes

### Limitaciones Identificadas:
* **Tamaño Reducido de la Muestra:** Tras eliminar los registros nulos de salario y filtrar atípicos, el conjunto de entrenamiento disponible se redujo a ~200 observaciones.
* **Dataset Histórico:** La base de datos `Hitters` contiene métricas históricas de la MLB que no reflejan la escala salarial actual ni los contratos modernos.
* **Falta de Métricas Avanzadas:** No se cuentan con métricas sabermétricas modernas (*WAR*, *OPS+*, *wRC+*).

### Futuras Mejoras:
1. **Modelos Avanzados de Boosting:** Probar librerías de vanguardia como **XGBoost**, **LightGBM** y **CatBoost**.
2. **Imputación de Datos:** Probar métodos como `KNNImputer` o `IterativeImputer` para aprovechar los 59 registros con salarios nulos.
3. **Despliegue Interactivo:** Construir un dashboard interactivo en **Streamlit** para simular salarios ingresando estadísticas en tiempo real.

---
*Desarrollado como proyecto de análisis y modelado predictivo en Ciencia de Datos.*
