# Análisis Integrado de Determinantes Sociales y Salud: Región Metropolitana (Chile)

## Resumen del Proyecto
Este proyecto representa la tercera etapa de una línea de investigación diseñada para comprender la relación entre los determinantes sociales y los resultados de salud pública en la Región Metropolitana de Santiago. Mediante la integración de datos microcensales del **Censo 2024**, registros epidemiológicos de **Enfermedades de Notificación Obligatoria (ENO)** y datos de egresos hospitalarios (**GRD**), modelamos cómo las vulnerabilidades territoriales —específicamente la educación y la migración— influyen en el riesgo de enfermedad y la gestión hospitalaria.

El estudio se centra en tres comunas específicas: **La Granja, Macul y San Ramón**.

---

## Conjuntos de Datos
*   **Datos Demográficos:** Microdatos del Censo 2024 (Vivienda, Hogar, Persona).
*   **Vigilancia Epidemiológica (ENO):** Registros de enfermedades de notificación obligatoria (2007–2024)[cite: 1].
*   **Actividad Hospitalaria (GRD):** Registros de Grupos Relacionados por Diagnóstico, con foco en el promedio de días de estadía (Length of Stay - LOS)[cite: 1].
*   **Datos Cartográficos:** Archivos Shapefile de las comunas de la Región Metropolitana.

---

## Metodologías Clave y Hallazgos

### 1. Modelamiento Estadístico de Enfermedades (ENO)
Implementamos una regresión cruzada para predecir el conteo de notificaciones ENO basado en la demografía comunal.
*   **De Poisson a Binomial Negativa:** Los modelos iniciales de Poisson mostraron una sobredispersión masiva (Estadístico de dispersión: **251.62**), revelando un "espejismo de Poisson" donde variables como el desempleo parecían falsamente significativas.
*   **Hallazgos Robustos:** El modelo Binomial Negativo corrigió estos errores, revelando que solo la **brecha de escolaridad** (`schooling_gap`, p=0.0027) es un predictor robusto del riesgo. La nacionalidad y el desempleo perdieron significancia tras ajustar la varianza.

### 2. Modelamiento Lineal de Estadía Hospitalaria (GRD)
Modelamos el promedio de días de estadía (`grd_mean_los`) mediante Mínimos Cuadrados Ordinarios (OLS).
*   **Resultados:** El modelo arrojó un bajo **R-cuadrado de 0.089**, indicando que la demografía comunal explica menos del 10% de la duración de la estadía hospitalaria.
*   **Diagnóstico:** Los residuos superaron las pruebas de normalidad (Jarque-Bera p = 0.82), confirmando que, aunque el modelo es estadísticamente válido, las variables clínicas (y no las demográficas) son las que realmente determinan la estadía.

### 3. Análisis Espacial y de Residuos
Utilizando **GeoPandas**, generamos visualizaciones territoriales para identificar desajustes entre el modelo y la realidad observada.
*   **Tasas Predichas:** Mapas de coropletas que muestran la distribución esperada de ENO basada en niveles educativos.
*   **Análisis de Residuos:** Identificamos a **San Pedro** como un valor atípico significativo (Residuo estandarizado: **8.52**), evidenciando el efecto de "vulnerabilidad por población pequeña", donde bajas poblaciones inflan artificialmente las tasas per cápita.

---

## Síntesis Integrada
El proyecto demuestra que la Región Metropolitana está profundamente fragmentada. El eje dominante de variación es la vulnerabilidad socioeconómica (educación) en lugar del tamaño poblacional.

Mientras que los riesgos de salud pública (ENO) están estrechamente vinculados a las brechas educativas comunales, la dinámica hospitalaria (GRD) es mayormente independiente de la demografía del territorio. Esto sugiere que la **salud preventiva** debe ser focalizada territorialmente, mientras que la **gestión hospitalaria** actúa como un proceso clínico centralizado que trasciende las fronteras comunales.

---

## Estructura del Repositorio
*   `Tarea3_Modelamiento_Espacial.ipynb`: Notebook principal con código, diagnósticos y mapas.
*   `analytical_dataset_tarea3_vars.csv`: Conjunto de datos integrado utilizado para el modelamiento.
*   `requirements.txt`: Librerias principales utilizadas en el proyecto.
*   `FAMILY.zip`: Archivo zip con los archivos CVs que son procesados dentro del proyecto.
*   `Datasets/`: Carpeta con datos crudos/procesados y shapefiles. (muy extensa para ser subida)

## Requisitos Técnicos
*   **Lenguaje:** Python 3.13+
*   **Librerías:** `pandas`, `statsmodels`, `geopandas`, `matplotlib`, `seaborn`.

---

**Autores:** Jose Pino & Javier Becerra  
**Curso:** Preparación y Análisis de Datos  
**Fecha:** Mayo 2026
