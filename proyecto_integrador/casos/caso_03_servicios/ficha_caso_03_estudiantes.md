# Caso 03 — Capacidad de un centro de servicios

## Contexto

Un centro de servicios recibe solicitudes que deben ser atendidas con la capacidad disponible. Su funcionamiento depende tanto del volumen de solicitudes nuevas como de la carga pendiente, los tiempos de atención, la disponibilidad de personal y recursos, y otros indicadores operativos. El equipo de planificación dispone de un histórico de 208 semanas para estudiar la evolución del centro y, posteriormente, apoyar decisiones sobre su capacidad.

**Los datos y todos los parámetros del caso son sintéticos y se utilizan exclusivamente con fines educativos.**

## Proceso de estados y unidad temporal

$X_t$ es la variable aleatoria que representa el estado operativo del centro de servicios al finalizar la semana $t$, según una evaluación conjunta de su condición operativa. Un paso temporal equivale a **una semana**. El valor registrado en el histórico, $x_t$, pertenece a los siguientes estados:

| Código | Estado | Significado en el caso |
|---|---|---|
| 0 | Holgura operativa | Condición de baja presión sobre la capacidad disponible y margen para atender trabajo adicional. |
| 1 | Operación normal | Condición de funcionamiento habitual, con una carga operativa manejable bajo la planificación vigente. |
| 2 | Alta presión operativa | Condición de mayor exigencia sobre la capacidad y menor margen para absorber trabajo adicional. |
| 3 | Sobrecarga | Condición de saturación operativa, con dificultades para atender la carga de trabajo bajo la planificación vigente. |

El estado **no está determinado únicamente por el número de solicitudes recibidas**. Incorpora también carga pendiente, tiempos de atención, disponibilidad y otros indicadores. Los códigos identifican categorías del modelo; no se asignan mediante umbrales de solicitudes. El CSV registra la categoría resultante, pero no incluye por separado los indicadores de esa evaluación.

La evolución semanal de los estados se representa mediante una cadena de Markov con la matriz de transición proporcionada:

$$
P=\begin{pmatrix}
0.55&0.40&0.05&0\\
0.15&0.65&0.18&0.02\\
0.03&0.20&0.60&0.17\\
0.10&0.25&0.35&0.30
\end{pmatrix}.
$$

Las filas y columnas siguen el orden **0, 1, 2, 3**: Holgura operativa, Operación normal, Alta presión operativa y Sobrecarga. Cada entrada se define como $p_{ij}=\Pr(X_{t+1}=j\mid X_t=i)$.

## Datos disponibles y solicitudes semanales

El archivo `caso_03_servicios.csv` contiene **208 observaciones**, una por semana, en orden temporal. Está codificado en UTF-8 y separado por comas, con estas columnas:

| Columna | Descripción | Unidad o codificación |
|---|---|---|
| `semana` | Índice de la semana del histórico. | Entero de 1 a 208; intervalo semanal. |
| `estado` | Realización observada $x_t$ del estado operativo del centro. | 0, 1, 2 o 3. |
| `solicitudes` | Realización observada $y_t$ del número de solicitudes recibidas durante la semana. | Número entero de solicitudes por semana. |

$Y_t$ representa el **número semanal de solicitudes recibidas**. Cuenta las solicitudes que ingresan durante la semana; no representa el número atendido ni el total de solicitudes pendientes. Es una variable cuantitativa distinta del código de estado $X_t$.

En el diseño sintético, las solicitudes están asociadas al estado e incorporan variación temporal adicional. Esta asociación no convierte el volumen de solicitudes en una regla suficiente para identificar el estado operativo. El histórico corresponde a una única realización del sistema.

## Situación de decisión

Al cierre de la semana 208, el equipo de planificación busca evaluar posteriormente si conviene **realizar un ajuste anticipado de capacidad o mantener la planificación actual**. En los hitos posteriores se combinarán el análisis de los estados operativos y el comportamiento temporal de las solicitudes para aportar evidencia a esa evaluación.

En este hito se delimita el problema. No se solicita elegir una acción, emitir una recomendación ni dimensionar exactamente el personal o los recursos necesarios.

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
