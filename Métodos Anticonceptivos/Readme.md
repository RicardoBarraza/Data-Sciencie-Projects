
## Resumen del Proyecto: Selección de Método Anticonceptivo

### Metodología
El proyecto se centró en predecir el uso de métodos anticonceptivos en mujeres casadas en Indonesia basándose en datos demográficos y socioeconómicos. Los pasos principales incluyeron:
1. **Análisis Exploratorio (EDA):** Cálculo de intervalos de confianza para la edad promedio (32.53 años) y número de hijos (3.26).
2. **Preprocesamiento:** Transformación de variables categóricas mediante `OneHotEncoder` y recodificación de la variable objetivo a un problema binario (Uso vs. No uso).
3. **Modelado y Validación:** 
   - Uso de **AdaBoost** y **Gradient Boosting**.
   - Implementación de **Validación Cruzada Anidada** para asegurar la generalización.
   - Aplicación de técnicas de balanceo de datos como **SMOTE-Tomek** para mitigar el desequilibrio de clases.

### Conclusiones y Métricas Clave
- **Exactitud (Accuracy):** El modelo de Gradient Boosting optimizado mediante `GridSearchCV` alcanzó una exactitud promedio de **72.4%** en validación cruzada anidada.
- **Estabilidad:** La mínima diferencia entre la validación cruzada estándar y la anidada sugiere que el modelo no presenta sobreajuste (overfitting).
- **Impacto del Balanceo:** Aunque el balanceo con SMOTE-Tomek no aumentó drásticamente la exactitud global (0.715), proporciona un modelo más robusto para identificar ambas clases equitativamente.

### Limitaciones
- **Temporalidad:** Los datos datan de 1987, lo que puede no reflejar las dinámicas sociales, educativas y de salud actuales.
- **Atributos:** El modelo actual tiene un techo de exactitud cercano al 73%. Podría beneficiarse de variables adicionales como acceso a servicios de salud o ingresos económicos más detallados.
- **Clasificación Binaria:** Al simplificar el problema a 'Uso' vs 'No uso', se pierde la capacidad de distinguir entre métodos de corto y largo plazo, lo cual era un objetivo inicial.
```
