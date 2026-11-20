# Análisis Exploratorio de Datos (EDA) - Dataset de Propinas (Tips)
Este repositorio contiene un análisis estadístico y visual detallado sobre el comportamiento del consumo y las propinas de los clientes de un restaurante, utilizando herramientas de Python como Pandas, Seaborn y Matplotlib.
El dataset tips combina variables numéricas y categóricas en un formato limpio de 244 registros (filas) y 7 columnas:
Estructura de variables de tips
•	Variables Numéricas (3):
o	total_bill: Total de la cuenta en dólares (float).
o	tip: Monto de la propina en dólares (float).
o	size: Cantidad de comensales en la mesa (int).
•	Variables Categóricas (4):
o	sex: Género de quien pagó la cuenta (Male, Female).
o	smoker: Si había fumadores en la mesa (Yes, No).
o	day: Día de la semana (Thur, Fri, Sat, Sun).
o	time: Momento del día (Lunch, Dinner).

## 📌 Origen de los Datos
Los datos analizados en este proyecto provienen del dataset público e histórico de propinas de la librería **Seaborn** (`tips.csv`), el cual recopila información real sobre consumos en un restaurante en la década de 1990.
* **Fuente oficial:** [Seaborn Data Repository (mwaskom)](https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv)
## 📊 Distribución y Asimetría
Es crucial evitar confusiones teóricas comunes al analizar la forma de las distribuciones:
•	Total de la Cuenta (`total_bill`): Presenta una Asimetría Positiva (sesgo a la derecha)con un coeficiente de `1.1332`. Esto indica que la cola de la distribución se extiende hacia la derecha (valores altos), mientras que la gran mayoría de las cuentas se acumulan en montos menores.
•	Propinas (`tip`): Presenta de igual manera una marcada Asimetría Positiva(`1.4655`), confirmando que la mayoría de los clientes deja una propina estándar de entre $2 y $3 USD, existiendo pocos casos excepcionales de alta generosidad.

## 🛡️ Tratamiento de Outliers (Valores Atípicos)
Tras aplicar el método estadístico del Rango Intercuartílico (IQR) y contrastarlo con el análisis visual de los gráficos de caja (Boxplot) y violín (Violin Plot), se identificaron:
•	9 outliers** en `total_bill` (valores mayores a $40.30 USD).
•	9 outliers** en `tip` (valores mayores a $5.91 USD).

### 💡 Decisión Tomada sobre los Outliers:
**No eliminarlos ni aplicar transformaciones drásticas (como logaritmos) para el análisis general.**
Justificación basada en el Análisis Visual:**
1. **Son outliers legítimos:** Las gráficas de caja y violín muestran un comportamiento de dispersión continuo y natural hacia la parte superior. No hay indicios de errores de digitalización o carga de datos (por ejemplo, cuentas de $1,000 USD o valores negativos).
2. **Importancia para el negocio:** Estos outliers representan a los clientes de mayor valor para el restaurante (mesas grandes, eventos o consumo premium). Eliminarlos sesgaría negativamente la realidad económica del negocio, ocultando la existencia de este segmento de alta facturación.
3. **Recomendación:** Se sugiere mantener estos registros en su forma original para la toma de decisiones comerciales, o bien, si se fuese a construir un modelo predictivo lineal sensible a extremos, crear una segmentación para tratarlos como un subgrupo o usar algoritmos robustos a outliers (como árboles de decisión).

## Justificación Técnica para la Conservación de Outliers

En la estadística aplicada y la ciencia de datos, la eliminación automática de outliers es una práctica desaconsejada a menos que exista una justificación metodológica estricta. A continuación se presentan los argumentos técnicos para mantener los valores atípicos en este dataset:

### 1. Ausencia de Errores de Medición o Registro (Outliers Legítimos)
* **Fundamento:** Un valor atípico debe eliminarse si proviene de un error instrumental, fallas en la captura de datos o ruido térmico. 
* **Aplicación:** En este dataset, una cuenta de $50.81 USD o una propina de $10.00 USD son perfectamente viables y realistas para la gastronomía. No representan errores tipográficos (como un valor de $5,000 USD por un cero extra), por lo que eliminarlos equivaldría a alterar artificialmente datos reales.

### 2. Representación del Comportamiento Real del Negocio
* **Fundamento:** En economía y negocios, la distribución de ingresos a menudo sigue leyes de potencia (como el principio de Pareto) en lugar de una distribución normal.
* **Aplicación:** Los clientes que consumen montos elevados (`total_bill` > $40) representan el segmento de mayor valor (VIP o mesas grandes). Eliminar estos datos subestimaría la capacidad de facturación máxima del restaurante y sesgaría los análisis de ingresos reales.

### 3. Evitar el Sesgo de Selección y la Subestimación de la Varianza
* **Fundamento:** La eliminación de colas reduce artificialmente la desviación estándar y el rango de la muestra, induciendo un sesgo de selección que invalida las pruebas de hipótesis posteriores.
* **Aplicación:** Si eliminamos los 9 outliers de propinas, estaríamos reportando un comportamiento del cliente artificialmente homogéneo, ocultando la verdadera dispersión y variabilidad del comportamiento de gratificación.

### 4. Alternativas de Modelado Robustas
* **Fundamento:** En lugar de modificar los datos para que se adapten a un modelo sensible (como la Regresión Lineal por Mínimos Cuadrados), la estadística moderna propone adaptar el modelo a los datos.
* **Aplicación:** Si se requiere realizar modelos predictivos, la decisión técnica óptima es utilizar algoritmos robustos a outliers (como *Random Forest*, regresión de Huber, o transformaciones de escala no lineales) en lugar de desechar información real y valiosa.`
