# TB1 - Fundamentos de Data Science: Hotel Booking Demand

<div align="center">

**Universidad Peruana de Ciencias Aplicadas (UPC)**  
**Carrera de Ciencias de la Computación / Ingeniería de Software**  
**Curso:** Fundamentos de Data Science (1ACC0216) - NRC 4878  
**Docente:** Nérida Isabel Manrique Tunque  
**Grupo:** 4 | **Ciclo:** 2026-02  

</div>

---

## 👥 Integrantes del Grupo 4
* **Diaz Miñano, Salvador Arturo** 
* **Nuñez Quispe, Mathias David** 
* **Ruiz Soto, Diego Gilmer** 
* **Yparraguirre Torres, Javier Andre** 

---

## 📋 Tabla de Contenidos
1. [Descripción del Proyecto y Dataset](#1-descripción-del-proyecto-y-dataset)
2. [Contexto y Preguntas Analíticas de Negocio](#2-contexto-y-preguntas-analíticas-de-negocio)
3. [Auditoría de Calidad y Limpieza de Datos](#3-auditoría-de-calidad-y-limpieza-de-datos)
4. [Análisis Exploratorio de Datos (EDA) y Hallazgos](#4-análisis-exploratorio-de-datos-eda-y-hallazgos)
5. [Conclusiones y Recomendaciones Estratégicas](#5-conclusiones-y-recomendaciones-estratégicas)
6. [Estructura del Repositorio](#6-estructura-del-repositorio)
7. [Ejecución del Código en RStudio](#7-ejecución-del-código-en-rstudio)
8. [Referencias](#8-referencias)

---

## 1. Descripción del Proyecto y Dataset

El presente proyecto corresponde al **Trabajo Parcial (TB1)** del curso **Fundamentos de Data Science**. Su objetivo principal es realizar un Análisis Exploratorio de Datos (EDA) riguroso sobre el conjunto de datos **Hotel Booking Demand**, aplicando una metodología técnica sistemática en **R y RStudio**.

El conjunto de datos original recopila información de **119,390 reservas** de dos establecimientos hoteleros en Portugal entre julio de 2015 y agosto de 2017:
* **City Hotel:** 79,330 reservas (66.45%)
* **Resort Hotel:** 40,060 reservas (33.55%)

El dataset consta de **32 variables** que describen el proceso de reserva, la estancia del huésped, los canales de comercialización, las solicitudes operativas y el estado final de la reserva (`is_canceled`).

---

## 2. Contexto y Preguntas Analíticas de Negocio

El análisis responde a las **dos preguntas de negocio asignadas al Grupo 4**, enfocadas en optimizar la gestión operativa, comercial y de Revenue Management:

### 🎯 Pregunta de Negocio 7 (Estacionamiento)
> *¿Qué tan frecuente es la solicitud de estacionamiento y qué características presentan esas reservas?*
* **Pregunta Analítica 7:** ¿Cuál es la proporción y frecuencia de reservas que solicitan espacios de estacionamiento (`required_car_parking_spaces`), y cómo se distribuyen estas solicitudes según el tipo de hotel (`hotel`), la duración total de la estancia (`stays_in_weekend_nights` + `stays_in_week_nights`) y el estado final de cancelación (`is_canceled`)?

### 🎯 Pregunta de Negocio 8 (Patrones por Canal, Depósito y Solicitudes)
> *¿Qué patrones se observan en las reservas según canal de distribución, tipo de depósito o solicitudes especiales?*
* **Pregunta Analítica 8:** ¿Cómo varía la distribución del volumen de reservas y la tasa de cancelación al cruzar los canales de distribución (`distribution_channel`), los tipos de depósito (`deposit_type`) y el número total de solicitudes especiales (`total_of_special_requests`)?

---

## 3. Auditoría de Calidad y Limpieza de Datos

Siguiendo las dimensiones de calidad de datos exigidas por el curso, se ejecutó la siguiente auditoría sobre el dataset original:

1. **Unicidad:** Se identificaron **31,994 registros duplicados exactos** (26.80% del total). Tras su remoción, se consolidó el dataset de trabajo preparado `hotel_limpio` con **87,396 reservas únicas**.
2. **Completitud:**
   * La variable `company` presentó un 93.98% de valores no especificados (`"NULL"`), estandarizados como `"None"`.
   * La variable `agent` registró un 13.69% de registros `"NULL"`, imputados como `"Direct"`.
   * En `children` se detectaron 4 valores perdidos, imputados con la mediana (0).
   * Variables categóricas como `country`, `meal` y `market_segment` con etiquetas `"Undefined"` o `"NULL"` fueron convertidas a valores faltantes explícitos (`NA`).
3. **Consistencia:**
   * **Reservas sin huéspedes:** Se detectaron 166 registros con 0 adultos, 0 niños y 0 bebés, corregidos en la lógica del negocio.
   * **Reservas sin noches:** Se identificaron 651 reservas con 0 noches de estancia total.
   * **ADR atípico/negativo:** Se detectó un valor de tarifa diaria negativa (`adr = -6.38`), tratado como valor anómalo.

---

## 4. Análisis Exploratorio de Datos (EDA) y Hallazgos

### 🚗 Hallazgos de la Pregunta 7 (Solicitud de Estacionamiento)
* **Concentración por tipo de hotel:** Del total de reservas en `hotel_limpio`, únicamente el **7.38%** solicita estacionamiento (6,448 reservas).
* **Marcada disparidad:** En el **Resort Hotel**, el **16.00%** de los huéspedes solicita parqueo, comparado con solo el **3.55%** en el **City Hotel**, reflejando la prevalencia de viajes en auto particular hacia destinos de playa/resort.
* **Impacto nulo en cancelaciones:** **El 100% de las reservas que solicitaron estacionamiento se concretaron con éxito (0.00% de tasa de cancelación).** La solicitud de parqueo opera como un indicador de alta intención de viaje.

### 📊 Hallazgos de la Pregunta 8 (Canales, Depósitos y Solicitudes)
* **Dominio del Canal TA/TO:** El canal de Agencias de Viajes y Tour Operadores (TA/TO) concentra el **79.12%** del volumen total de reservas, pero registra la tasa de cancelación más alta (**30.97%**).
* **Efecto protector de las Solicitudes Especiales:** Existe una relación inversamente proporcional entre las solicitudes especiales y la cancelación. Las reservas sin solicitudes cancelan en un **33.20%**, mientras que al registrar **5 solicitudes especiales**, la tasa de cancelación disminuye drásticamente al **5.56%**.
* **Anomalía en el Depósito "No Refundable":** El segmento con depósito "No Refundable" mostró una tasa de cancelación atípica superior al **98%**, evidenciando que este tipo de depósito suele asociarse a reservas masivas bloqueadas por agencias que luego son liberadas sistemáticamente.

---

## 5. Conclusiones y Recomendaciones Estratégicas

1. **Garantía y Monetización de Estacionamiento:** Priorizar la asignación de plazas de parqueo en el *Resort Hotel* e implementar tarifas dinámicas de estacionamiento como paquete adicional durante el proceso de reserva.
2. **Fidelización y Engagement Operativo:** Incentivar a los huéspedes a registrar solicitudes especiales desde el canal directo (p. ej. elección de cama o piso), lo que incrementa el compromiso del cliente y reduce la tasa de cancelación.
3. **Revisión de Políticas de Cancelación en Agencias:** Reevaluar las condiciones comerciales y los depósitos no reembolsables con las agencias de viaje (TA/TO) para evitar el bloqueo especulativo de inventario de habitaciones.

---

## 6. Estructura del Repositorio

```text
1ACC0216-TB1-2026-2-grupo04/
│
├── README.md                        <- Documentación general y síntesis del trabajo
├── data/
│   ├── hotel_bookings_original.csv  <- Dataset original (119,390 filas)
│   └── hotel_bookings_preparado.csv <- Dataset limpio de trabajo (87,396 filas)
│
├── code/
│   └── upc-grupo04-tb1-codigo.R    <- Script completo en R (calidad de datos y EDA)
│
└── output/
    └── gráficos/                    <- Visualizaciones exportadas de ggplot2 (.png)
```

---

## 7. Ejecución del Código en RStudio

Para replicar el análisis y generar las tablas y gráficos presentados en el informe:

1. Clonar el repositorio en tu máquina local:
   ```bash
   git clone https://github.com/tu-usuario/1ACC0216-TB1-2026-2-grupo04.git
   ```
2. Abrir el proyecto en **RStudio** ejecutando el archivo del proyecto o configurando el directorio de trabajo en la raíz del repositorio.
3. Asegurarse de tener instaladas las siguientes librerías en R:
   ```R
   install.packages(c("tidyverse", "readr", "dplyr", "ggplot2", "e1071"))
   ```
4. Ejecutar de forma secuencial el script `code/upc-grupo04-tb1-codigo.R`.

---

## 8. Referencias

* **Antonio, N., de Almeida, A., & Nunes, L. (2019).** *Hotel booking demand datasets*. Data in Brief, 22, 41-49.
* **Manrique Tunque, N. I. (2026).** *Material de clase y guías de RStudio para Fundamentos de Data Science (1ACC0216)*. Universidad Peruana de Ciencias Aplicadas (UPC).
* **Wickham, H., & Grolemund, G. (2017).** *R for Data Science: Import, Tidy, Transform, Visualize, and Model Data*. O'Reilly Media.

---

<div align="center">

**Desarrollado con fines académicos para el curso Fundamentos de Data Science - UPC (2026-02)**

</div>
