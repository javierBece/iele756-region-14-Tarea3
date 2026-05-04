# Análisis Integrado de Determinantes Sociales y Salud: Región Metropolitana (Chile) 📊⚕️

**Repositorio:** [iele756-region-14-Tarea3](https://github.com/javierBece/iele756-region-14-Tarea3)  
**Asignatura:** IELE756 -- Preparación y Análisis de Datos  
**Profesor:** Leo Ferres, PhD  
**Integrantes:** Javier Becerra Muñoz, Jose Pino Muñoz (Grupo 1)  
**Comunas Asignadas:** La Granja, Macul, San Ramón  

---

## 🎯 Resumen del Proyecto
Este es el proyecto analítico final del curso. El objetivo principal es vincular tres tablas maestras a nivel comunal de la Región Metropolitana (Censo 2024, registros de salud ENO y registros de egresos hospitalarios GRD) en un único dataset analítico. A partir de esto, modelamos cómo las vulnerabilidades territoriales —específicamente la educación y la migración— influyen en el riesgo de enfermedad y la gestión hospitalaria.

## 🗂️ Preparación de Datos y Sanity Checks
Se realizó un *pooling* de los datos de todos los equipos del curso. 
*   **Limpieza de Datos:** Tras filtrar archivos malformados (Equipo 07) y anomalías estadísticas severas (como tasas de población extranjera lógicamente imposibles), consolidamos un dataset analítico final de **N = 45 comunas**.
*   **Variables Derivadas:** Se calcularon indicadores clave como `log_pop_total`, `pct_unemployed` (corrigiendo la escala inversa a 100), `schooling_gap` (brecha educativa entre chilenos y extranjeros) y `age_gap`.

---

## 📈 Hallazgos Principales (Exploratory & Modeling)

### 1. Análisis Exploratorio (Correlaciones y Outliers)
La matriz de correlación reveló dinámicas territoriales importantes:
*   Una correlación inversa contraintuitiva entre la **tasa de dependencia y el desempleo** (-0.51).
*   Una relación positiva lógica entre los **egresos hospitalarios (GRD) y la dependencia** (0.42).
*   Una correlación positiva entre la **población extranjera y la brecha de escolaridad** (0.38).

En el análisis de dispersión, comunas como **San Pedro** (ENO) y **Renca, San Miguel y Peñalolén** (GRD) destacaron como *outliers* extremos, sugiriendo dinámicas no capturadas por la demografía pura.

### 2. El "Espejismo de Poisson" y Modelamiento ENO
Al modelar las Enfermedades de Notificación Obligatoria (ENO):
*   Un modelo de regresión de Poisson ingenuo sugirió falsamente que la migración (`pct_foreign`) y el desempleo (`pct_unemployed`) eran predictores significativos.
*   Sin embargo, se detectó una **sobredispersión masiva** (Estadístico: 251.62). 
*   Al ajustar un **Modelo Binomial Negativo**, estas variables perdieron su significancia. El **Forest Plot** confirmó que la única variable sociodemográfica robusta para predecir el riesgo epidemiológico en la RM es la **brecha de escolaridad** (`schooling_gap`, p=0.0027).

### 3. Modelamiento OLS Hospitalario (GRD)
Modelamos el promedio de días de estadía hospitalaria (`grd_mean_los`) mediante regresión lineal múltiple (OLS).
*   **Resultado:** El modelo arrojó un **$R^2$ de 0.089**, con ninguna covariable demográfica resultando estadísticamente significativa.
*   **Conclusión:** La gestión de camas y la duración de la estadía en los hospitales responden a factores sistémicos y clínicos, siendo independientes del código postal o demografía de los pacientes.

---

## 🗺️ Análisis Espacial (GeoPandas)
Integramos datos espaciales para mapear el comportamiento del Modelo Binomial Negativo:
*   **Tasas Predichas:** Se generó un mapa de coropletas ilustrando la tasa predicha de ENO a nivel espacial.
*   **Análisis de Residuos:** Se generó un mapa de residuos estandarizados (Pearson). La comuna rural de **San Pedro** se reveló como el residuo positivo extremo (**8.52**), demostrando el efecto *Small-N sparsity*, donde un brote localizado en poblaciones muy pequeñas rompe las predicciones del modelo. **María Pinto** destacó como el residuo negativo extremo (**-1.24**).

---

## 📂 Estructura del Repositorio

El repositorio está organizado de la siguiente manera:
```text
📦 iele756-region-14-Tarea3
 ┣ 📂 Datasets
 ┃ ┗ 📂 Mapa comuna            # Archivos cartográficos
 ┃   ┣ 📜 comunas.shp          # Shapefile principal de las comunas RM
 ┃   ┣ 📜 comunas.shx
 ┃   ┣ 📜 comunas.dbf
 ┃   ┗ 📜 comunas.prj
 ┣ 📂 shared_data              # Directorio con los CSVs extraídos de todos los grupos
 ┃ ┣ 📂 team01
 ┃ ┣ 📂 team02
 ┃ ┗ 📜 ... (21 carpetas por equipo con datos Censo, ENO, GRD)
 ┣ 📜 Tarea3.ipynb             # Notebook principal con el análisis, modelos y mapas
 ┣ 📜 analytical_dataset_tarea3_vars.csv # Dataset final consolidado y limpio (N=45)
 ┣ 📜 requirements.txt         # Lista de dependencias y librerías de Python
 ┣ 📜 README.md                # Este archivo de documentación
 ┗ 📜 FAMILY.zip               # Archivo comprimido original con los datos del curso
