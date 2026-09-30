# WELLWISE X alcance y cronograma

## Propósito

WELLWISE-X estudiará la predicción de producción futura de petróleo y cuándo una predicción ofrece evidencia suficiente para ser utilizada como apoyo analítico. El proyecto incluye exploración, un modelo supervisado, evaluación de incertidumbre y clustering de comportamientos operativos. Esta revisión del 30 de septiembre de 2026 sustituye el alcance anterior; su versión histórica queda en Git.

## Pregunta de trabajo

¿Puede una regla de aceptación basada en incertidumbre y calidad de datos reducir el error de las predicciones aceptadas, y qué proporción de casos debe dejar sin respuesta para conseguirlo? El resultado puede ser negativo: no se presupone una mejora.

## Alcance comprometido

Revisión y EDA por pozo; definición de una variable objetivo futura y su horizonte; comparación de persistencia y un modelo supervisado; intervalos de predicción y evaluación temporal; análisis del error según la proporción de predicciones aceptadas; clustering de estados o ventanas operativas con interpretación de los grupos; notebooks reproducibles e informe de resultados.

## Extensiones

Detección fuera de dominio como señal adicional, dashboard Streamlit y escenarios económicos simulados quedan sujetos al tiempo y a los resultados. Value of Information, Policy Value, regret económico y recomendaciones de intervenciones no forman parte del compromiso principal. No se promete demostrar ahorro, causalidad ni optimización real de pozos.

## Relación con la cursada

El EDA cubre la segunda preentrega; la regresión y una clasificación con etiqueta temporal explícita cubren el trabajo supervisado; el clustering cubre el no supervisado. Para clasificación, se propone evaluar una caída futura de producción, con ventana y umbral definidos antes del test, sin interpretarla como falla confirmada. La consigna mezcla referencias a regresión y clasificación: se contempla ambas y se confirmará su exigencia con la tutora.

# Datos y evaluación

## Disponibilidad

El Excel contiene 15.634 registros diarios de siete pozos: 9.161 de producción en seis pozos y 6.473 de inyección. La hoja mensual contiene 526 registros, excluyendo la fila de unidades. La cobertura y calidad varían por pozo. Estos recuentos describen registros, no observaciones estadísticamente independientes.

## Limitaciones

Existen faltantes, días ausentes, paradas e inconsistencias operativas que requieren revisión. La presión anular y otras variables no tienen igual disponibilidad en todos los pozos. No hay en estos archivos un catálogo estructurado de acciones, costos y efectos causales; un cambio observado de producción no prueba el beneficio de una intervención. Las unidades pendientes del diccionario siguen pendientes.

## Objetivo predictivo

Se definirá tras el EDA si el objetivo es volumen futuro diario o agregado en una ventana. Deben quedar escritos el horizonte, el instante de predicción, las variables disponibles entonces, los pozos elegidos y el tratamiento de huecos de calendario. No se usará el siguiente registro como sinónimo del siguiente día ni variables futuras para explicar el objetivo.

## Evaluación temporal

Separar cronológicamente entrenamiento, validación o calibración y prueba final. Ajustar transformaciones, modelos y umbrales sin consultar el test; evitar solapamiento de ventanas objetivo entre cortes. Comparar con persistencia mediante MAE y RMSE, por pozo y período. Para clasificación, revisar balance, precisión, recall y F1. Las métricas no se fijan de antemano como resultados esperados.

## Confianza y clustering

Evaluar cobertura empírica y ancho de intervalos; la dependencia temporal impide dar por garantizada la cobertura nominal. Comparar error frente a proporción de predicciones aceptadas para distintos umbrales. Clustering agrupará estados o ventanas operativas, con escalado y selección de variables documentados; revisar separación, estabilidad e interpretación, sin llamar falla a un grupo por su sola pertenencia.

## Estados de la propuesta

Si se conservan las etiquetas originales, ACT significará aceptar la predicción para consulta analítica; INVESTIGATE, derivar el caso a revisión humana; ABSTAIN, no emitir una predicción utilizable. Ninguno recomienda modificar equipos. Las reglas y umbrales están pendientes; INVESTIGATE no implica demostrar que una medición adicional sea rentable.

# Arquitectura medallion hasta Bronze

## Implementación actual

Bronze es la capa de conservación de las fuentes originales en data/bronze/. Contiene el Excel de producción y el diccionario, sin cambios de contenido. Un manifiesto registra ruta, procedencia conocida, tamaño y SHA-256 para comprobar integridad. El traslado reemplaza la antigua ruta data/raw/.

## Recorrido de trabajo

Los notebooks 01_revision_datos y 02_exploracion leen Bronze y mantienen sus controles y análisis en memoria. Los reportes exploratorios se guardan en outputs/. El repositorio conserva notebooks, documentación y dependencias existentes. No se incorpora infraestructura adicional para esta etapa.

## Límites de Bronze

No se imputan faltantes, eliminan filas, corrigen unidades ni crean variables de entrenamiento en los archivos de Bronze. La lectura mensual puede omitir la fila de unidades dentro del DataFrame, preservando el Excel íntegro. Un reporte de calidad no equivale a un dataset limpio.

## Silver y Gold pendientes

Estas capas quedan únicamente como etapas futuras nombradas. No se crean carpetas, esquemas, pipelines, tablas, features ni resultados para ellas. Antes de diseñar Silver se deben acordar criterios de limpieza, unidades, claves, calendario y selección de pozos. Gold se definirá después de acordar el objetivo predictivo, las variables y los productos analíticos.

## Estado y siguiente decisión

La entrega actual implementa organización y trazabilidad hasta Bronze, y actualiza el alcance. El entrenamiento, los intervalos, la clasificación y el clustering pertenecen al plan posterior. El siguiente paso es revisar los hallazgos del EDA y acordar las decisiones de calidad necesarias para Silver.

# Cronograma y entregables

## Plan de ocho semanas

El plan es relativo y se ajustará a las fechas de la comisión. No implica que las etapas ya estén completadas.

## Semanas 1 y 2

Consolidar Bronze y el inventario; revisar EDA, diccionario, unidades, huecos y calidad por pozo. Definir objetivo, horizonte y criterios de selección. Acordar el diseño de Silver antes de implementarlo.

## Semanas 3 y 4

Preparar los datos aprobados y variables temporales sin fuga de información; construir persistencia y regresión. Incorporar una clasificación de caída futura con etiqueta y umbral explícitos. Evaluar temporalmente y controlar overfitting.

## Semanas 5 y 6

Construir y evaluar intervalos; comparar error y cobertura de las predicciones aceptadas. Desarrollar clustering de comportamientos operativos, interpretar grupos y revisar estabilidad. Priorizar estos resultados sobre nuevas herramientas.

## Semanas 7 y 8

Consolidar notebooks y resultados, límites y conclusiones. Completar README, informe y presentación de hasta 15 minutos. Sólo si el núcleo está terminado, evaluar un dashboard sencillo u otra extensión.

## Criterios de finalización

Las fuentes deben ser trazables; la preparación debe poder reproducirse; los modelos deben compararse con referencias temporales y reportar resultados por pozo. El clustering debe tener una interpretación sustentada. Una abstención que no mejore el error es un resultado válido y debe informarse.

## Fuentes y continuidad

Fuentes de esta revisión: Excel de producción Volve, diccionario del repositorio, documento original del proyecto y guía EnergIA Digital Data Science. La revisión bibliográfica y cualquier afirmación de originalidad requieren verificación posterior. Esta versión no sostiene una novedad mundial ni una puntuación de originalidad. La guía del curso y los datos originales se conservan sin modificaciones.
