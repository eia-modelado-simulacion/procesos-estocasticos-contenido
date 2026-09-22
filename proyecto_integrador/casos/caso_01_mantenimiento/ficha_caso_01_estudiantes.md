# Caso 01 — Mantenimiento predictivo de un motor industrial

## Contexto

Una planta industrial utiliza un motor cuyo estado de funcionamiento se registra semanalmente junto con una medida de vibración. El equipo de mantenimiento dispone de un histórico sintético de 208 semanas para estudiar la evolución del sistema y, posteriormente, apoyar la planificación de sus intervenciones.

## Proceso de estados

$X_t$ es la variable aleatoria que representa el estado del motor al finalizar la semana $t$. Un paso temporal equivale a **una semana**. El valor registrado en el histórico, $x_t$, pertenece a los siguientes estados:

| Código | Estado | Significado en el caso |
|---|---|---|
| 0 | Normal | Funcionamiento habitual del motor. |
| 1 | Degradado | Condición de funcionamiento deteriorada respecto al estado normal. |
| 2 | Crítico | Condición de deterioro más severa dentro del modelo. |
| 3 | Mantenimiento | Motor registrado en una etapa de intervención de mantenimiento. |

Estas categorías pertenecen al modelo didáctico; no se definen mediante cortes normativos de vibración.

La matriz de transición semanal proporcionada es:

$$
P=\begin{pmatrix}
0.82&0.16&0.02&0\\
0.10&0.65&0.20&0.05\\
0&0.10&0.55&0.35\\
0.80&0.15&0.05&0
\end{pmatrix}.
$$

Las filas y columnas siguen el orden **Normal, Degradado, Crítico, Mantenimiento**. Cada entrada se define como $p_{ij}=\Pr(X_{t+1}=j\mid X_t=i)$.

## Datos disponibles

El archivo `caso_01_mantenimiento.csv` contiene una fila por semana, en orden temporal, con 208 observaciones y estas columnas:

| Columna | Descripción | Unidad o codificación |
|---|---|---|
| `semana` | Índice de la semana del histórico. | 1 a 208; intervalo semanal. |
| `estado` | Realización observada $x_t$ del estado del motor. | 0, 1, 2 o 3. |
| `vibracion_rms` | Valor semanal de vibración RMS (raíz media cuadrática), denotado $Y_t$. | mm/s. |

La vibración es una cantidad numérica asociada al funcionamiento del motor, distinta del código de estado. En la simulación depende del estado e incorpora variación temporal adicional. El histórico corresponde a una única realización; no representa todas las trayectorias posibles del sistema.

**Los datos y parámetros de vibración son sintéticos, creados con fines educativos. No son umbrales normativos, límites de seguridad ni criterios técnicos de intervención para un motor real.**

## Situación de decisión

Al cierre de la semana 208, el equipo debe plantear cómo apoyar la decisión de programar una intervención de mantenimiento o continuar la operación bajo seguimiento. En los hitos posteriores se combinarán el análisis de los estados y el comportamiento temporal de la vibración. En este hito se delimita el problema: no se solicita elegir una acción ni emitir una recomendación.

## Hito 0 — Comprensión del caso y planteamiento de la integración

Actividad formativa orientada a comprensión, no a cálculo. Respondan en lenguaje del sistema:

1. ¿Qué sistema representa el caso y cuál es su problema operativo?
2. ¿Qué representa $X_t$, cuál es la unidad temporal y qué significa cada estado?
3. ¿Qué información proporciona $P$? Interpreten al menos dos $p_{ij}$ en contexto.
4. ¿Qué variable o variables están registradas a través del tiempo?
5. ¿Cómo se relacionan la cadena de Markov y la serie temporal proporcionada?
6. ¿Qué decisión técnica, operativa o de negocio podría apoyarse combinando posteriormente el análisis de Markov y el análisis temporal?

Para interpretar cada $p_{ij}$, indiquen **estado de origen, estado de destino, duración del paso y probabilidad**. Para conectar cadena y serie, precisen si el dato registra directamente el estado u otra cantidad asociada al funcionamiento del caso.

No se requieren cálculos avanzados, análisis de autocorrelación ni modelos de pronóstico en este hito.
