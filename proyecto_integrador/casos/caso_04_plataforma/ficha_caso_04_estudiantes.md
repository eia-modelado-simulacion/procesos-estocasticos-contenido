# Caso 04 — Engagement de usuarios en una plataforma digital

## Contexto

Una plataforma digital busca comprender la evolución del vínculo de sus usuarios con el servicio. El equipo de producto dispone de un histórico de 208 semanas con una clasificación del engagement y el número de usuarios activos semanales. Esta información permitirá estudiar posteriormente la dinámica del engagement y el comportamiento temporal de la actividad para apoyar decisiones de reactivación.

**Los datos y todos los parámetros del caso son sintéticos y se utilizan exclusivamente con fines educativos.**

## Proceso de estados y unidad temporal

$X_t$ es la variable aleatoria que representa el estado de engagement de la base de usuarios de la plataforma al finalizar la semana $t$, según una clasificación conjunta de su vinculación con el servicio. Un paso temporal equivale a **una semana**. El valor registrado en el histórico, $x_t$, pertenece a los siguientes estados:

| Código | Estado | Significado en el caso |
|---|---|---|
| 0 | Alta actividad | Vinculación elevada con la plataforma, con acceso frecuente y uso sostenido de funcionalidades. |
| 1 | Actividad regular | Vinculación habitual con la plataforma y uso relativamente constante del servicio. |
| 2 | Riesgo de desenganche | Condición de vulnerabilidad del vínculo con la plataforma, según los indicadores de engagement. |
| 3 | Reactivación | Condición de retorno o renovación de la participación tras un debilitamiento del vínculo. |

**El estado de engagement es una clasificación amplia y no está determinado exclusivamente por el número de usuarios activos.** Incorpora frecuencia de acceso, uso de funcionalidades, actividad reciente y otros indicadores. Los códigos identifican categorías del modelo; no se asignan mediante umbrales de usuarios activos. El CSV registra la categoría resultante, pero no incluye por separado los indicadores de esa clasificación. El estado Reactivación describe una condición del engagement y no acredita, por sí mismo, que se haya implementado una acción de reactivación.

La evolución semanal de los estados se representa mediante una cadena de Markov con la siguiente matriz de transición sintética:

$$
P=\begin{pmatrix}
0.75&0.22&0.03&0\\
0.12&0.67&0.17&0.04\\
0.02&0.16&0.60&0.22\\
0.30&0.45&0.20&0.05
\end{pmatrix}.
$$

Las filas y columnas siguen el orden **0, 1, 2, 3**: Alta actividad, Actividad regular, Riesgo de desenganche y Reactivación. Cada entrada se define como $p_{ij}=\Pr(X_{t+1}=j\mid X_t=i)$.

## Datos disponibles y usuarios activos semanales

El archivo `caso_04_plataforma.csv` contiene **208 observaciones**, una por semana, en orden temporal. Está codificado en UTF-8 y separado por comas, con estas columnas:

| Columna | Descripción | Unidad o codificación |
|---|---|---|
| `semana` | Índice de la semana del histórico. | Entero de 1 a 208; intervalo semanal. |
| `estado` | Realización observada $x_t$ del estado de engagement de la base de usuarios. | 0, 1, 2 o 3. |
| `usuarios_activos` | Realización observada $y_t$ del número de usuarios activos durante la semana. | Número entero de usuarios por semana. |

$Y_t$ representa el **número semanal de usuarios activos**. Para este caso, un usuario activo es un usuario distinto que registra al menos un acceso a la plataforma durante la semana. Cada usuario se cuenta una sola vez dentro de esa semana, aunque acceda varias veces; puede volver a contarse en semanas posteriores. Esta variable no representa el número de sesiones ni el total acumulado de usuarios registrados.

En el diseño sintético, los usuarios activos están asociados al estado e incorporan variación temporal adicional. Esta asociación no convierte el número de usuarios activos en una regla suficiente para identificar el estado de engagement. El histórico corresponde a una única realización del sistema.

## Situación de decisión

Al cierre de la semana 208, el equipo de producto busca evaluar posteriormente si la evidencia conjunta justifica **implementar o reforzar una acción de reactivación o mantener la estrategia actual**. En los hitos posteriores se combinarán el análisis de los estados de engagement y el comportamiento temporal de los usuarios activos para aportar evidencia a esa evaluación.

En este hito se delimita el problema. No se solicita elegir una acción, emitir una recomendación ni diseñar todavía una intervención específica.

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
