# Handoff — WELLWISE-X

Actualizado: 2026-10-21.

## Alcance vigente

Predecir producción futura y reconocer de antemano cuándo conviene desconfiar del pronóstico. Comparar el error de pronosticar siempre con el error y la proporción de casos respondidos bajo una regla de revisión. Clasificación de caída futura y clustering completan la consigna. El alcance se explica en `docs/ALCANCE.md` y en su versión Word.

## Estado

- Los dos Excel originales están en `data/raw/`, sin cambiar sus bytes; `manifest.json` conserva tamaños y SHA-256. La carpeta anterior `data/bronze/` se retiró.
- Los notebooks 01 y 02 leen desde `data/raw/`. Se retiró `outputs/`, que sólo contenía un marcador vacío.
- El notebook 01 conserva controles de calidad y una síntesis de hallazgos, sin generar reportes HTML. Se retiraron la dependencia de reportes automáticos y su restricción de setuptools.
- Después del diccionario, el 01 crea copias completas `produccion` e `inyeccion`, una tabla de decisiones para las 24 columnas y vistas de 19 columnas. Se apartan sólo `WELL_BORE_CODE` y cuatro metadatos constantes de campo/instalación. Se conservan índices, mediciones, unidades, ceros y faltantes; `diario` permanece completo. Las vistas no se exportan ni constituyen datos preparados para modelos.
- El 01 incluye un mapa de los siete pozos con períodos, registros por flujo, días presentes/ausentes, cobertura, cero horas, horas faltantes y duplicados. El 02 usa Plotly Express: calendario por flujo sin mezclar horas, barras de días con 0 horas y tres curvas del pozo seleccionado. Títulos, detalles al pasar el cursor y explicaciones están en español; hay zoom y selectores de fechas. Los puntos naranjas de la curva de horas marcan 0 horas operativas registradas, no una hora del reloj. Los volúmenes diarios mantienen la advertencia de unidad pendiente.
- La última celda del 02 genera `reports/02_exploracion_interactiva.html`, con cinco gráficos, explicaciones y JavaScript incorporado para abrir sin Internet. `reports/` queda fuera de Git. Plotly reemplaza Matplotlib en las dependencias.
- Ambos cuadernos muestran tablas, tipos de dato, estadísticas y categorías de flujo en español mediante copias de presentación. El 01 incluye equivalencias de las 24 variables diarias y diez mensuales con el Excel. Los nombres originales permanecen en el código y en el diccionario de referencia; códigos y unidades pendientes se conservan. Plotly incorpora traducciones locales para los controles, meses y separadores numéricos, también en el HTML sin conexión.
- No hay datos preparados persistidos, modelos entrenados ni conclusiones finales del análisis por pozo.

## Verificación y siguiente paso

Se validó el formato y se ejecutaron todas las celdas de ambos cuadernos desde la raíz y desde `notebooks/`, capturando las tablas y sustituyendo sólo `Figure.show` durante la validación. Se comprobaron las traducciones de encabezados, estadísticas y flujos y que `diario` y `mensual` permanecen intactos. Las comprobaciones previas del calendario, conteos de horas y preservación de huecos siguen vigentes; también se verificó que las curvas mantienen `connectgaps=False`. Se regeneró el HTML y se revisó en Chrome sin interfaz: todos los controles presentes tienen textos en español. No se ejecutó Jupyter. Se limpiaron las salidas guardadas anteriores para evitar tablas en inglés desactualizadas; las nuevas tablas y gráficos aparecen al ejecutar las celdas. `git diff --check` pasó. El 21 de octubre se revisaron los cambios pendientes, se validó el formato de ambos cuadernos y se compiló el código de sus celdas antes de subir la actualización a GitHub. No se repitió su ejecución completa.

El mapa muestra que F-5 tiene registros tanto de inyección como de producción; F-4 sólo de inyección. Cinco pozos tienen fechas ausentes dentro de sus períodos y no hay duplicados pozo-fecha. El siguiente paso es revisar disponibilidad de sensores por pozo y período y completar las observaciones del notebook 02 antes de decidir objetivo y plazo del pronóstico. La reconciliación diaria/mensual y la confirmación de unidades siguen pendientes.
