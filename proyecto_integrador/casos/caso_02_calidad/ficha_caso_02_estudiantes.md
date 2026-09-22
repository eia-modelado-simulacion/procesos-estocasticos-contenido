# Caso 02 — Calidad de un proceso productivo

## Contexto

Una planta industrial registra semanalmente el estado de un proceso productivo y el porcentaje de unidades no conformes. El equipo de producción y calidad dispone de un histórico de 208 semanas para estudiar la evolución del proceso y apoyar la planificación de ajustes o intervenciones.

**Los datos y parámetros son sintéticos, creados con fines educativos. No representan límites de especificación ni estándares normativos.**

## Proceso de estados y unidad temporal

$X_t$ es la variable aleatoria que representa el estado del proceso productivo al finalizar la semana $t$. Un paso temporal equivale a **una semana**. El valor registrado en el histórico, $x_t$, pertenece a los siguientes estados:

| Código | Estado | Significado en el caso |
|---|---|---|
| 0 | Proceso estable | Condición de funcionamiento estable del proceso dentro del modelo. |
| 1 | Alerta | Condición que requiere atención y seguimiento de la evolución del proceso. |
| 2 | Deteriorado | Condición de deterioro más marcado del proceso dentro del modelo. |
| 3 | Ajuste/intervención | Proceso registrado en una etapa de ajuste o intervención. |

Estas categorías pertenecen al modelo didáctico; no se definen mediante cortes del porcentaje de unidades no conformes.

La matriz de transición semanal proporcionada es:

$$
P=\begin{pmatrix}
0.86&0.12&0.02&0\\
0.12&0.64&0.18&0.06\\
0&0.12&0.58&0.30\\
0.75&0.20&0.05&0
\end{pmatrix}.
$$

Las filas y columnas siguen el orden **Proceso estable, Alerta, Deteriorado, Ajuste/intervención**. Cada entrada se define como $p_{ij}=\Pr(X_{t+1}=j\mid X_t=i)$.

## Datos disponibles

El archivo `caso_02_calidad.csv` contiene una fila por semana, en orden temporal, con 208 observaciones y estas columnas:

| Columna | Descripción | Unidad o codificación |
|---|---|---|
| `semana` | Índice de la semana del histórico. | 1 a 208; intervalo semanal. |
| `estado` | Realización observada $x_t$ del estado del proceso productivo. | 0, 1, 2 o 3. |
| `porcentaje_no_conformes` | Porcentaje semanal de unidades no conformes, denotado $Y_t$. | Porcentaje (%). |

Una unidad no conforme es una unidad que no cumple los requisitos de calidad considerados en el caso. La variable `porcentaje_no_conformes` expresa qué porcentaje de las unidades evaluadas durante la semana corresponde a esa condición. Por ejemplo, un valor de `4.5` se interpreta como **4.5 %**, no como una proporción de 4.5. Una diferencia entre dos porcentajes se expresa en puntos porcentuales.

El porcentaje es una cantidad numérica distinta del código de estado. El CSV proporciona directamente el porcentaje sintético; no incluye conteos de unidades evaluadas ni de unidades no conformes. El histórico corresponde a una única realización del proceso.

## Situación de decisión

Al cierre de la semana 208, el equipo de producción y calidad debe plantear cómo apoyar la decisión de programar un ajuste o intervención del proceso, o continuar la operación bajo seguimiento. En los hitos posteriores se combinarán el análisis de los estados y el comportamiento temporal del porcentaje de unidades no conformes. En este hito se delimita el problema: no se solicita elegir una acción ni emitir una recomendación.

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
