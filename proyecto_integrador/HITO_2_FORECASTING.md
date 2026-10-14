# Proyecto integrador — Hito 2 de Forecasting

## Benchmarks y evaluación fuera de muestra

### Propósito

Establecer una estrategia de evaluación para la variable cuantitativa $Y_t$ del caso asignado y definir el benchmark que servirá como referencia para comparar modelos de forecasting más adelante.

Este hito continúa la caracterización temporal realizada en el Hito 1.

En esta etapa **todavía no deben seleccionar un modelo final ni ajustar ETS o ARIMA**.

El objetivo es responder:

> ¿Cómo evaluaremos los pronósticos y qué método simple deberá superar un modelo más complejo?

## Variables por caso

| Caso | Variable de estado | Variable temporal a analizar |
|---|---|---|
| 01 — Mantenimiento | `estado` | `vibracion_rms` |
| 02 — Calidad | `estado` | `porcentaje_no_conformes` |
| 03 — Servicios | `estado` | `solicitudes` |
| 04 — Plataforma | `estado` | `usuarios_activos` |

Todos los casos contienen 208 observaciones con frecuencia semanal.

## Actividades

### 1. Definición del problema de forecasting

Retomen la variable $Y_t$ de su caso y definan explícitamente:

- qué se desea pronosticar;
- la frecuencia temporal;
- el horizonte de pronóstico $h$;
- qué significa ese horizonte en el contexto del caso.

El horizonte debe justificarse desde el problema de decisión.

No utilicen únicamente una regla porcentual del tipo “20% de los datos para test” sin explicar su sentido temporal.

### 2. Partición temporal train/test

Construyan una partición temporal que respete el orden cronológico:

- `train`: observaciones disponibles antes del período de evaluación;
- `test`: últimas observaciones reservadas para evaluar pronósticos.

Reporten:

- número de observaciones en `train`;
- número de observaciones en `test`;
- primera y última semana de `train`;
- primera y última semana de `test`.

Justifiquen por qué la partición elegida representa razonablemente un escenario real de forecasting.

> En series temporales no deben mezclar aleatoriamente observaciones pasadas y futuras.

### 3. Benchmark mean

Construyan el benchmark de la media.

El método debe pronosticar todos los períodos futuros usando la media calculada únicamente con `train`.

Reporten:

- valor medio utilizado;
- pronóstico para todo el horizonte;
- una visualización donde se comparen `train`, `test` y el pronóstico.

Interpreten si este benchmark representa adecuadamente el nivel reciente de la serie.

### 4. Benchmark naive

Construyan el benchmark naive.

El método debe utilizar la última observación disponible de `train` como pronóstico para todos los períodos futuros.

Reporten:

- último valor observado en `train`;
- pronóstico para todo el horizonte;
- una visualización comparable con la del benchmark mean.

Interpreten el resultado en lenguaje del caso.

### 5. Benchmark drift

Construyan el benchmark drift.

Usen la expresión:

$$
\hat{y}_{T+h|T}
=
y_T
+
h\left(
\frac{y_T-y_1}{T-1}
\right)
$$

Reporten:

- $y_1$;
- $y_T$;
- número de observaciones $T$;
- cambio promedio por período;
- pronóstico para todo el horizonte.

Expliquen qué tendencia lineal está extrapolando este benchmark.

### 6. Seasonal naive — solo si está justificado

El benchmark seasonal naive es opcional.

Solo debe incluirse si el análisis del Hito 1 mostró evidencia razonable de estacionalidad.

Si deciden utilizarlo:

- identifiquen el período estacional;
- expliquen por qué ese período es razonable;
- construyan el pronóstico;
- compárenlo con los otros benchmarks.

No deben usar seasonal naive únicamente porque la serie tenga frecuencia semanal.

### 7. Cálculo de errores fuera de muestra

Para cada benchmark calculen los errores sobre `test`:

$$
e_t = y_t - \hat{y}_t
$$

Construyan una tabla que contenga, como mínimo:

- período;
- valor real;
- pronóstico mean;
- pronóstico naive;
- pronóstico drift;
- errores correspondientes.

Si utilizaron seasonal naive, inclúyanlo también.

### 8. Métricas de evaluación

Calcule para cada benchmark:

- MAE;
- RMSE;
- MASE.

Para MASE, utilicen como escala el error naive de un paso calculado exclusivamente con `train`.

Presenten una tabla comparativa de resultados.

La comparación debe realizarse usando exactamente el mismo conjunto `test` para todos los métodos.

### 9. Interpretación de las métricas

Interpreten los resultados en el contexto del caso.

Respondan, como mínimo:

- ¿qué benchmark obtiene el menor MAE?;
- ¿qué benchmark obtiene el menor RMSE?;
- ¿coinciden ambos rankings?;
- ¿qué significa el valor de MASE obtenido?;
- ¿existen errores grandes que hagan que RMSE difiera significativamente de MAE?;
- ¿algún benchmark parece claramente inadecuado para la estructura observada en el Hito 1?

No se limiten a afirmar cuál número es menor.

### 10. Selección del benchmark principal

Seleccionen un benchmark principal que servirá como referencia para las semanas siguientes.

La decisión debe estar sustentada en:

- desempeño fuera de muestra;
- interpretación del problema;
- horizonte seleccionado;
- métricas obtenidas;
- estructura temporal identificada en el Hito 1.

No es obligatorio que el benchmark principal sea naive.

La selección debe responder:

> ¿Qué método simple deberá superar un modelo más complejo para justificar su uso?

### 11. Sensibilidad al horizonte

Realicen una segunda evaluación con un horizonte diferente al utilizado inicialmente.

No es necesario repetir todo el análisis con el mismo nivel de detalle.

Comparen:

- ranking de benchmarks;
- MAE;
- RMSE;
- MASE.

Expliquen si la selección del benchmark principal se mantiene o cambia.

El propósito de esta sección es reconocer que el desempeño de un método puede depender del horizonte de pronóstico.

### 12. Conclusión diagnóstica

En máximo **250 palabras**, integren los resultados y respondan:

> ¿Qué tan difícil parece pronosticar esta serie y qué benchmark debe utilizarse como línea base para evaluar los modelos de las próximas semanas?

La conclusión debe mencionar, cuando corresponda:

- horizonte de pronóstico;
- partición train/test;
- benchmark con mejor desempeño;
- comportamiento de MAE, RMSE y MASE;
- sensibilidad al horizonte;
- relación con la estructura identificada en el Hito 1.

En este hito **no deben seleccionar todavía ETS, ARIMA ni ningún modelo final**.

## Entregable

Entreguen un notebook ejecutable de principio a fin que contenga:

- código reproducible;
- partición temporal claramente identificada;
- pronósticos de los benchmarks;
- gráficos con título, ejes y unidades;
- tabla de errores;
- tabla comparativa de métricas;
- interpretaciones en lenguaje del caso;
- selección justificada del benchmark principal;
- análisis de sensibilidad al horizonte;
- conclusión diagnóstica final.

El notebook debe poder ejecutarse mediante **Run All** sin errores.

## Criterio central de evaluación

La evaluación se centrará en la capacidad de **diseñar una comparación justa de forecasting e interpretar el desempeño fuera de muestra**.

No es suficiente calcular métricas. Deben justificar:

- cómo se definió el horizonte;
- por qué la partición temporal es adecuada;
- qué benchmark constituye la línea base;
- y qué significan los resultados para la decisión de modelado posterior.
