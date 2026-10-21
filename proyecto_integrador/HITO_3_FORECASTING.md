# Proyecto integrador — Hito 3 de Forecasting

## Suavizamiento exponencial y comparación con el benchmark

### Propósito

Seleccionar, ajustar, evaluar e interpretar modelos de suavizamiento exponencial para la variable cuantitativa $Y_t$, distinguiendo nivel, tendencia y error. Este hito conecta la estructura del Hito 1 con la evaluación del Hito 2.

> ¿El uso de suavizamiento exponencial aporta una mejora real frente al benchmark simple seleccionado en el Hito 2?

La comparación es provisional: todavía no se selecciona el modelo final definitivo del proyecto.

## Variables por caso

| Caso | Variable de estado $X_t$ | Variable temporal $Y_t$ |
|---|---|---|
| 01 — Mantenimiento | `estado` | `vibracion_rms` |
| 02 — Calidad | `estado` | `porcentaje_no_conformes` |
| 03 — Servicios | `estado` | `solicitudes` |
| 04 — Plataforma | `estado` | `usuarios_activos` |

Todos los casos contienen 208 observaciones semanales. Se pronostica $Y_t$; $X_t$ conserva su interpretación como estado del sistema, sin convertirse automáticamente en predictor.

## Actividades

### 1. Recuperación del diagnóstico y del benchmark

- Resuman la estructura identificada en Hito 1: nivel, posible tendencia, variabilidad y dependencia de corto plazo.
- Recuperen el benchmark principal seleccionado en Hito 2 y su justificación; puede ser mean, naive, drift u otro benchmark previamente justificado.
- Indiquen qué tendría que mejorar un modelo más flexible para justificar su uso.

Contrasten la estructura identificada en el Hito 1 con los resultados de los modelos de suavizamiento. No impongan tendencia ni estacionalidad si sus datos no proporcionan evidencia para ello.

### 2. Evaluación temporal comparable

Mantengan el horizonte, corte temporal y test principal del Hito 2. Reporten número de observaciones, primera y última semana de cada partición y sentido operativo del horizonte. No mezclen pasado y futuro aleatoriamente.

Si es indispensable corregir una decisión previa, justifíquenlo y recalculen también el benchmark con el nuevo corte: no comparen métricas de tests diferentes. Ajusten parámetros y seleccionen estructura únicamente con train. Los valores de test se utilizan para evaluación, no para optimizar alpha, phi ni la configuración de AutoETS.

### 3. SES: nivel que se actualiza

Ajusten SES con inicialización y alpha estimados exclusivamente con train. Reporten $\alpha$ y el nivel final $\ell_T$ e interpreten:

$$
\ell_t=\alpha y_t+(1-\alpha)\ell_{t-1}
$$

Expliquen qué implica el alpha obtenido en términos de memoria y reacción. Distingan el nivel actualizado de una predicción previa a observar el dato. Construyan el pronóstico para todo test y expliquen por qué es horizontal.

### 4. Holt: nivel y tendencia

Ajusten Holt con train. Reporten e interpreten nivel final $\ell_T$ y tendencia final $b_T$, en unidades de su variable y unidades por semana, respectivamente.

$$
\hat{y}_{T+k|T}=\ell_T+k b_T,\qquad k=1,\ldots,h
$$

Construyan gráficos de las componentes de nivel y tendencia, como en F02_03, y expliquen si la tendencia local resulta consistente con Hito 1. Produzcan pronósticos sobre el mismo test.

### 5. Tendencia amortiguada

Ajusten al menos una variante de Holt con tendencia amortiguada. Reporten $\phi$, interpreten cómo reduce la contribución futura de tendencia y comparen su trayectoria con Holt no amortiguado.

$$
\hat{y}_{T+k|T}=\ell_T+(\phi+\phi^2+\cdots+\phi^k)b_T
$$

Justifiquen si amortiguar tiene sentido operativo. No es obligatorio que esta variante gane.

### 6. AutoETS e interpretación de la estructura

Usen AutoETS sin estacionalidad: `season_length=1, model="ZZN"`, como en el notebook formativo. Esto restringe la búsqueda de componentes, no cambia la frecuencia semanal de los datos.

Reporten el modelo seleccionado mediante `auto.model_["method"]` y su AICc. Decodifiquen **ETS(Error, Trend, Seasonal)**:

- error aditivo o multiplicativo y su interpretación en relación con la variabilidad;
- presencia o ausencia de tendencia y, si corresponde, amortiguación;
- ausencia de componente estacional.

AICc selecciona entre candidatos admisibles dentro de train; no garantiza el menor error fuera de muestra. Si hay ceros o valores negativos, no fuercen error multiplicativo. Interpreten la estructura elegida en el contexto del caso, sin aceptar el resultado automáticamente.

Produzcan el forecast sobre el mismo test. No se requieren derivaciones completas de las ecuaciones de espacio de estados.

### 7. Comparación fuera de muestra

Construyan una tabla por período con valor real, pronóstico y error $e_t=y_t-\hat{y}_t$ para benchmark principal, SES, Holt, Holt amortiguado y AutoETS. Incluyan un gráfico comparable de train reciente, test y pronósticos, con títulos, ejes y unidades.

Calculen para todos:

$$
MAE=\frac{1}{h}\sum_{t=T+1}^{T+h}|e_t|,\qquad
RMSE=\sqrt{\frac{1}{h}\sum_{t=T+1}^{T+h}e_t^2}
$$

$$
MASE=\frac{MAE}{\frac{1}{T-1}\sum_{t=2}^{T}|y_t-y_{t-1}|}
$$

La escala MASE es la naive de un paso calculada solo con train, igual que en Hito 2. Si es prácticamente cero, reporten MASE como no definido y expliquen la limitación.

Presenten una tabla única. MAE y RMSE tienen las unidades de $Y_t$; RMSE penaliza más errores grandes. MASE es adimensional: un valor menor que 1 mejora la escala naive de un paso en train, pero **no demuestra por sí solo superar al benchmark principal sobre test**. Para eso comparen directamente sus filas.

Identifiquen el mejor suavizamiento según MAE y revisen si el ranking por RMSE coincide. Cuantifiquen la reducción relativa de MAE frente al benchmark si su MAE no es cero; en caso contrario comparen diferencias absolutas. Discutan si la mejora es relevante para la decisión del caso, sin inventar un umbral universal.

### 8. Residuos y dependencia pendiente

Analicen residuos de un paso en train de AutoETS y del suavizamiento con menor MAE si es otro modelo. Para SES/Holt usen observado menos `fittedvalues`; para AutoETS, observado menos `predict_in_sample()["fitted"]`, en unidades originales.

Para cada modelo diagnosticado:

- construyan el gráfico temporal de residuos;
- reporten e interpreten su media respecto a la escala de la variable;
- construyan ACF sin lag cero, con un máximo de rezagos razonable y justificado;
- indiquen si hay rachas, cambios de variabilidad o autocorrelaciones sistemáticas;
- determinen si queda dependencia temporal lineal sin explicar, conectando con lag plots y ACF del Hito 1.

Distingan residuos de un paso en train de errores a varios pasos sobre test. Media próxima a cero no implica independencia. Un pico aislado puede ocurrir por azar; las bandas de ACF son aproximadas. Tampoco una ACF sin patrones garantiza buen pronóstico.

### 9. Juicio sobre el valor de la complejidad

Integren desempeño fuera de muestra, estructura de la serie, componentes estimadas y residuos. Justifiquen si el suavizamiento aporta valor frente a la referencia del Hito 2. Conservar el benchmark si no se mejora es una conclusión válida.

Un único corte no demuestra superioridad general. No reemplacen repetidamente el test para obtener una victoria. Pueden recuperar la sensibilidad al horizonte del Hito 2 como análisis adicional, siempre recalculando todos los modelos y el benchmark para cada corte.

### 10. Conclusión diagnóstica

En máximo **300 palabras**, respondan:

1. ¿Qué modelo de suavizamiento funcionó mejor?
2. ¿Superó al benchmark? Sustenten con las tres métricas y el horizonte.
3. ¿Qué estructura temporal parece haber capturado?
4. ¿Qué estructura parece quedar en los residuos?
5. ¿Qué implicación tienen estos resultados para la siguiente etapa de modelado?

Conecten explícitamente con Hito 1 y preparen preguntas para estacionariedad, diferenciación y modelos ARIMA de Semana 4. En este hito **no ajusten ARIMA**, no seleccionen el modelo final definitivo y no realicen derivaciones completas de espacio de estados.

Holt-Winters se menciona como extensión estacional: **no es requisito ni debe utilizarse automáticamente por tratarse de datos semanales**.

## Entregable

Entreguen un notebook ejecutable de principio a fin mediante **Run All**, con:

- código reproducible y rutas relativas, sin depender de una carpeta personal;
- benchmark del Hito 2 y evaluación temporal claramente identificados;
- ajustes SES, Holt, tendencia amortiguada y AutoETS;
- interpretación de alpha, nivel, tendencia, phi y estructura ETS;
- pronósticos, tabla de errores y comparación MAE/RMSE/MASE;
- gráficos con títulos, ejes y unidades;
- diagnóstico de residuos y relación con Hito 1;
- conclusión final de máximo 300 palabras.

## Criterio central de evaluación

La evaluación se centra en determinar si la complejidad adicional **aporta valor frente a una referencia simple bajo una comparación justa**. No basta ejecutar AutoETS ni señalar el número más bajo: deben explicar la estructura capturada, la dependencia pendiente y las implicaciones para el sistema.
