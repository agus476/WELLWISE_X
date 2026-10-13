# WELLWISE X alcance y plan de trabajo

Revisión del 13 de octubre de 2026. Este documento define la idea vigente para el proyecto final de EnergIA Digital Data Science. El trabajo todavía está en exploración: las hipótesis y las técnicas previstas no son resultados obtenidos.

## La idea en una frase

Queremos **predecir la producción futura de petróleo de un pozo y comprobar si podemos reconocer, antes de ver el resultado, cuándo conviene desconfiar de esa predicción**.

Un pronóstico siempre tiene errores. Para una persona que lo consulta, también importa saber cuándo la información disponible es insuficiente. Por eso compararemos el desempeño de un modelo que entrega todos sus pronósticos con el de una regla que deriva los casos de mayor incertidumbre a revisión. La pregunta es si esa regla reduce el error en los casos aceptados y cuánto se pierde en cobertura: qué proporción de pronósticos todavía puede utilizarse.

## Qué vamos a hacer

1. **Explorar los datos.** Revisar fechas, pozos, paradas, faltantes, variables operativas y posibles problemas de medición. Elegir pozos y definir exactamente qué fecha y qué producción se quiere predecir.
2. **Pronosticar y comparar.** Crear una referencia simple basada en la producción reciente y un modelo de regresión. Evaluarlos en períodos posteriores a los usados para entrenar, por pozo y en conjunto.
3. **Evaluar la confianza.** Construir y comprobar intervalos o una medida de incertidumbre. Medir si los casos señalados como inciertos concentran errores más altos. Mostrar juntos el error de los pronósticos aceptados y la proporción que se acepta.
4. **Cumplir los otros componentes del curso.** Definir una etiqueta reproducible de caída futura de producción para un modelo de clasificación. Agrupar observaciones o ventanas mediante clustering y describir qué representan los grupos. Comprobar si ayudan a entender dónde falla el pronóstico, sin suponer de antemano que lo harán.

## Qué haría valioso el resultado

La comparación principal es: **pronosticar siempre frente a pronosticar con una regla de revisión**. Si la segunda alternativa reduce el error sólo porque deja casi todos los casos sin respuesta, no será una mejora útil. Mostraremos la relación completa entre error y cobertura, ejemplos reales de aciertos y fallos, y los límites del método. También es válido concluir que la regla no ayuda.

La aplicación de esta pregunta a los datos disponibles constituye nuestro enfoque de investigación. No afirmamos que pronosticar producción, crear intervalos o permitir abstención sean técnicas inéditas.

## Datos y límites

Usaremos los dos Excel de `data/bronze/`: producción de Volve y diccionario. La hoja diaria tiene 15.634 registros de siete pozos; 9.161 son registros de producción correspondientes a seis pozos. Hay fechas ausentes, períodos con el pozo detenido y variables con cobertura diferente entre pozos. El horizonte de predicción, los pozos finales, la definición de caída y las reglas de tratamiento de esos casos se decidirán después del análisis exploratorio.

Una fecha faltante no equivale a un día con producción cero. Las variables explicativas deberán estar disponibles en el momento en que se hace el pronóstico. La prueba final usará fechas posteriores a entrenamiento y calibración. En una serie temporal, la cobertura teórica de un intervalo no se da por garantizada: se medirá con datos reservados.

Estos Excel no documentan de forma estructurada acciones de intervención, sus costos ni sus efectos causales. El proyecto **no recomendará cambios físicos en los pozos ni afirmará ahorro económico real**. La clasificación de una caída de producción no significa diagnosticar una falla mecánica.

## Estado actual y próximos pasos

**Hecho:** fuentes originales preservadas en Bronze con inventario y controles iniciales; notebooks de revisión y exploración. El segundo notebook permite recorrer un pozo, pero sus conclusiones por pozo todavía deben completarse.

**Pendiente:** definir objetivo y horizonte, acordar criterios de calidad y diseñar Silver; preparar variables; entrenar y evaluar regresión y clasificación; hacer clustering; analizar incertidumbre y cobertura; redactar resultados y preparar la presentación. Gold se diseñará después de definir los productos de análisis. Todavía no hay modelos entrenados, una capa Silver ni una capa Gold.

## Orden de trabajo y entregables

| Etapa | Resultado esperado |
| --- | --- |
| Exploración y decisiones de datos | Hallazgos por pozo, objetivo y horizonte escritos, reglas de calidad justificadas. |
| Modelos supervisados | Referencia simple, regresión y clasificación, con evaluación temporal y control de sobreajuste. |
| Clustering y confianza | Grupos interpretados, intervalos evaluados y comparación entre error y cobertura. |
| Cierre | Notebooks reproducibles, README con resultados y límites, y presentación oral de hasta 15 minutos. |

El dashboard interactivo y las simulaciones económicas quedan como posibles extensiones. Una demostración clara con gráficos de casos reales alcanza para comunicar el resultado principal. El orden se ajustará a las fechas de preentrega de la comisión.

**Siguiente decisión:** terminar el análisis exploratorio de los pozos candidatos y definir el objetivo, el horizonte y las reglas de calidad antes de crear Silver. El repositorio refleja únicamente el trabajo efectivamente realizado.
