# Proyecto integrador — Hito 1 de Forecasting

## Caracterización temporal de la variable cuantitativa

### Propósito

Caracterizar el comportamiento temporal de la variable cuantitativa $Y_t$ del caso asignado antes de ajustar modelos de pronóstico.

Este hito continúa el trabajo previo con la cadena de Markov, pero **no solicita todavía construir ni seleccionar un modelo de forecasting**.

El objetivo es reconocer qué estructura temporal presenta la serie y qué características debería ser capaz de representar posteriormente un modelo de pronóstico.

## Variables por caso

| Caso | Variable de estado | Variable temporal a analizar |
|---|---|---|
| 01 — Mantenimiento | `estado` | `vibracion_rms` |
| 02 — Calidad | `estado` | `porcentaje_no_conformes` |
| 03 — Servicios | `estado` | `solicitudes` |
| 04 — Plataforma | `estado` | `usuarios_activos` |

Todos los casos contienen 208 observaciones con frecuencia semanal.

## Actividades

### 1. Identificación de la serie

- Definan la variable objetivo $Y_t$ en el contexto del caso.
- Indiquen qué representa cada observación.
- Indiquen su unidad.
- Indiquen la frecuencia temporal.
- Indiquen el número de observaciones disponibles.

### 2. Gráfico temporal

Construyan un gráfico temporal de las 208 observaciones.

El gráfico debe incluir:

- título;
- nombre del eje temporal;
- nombre y unidad de la variable analizada.

A partir del gráfico, describan si observan:

- cambios de nivel;
- tendencia;
- observaciones inusuales;
- cambios en la variabilidad;
- posibles patrones repetitivos.

Las conclusiones deben sustentarse en características observables de la serie.

### 3. Patrones temporales

Argumenten si existe evidencia visual de:

- tendencia;
- estacionalidad;
- comportamiento cíclico.

No deben concluir que existe estacionalidad únicamente porque la serie tenga frecuencia semanal.

Si consideran que no existe evidencia suficiente de alguno de estos componentes, indíquenlo explícitamente y justifiquen su respuesta.

### 4. Rezagos

Construyan al menos un **lag plot de orden 1**.

Expliquen qué representa el rezago 1 en el contexto específico de su caso.

Por ejemplo, deben ser capaces de interpretar conceptualmente la relación entre $Y_t$ y $Y_{t-1}$.

Describan si el lag plot muestra:

- una relación positiva;
- una relación negativa;
- poca relación aparente;
- u otra estructura relevante.

Recuerden que una relación entre valores rezagados indica asociación temporal y **no implica causalidad**.

### 5. Autocorrelación y ACF

Construyan la función de autocorrelación (ACF) de $Y_t$.

- Seleccionen un número máximo razonable de rezagos.
- Justifiquen brevemente por qué eligieron ese número.
- Interpreten la autocorrelación de primer orden.
- Describan cómo cambia la autocorrelación a medida que aumenta el rezago.
- Indiquen si la dependencia temporal parece desaparecer rápidamente o persistir durante varios períodos.
- Evalúen si la serie presenta un comportamiento compatible con ruido blanco o si contiene estructura temporal aprovechable.

La interpretación debe realizarse en unidades temporales del problema. Por ejemplo, en estos casos un rezago representa una cantidad de **semanas**.

### 6. Relación entre $X_t$ y $Y_t$

Comparen descriptivamente el comportamiento de $Y_t$ entre los cuatro estados de $X_t$.

Reporten, como mínimo, para cada estado:

- número de observaciones;
- media;
- desviación estándar.

Construyan además una visualización apropiada para comparar la distribución de $Y_t$ entre estados.

Interpreten los resultados en lenguaje del caso.

Esta comparación es **descriptiva**. Una asociación entre $X_t$ y $Y_t$:

- no implica causalidad;
- no significa que una variable determine completamente a la otra;
- no significa que ambas variables sean equivalentes.

$X_t$ representa el estado del sistema, mientras que $Y_t$ representa una cantidad numérica observada asociada a su funcionamiento.

### 7. Conclusión diagnóstica

En máximo **250 palabras**, elaboren una conclusión que responda:

> ¿Qué estructura temporal debería ser capaz de representar un futuro modelo de forecasting para esta serie?

La conclusión debe integrar, cuando corresponda:

- comportamiento observado en el gráfico temporal;
- tendencia;
- posible estacionalidad;
- dependencia observada en los rezagos;
- comportamiento de la ACF;
- relación descriptiva entre $X_t$ y $Y_t$.

En este hito **no deben seleccionar todavía ETS, ARIMA ni ningún otro modelo final**.

## Entregable

Entreguen un notebook ejecutable de principio a fin que contenga:

- código reproducible;
- gráficos con título, ejes y unidades;
- resultados numéricos relevantes;
- interpretaciones en lenguaje del caso;
- una conclusión diagnóstica final.

El notebook debe poder ejecutarse mediante **Run All** sin errores.

## Criterio central de evaluación

La evaluación se centrará en la capacidad de **comprender e interpretar la estructura temporal del sistema**.

No es suficiente producir gráficos o ejecutar funciones. Las decisiones analíticas deben explicarse y los resultados deben interpretarse en el contexto del caso.
