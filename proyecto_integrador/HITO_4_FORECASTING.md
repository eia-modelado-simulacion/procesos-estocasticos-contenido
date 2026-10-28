# Proyecto integrador — Hito 4 de Forecasting

## Cierre del bloque: dependencia, ARIMA y decisión final

**Propósito:** justificar si ARIMA aporta valor frente al benchmark de Hito 2 y al mejor ETS/suavizamiento de Hito 3. Integren estructura, desempeño fuera de muestra y residuos. La decisión cierra el bloque y sigue siendo provisional frente a nuevos datos y horizontes.

Lectura: [FPP3, capítulo 9](https://otexts.com/fpp3/arima.html), estacionariedad (§9.1), AR (§9.3), MA (§9.4), ARIMA (§9.5), estimación y selección (web actual §9.6–9.7), ARIMA vs ETS (§9.10).

## Variables y alcance

| Caso | Estado $X_t$ | Variable cuantitativa $Y_t$ |
|---|---|---|
| 01 — Mantenimiento | `estado` | `vibracion_rms` |
| 02 — Calidad | `estado` | `porcentaje_no_conformes` |
| 03 — Servicios | `estado` | `solicitudes` |
| 04 — Plataforma | `estado` | `usuarios_activos` |

Cada caso tiene **208 observaciones semanales**. Declaren unidades. Frecuencia semanal no impone estacionalidad de 52 semanas. SARIMA no es obligatorio: los casos no requieren aquí una semana estacional dedicada.

## Actividades

### 1. Recuperación de Hitos 2 y 3

Recuperen corte temporal, horizonte y test principal; benchmark principal de Hito 2 y justificación; mejor ETS/suavizamiento de Hito 3 según el criterio allí declarado, con su especificación exacta. No vuelvan a elegir referencias para favorecer ARIMA.

Reporten tamaño y primera/última semana de train y test y sentido operativo de h. Mantengan variable, escala, observaciones y horizonte comunes. Si deben corregir una decisión previa, justifiquen y recalculen todos los candidatos. Nunca comparen métricas de tests diferentes.

### 2. Estacionariedad y elección de d

Con **train únicamente**, grafiquen niveles y ACF de $Y_t$. Describan media, variabilidad y dependencia: ¿parecen estables? Estacionariedad permite autocorrelación; una ACF persistente puede deberse a un nivel cambiante.

Si corresponde, calculen y grafiquen:

$$
\Delta y_t=y_t-y_{t-1}
$$

Comparen niveles y diferencias, incluida su ACF. Expliquen unidades, pérdida de observaciones y diferencia entre cambio absoluto y porcentual. Justifiquen d=0 o d=1; no diferencien por rutina. Para d=2 aporten evidencia adicional y discutan sobrediferenciación. No usen test para elegir d.

ADF es opcional: su hipótesis nula es raíz unitaria. Rechazarla no demuestra toda forma de estacionariedad y no rechazarla tampoco prueba su presencia. Prioricen gráficos y argumento estructural; no se exige una batería de pruebas.

### 3. ACF/PACF de la serie usada para modelar

Examinen ambas sobre niveles si d=0 o sobre la serie diferenciada si d>0. Indiquen estimador PACF y máximo de rezagos justificado, con ejes en semanas.

Guía: en AR(p) puro estacionario PACF tiende a cortar después de p y ACF a decaer; en MA(q), ACF tiende a cortar después de q y PACF a decaer. Son patrones poblacionales ideales, **no reglas infalibles**. No conviertan cada pico fuera de banda en un término obligatorio.

### 4. Al menos un ARIMA razonado

Antes de evaluar, propongan y justifiquen un orden pequeño ARIMA(p,d,q) y presencia o ausencia de constante/drift. Expliquen p como memoria de valores, d como diferenciación y q como memoria de innovaciones. MA no es una media móvil para suavizar observaciones.

Ajusten `statsforecast.models.ARIMA` solo a niveles originales de train: el modelo diferencia internamente. No pasen diferencias a un modelo que vuelve a diferenciarlas. Reporten orden, coeficientes y configuración de constante/drift; interprétenlos en el caso. Un candidato simple, incluso (0,1,0), es válido si se razona.

### 5. AutoARIMA crítico

Ajusten `statsforecast.models.AutoARIMA` con train, `seasonal=False, season_length=1`. Documenten restricciones, `ic='aicc'`, elección automática o fijada de d y búsqueda escalonada. Pueden restringir órdenes pequeños y max_d=1 si el diagnóstico lo apoya. No es obligatorio coincidir con el candidato razonado.

Muestren el modelo seleccionado, su orden \((p,d,q)\), coeficientes, presencia de constante/drift y AICc. Utilicen la API validada durante la clase para recuperar correctamente esta información. Contrasten el resultado con el diagnóstico inicial.

AICc selecciona dentro de train y formulaciones comparables, sin garantizar menor MAE futuro. La diferenciación requiere diagnóstico propio: no comparen ingenuamente AICc de respuestas transformadas distintas. **No usen AICc directamente para decidir entre ETS y ARIMA**. La búsqueda escalonada tampoco garantiza exploración exhaustiva.

### 6. Forecast y comparación común

Pronostiquen en unidades originales todo el mismo test desde un origen fijo, sin actualizar modelos con sus observaciones. Incluyan al menos:

1. Benchmark principal recuperado de Hito 2.
2. Mejor ETS/suavizamiento recuperado de Hito 3.
3. ARIMA razonado.
4. AutoARIMA.

Construyan tabla por semana con real, pronóstico y error de cada modelo y gráfico común de train reciente, test y trayectorias, con títulos, ejes y unidades. Si AutoARIMA coincide con el benchmark, expliquen la equivalencia sin inventar una mejora.

Para $e_t=y_t-\hat y_t$:

$$
MAE=\frac1h\sum_{t=T+1}^{T+h}|e_t|,\qquad
RMSE=\sqrt{\frac1h\sum_{t=T+1}^{T+h}e_t^2}
$$

$$
MASE=\frac{MAE}{\frac1{T-1}\sum_{t=2}^{T}|y_t-y_{t-1}|}
$$

Usen la misma escala MASE no estacional de train para todos. Si es prácticamente cero, reporten MASE no definido. MAE/RMSE tienen unidades originales; MASE es adimensional. MASE<1 mejora la escala naive de un paso en train, no demuestra superar al benchmark en test.

Presenten tabla única con las tres métricas, ordenada por el criterio principal declarado previamente; contrasten rankings. Cuantifiquen reducción relativa de MAE frente al benchmark cuando su MAE sea positivo; si es cero, comparen diferencias absolutas. Discutan relevancia operativa, sin inventar umbrales universales.

### 7. Residuos del ARIMA seleccionado

Diagnostiquen prioritariamente el AutoARIMA seleccionado utilizando únicamente train. Si además desean contrastarlo con el ARIMA razonado, pueden reportar ambos diagnósticos. La elección del modelo a diagnosticar no debe hacerse buscando el menor error en test. Si el modelo final es ETS o benchmark, igualmente diagnostiquen el ARIMA y recuperen los residuos de Hito 3 para sustentar la comparación.

Calculen residuos de un paso en train como observado menos `predict_in_sample()['fitted']`, en unidades originales. Documenten exclusiones iniciales por diferenciación/inicialización. Reporten media, gráfico temporal y ACF residual sin lag cero, con máximo de rezagos justificado.

Busquen sesgo, rachas, cambios de variabilidad y dependencia lineal pendiente. Media pequeña no implica independencia; picos aislados pueden ocurrir por azar y las bandas son aproximadas. Una ACF sin estructura clara no garantiza buen pronóstico. Distingan estos residuos de los errores de varios pasos en test.

### 8. Decisión final y limitaciones

En máximo **350 palabras**, elijan benchmark, ETS/suavizamiento o ARIMA para el horizonte del caso. Citen MAE/RMSE/MASE, mejora o empate, complejidad, estructura y evidencia residual. Conservar el benchmark es válido: ARIMA no debe ganar por obligación.

Reconozcan explícitamente un solo corte train/test, horizonte específico, incertidumbre muestral, posibles cambios de régimen y resultados no universales. El mismo test se ha consultado en varios hitos: la comparación es exploratoria, no una confirmación independiente. No tanteen órdenes buscando ganar en test. Propongan como siguiente paso orígenes móviles o nuevos datos, sin exigirlo como trabajo adicional del hito.

### 9. Puente conceptual Markov + Forecasting

Sea $X_t$ el estado del sistema y $Y_t$ la variable cuantitativa. Recuperen P de la parte Markov y expliquen qué información distinta resume. Estimen **con train** medias condicionales y tamaños por estado:

$$
\mu_j=E[Y_t\mid X_t=j]
$$

Si un estado no aparece o tiene pocos datos, declaren la limitación y no inventen su media. Si P se estimó con toda la serie, solo puede usarse como ilustración descriptiva o debe reestimarse con train para una comparación predictiva justa.

Bajo transiciones homogéneas y medias por estado estables, la referencia conceptual es:

$$
E[Y_{t+h}\mid X_t=i]
=\sum_j P(X_{t+h}=j\mid X_t=i)\mu_j
$$

Las probabilidades provienen de la fila i de $P^h$. La igualdad requiere además que la media de Y, condicionada al estado de llegada, no dependa adicionalmente del estado inicial ni del horizonte. Markov para X por sí solo no basta; con tendencia dentro de los estados o medias cambiantes será una aproximación.

En un párrafo comparen esta expectativa basada en estados con modelos que utilizan la historia temporal de Y y describan qué supuesto podría fallar. Pueden ilustrar una fila de $P^h$. **No se exige implementar un modelo híbrido complejo, sustituir ARIMA/ETS ni añadir esta referencia al ranking obligatorio.**

## Entrega y evaluación

Entreguen un notebook ejecutable mediante Run All, con rutas relativas compatibles con repositorio/DataHub, sin rutas personales ni variables de sesiones previas. Incluyan recuperación de Hitos 2–3, estacionariedad y d, ACF/PACF, ajustes razonado/automático e interpretación, pronósticos y errores del test común, métricas, residuos, decisión final y puente Markov.

Se evalúa coherencia, ausencia de fuga temporal, comparación justa, interpretación crítica del automatismo y decisión respaldada por evidencia. No basta ejecutar AutoARIMA o señalar el número menor. No se exige SARIMA ni un ganador universal.
