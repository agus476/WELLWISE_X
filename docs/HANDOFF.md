# Handoff — WELLWISE-X

Actualizado: 2026-10-21.

## Alcance y protocolo vigente

Proyecto de EnergIA Digital: pronosticar petróleo futuro y reconocer cuándo desconfiar. Regresión y clasificación tienen un primer experimento ejecutado; incertidumbre/abstención y clustering siguen pendientes. No hay catálogo de intervenciones ni recomendaciones operativas. Consultar `docs/ALCANCE.md`; Word conserva la propuesta anterior.

Se usa el cierre de t, suponiendo que sus registros ya están disponibles, para estimar petróleo del día calendario t+7, no un acumulado semanal. Clasificación: caída ≥20 % respecto de t, sólo con petróleo(t)>0 y objetivo conocido/no negativo. Un cero futuro observado puede ser caída; un día sin fila no es cero. Las unidades diarias, algunos ceros de sensores y el criterio de horas siguen pendientes de confirmar.

## Archivos y decisiones

- `data/raw/`: Excel originales intactos, inventario SHA-256 y diccionario. Conservar códigos NPD como texto.
- `01_revision_datos.ipynb`: controles de calidad y vistas, sin exportar datos preparados. Se preservaron cambios locales del usuario.
- `02_exploracion.ipynb`: selecciona F-1 C, F-11, F-12, F-14 y F-15 D por historial exclusivamente productor (9.001 filas); excluye F-5 mixto y F-4 inyector. Copia con fila Excel, banderas, auditoría y reglas/hash en `data/processed/`. Horas fuera de [0,24] a faltante (13 celdas); conserva ceros/paradas/atípicos. Gráficos y explicaciones sencillas; sección 5 compara once entradas sin petróleo, con mínimo de 30 pares y señal exploratoria |Spearman|≥0,85. No elimina variables por correlación. Reporte: `reports/02_exploracion_interactiva.html`.
- `03_preparacion_modelos.ipynb`: copia para modelado; aparta partidas y respuestas marcadas (horas, petróleo negativo/positivo con cero horas, tipo WI). Petróleo histórico dudoso queda faltante; gas/agua negativos y no finitos no se usan como entradas. No se imputan respuestas. Rezagos exactos de 1/7 días y ventanas de 7 días calendario con conteo, incluyendo t. Desviación poblacional calculada directamente por ventana para evitar residuos numéricos en semanas constantes.
- El 03 exporta `ejemplos_modelos_7d.csv`, `auditoria_ejemplos_7d.csv` y `reglas_modelos_7d.json`. Hashes enlazan Excel, copia preparada y ejemplos. Entrenamiento: 5.199 filas; validación: 1.810; prueba: 1.711; separación: 70. Clasificación: 4.772/1.528/1.391 casos, respectivamente. Cortes compartidos por fecha: validación desde 2014-08-30, prueba desde 2015-09-05. Se purgan ejemplos cuya respuesta alcanza la siguiente partición.
- `04_modelos.ipynb`: regresión y clasificación juntas, con explicaciones para lectores sin conocimientos de petróleo. Cinco grupos acumulativos: historia, horas, presiones principales, otros sensores, gas/agua. Identidad de pozo como categoría en todos. Referencias: persistencia y clase mayoritaria/prior. Ridge/logística (tres ajustes) y bosques de 80 árboles (dos ajustes), GridSearchCV con tres rondas temporales purgadas sólo dentro del entrenamiento. Preparación en Pipeline: mediana/marca de faltantes, escalado y categorías; columnas enteramente vacías se apartan en ese ajuste. Se elige grupo/familia en validación; no se reentrena incorporando validación. Umbral de alarma por F1 en validación, no prueba. Semilla 42.
- Resultados y reporte en `reports/modelos_7d/`: CSV de búsqueda, validación, umbrales, prueba/por pozo, predicciones con filas Excel y JSON de selección; `04_modelos.html`. Directorio ignorado por Git. No se guardan binarios: los modelos se ajustan al ejecutar el 04. Notebooks nuevos tienen salidas guardadas de ejecución secuencial por Python.
- `requirements.txt`: se añadieron numpy y scikit-learn>=1.2,<2 por uso concreto. Ejecución verificada con Python 3.12.10 y scikit-learn 1.9.1. No se tocaron los notebooks 01/02 en la incorporación de modelos.

## Resultados del primer experimento

- Regresión: mejor supervisado por validación, Ridge con historia (alpha=1). No supera a persistencia en validación; se conserva persistencia como elección final. En prueba: MAE 53,19 y RMSE 138,03 para persistencia; MAE 92,63 para el candidato supervisado. Unidad diaria pendiente.
- Clasificación: bosque con grupo E (incluye gas/agua históricos), profundidad 4, mínimo 20 por hoja, max_features=0,8. Umbral 0,20 elegido en validación. Prueba: AP 0,215 frente a referencia 0,118; precision 0,321, recall 0,274, F1 0,296. Encuentra 45 de 164 caídas, omite 119 y genera 95 falsas alarmas. No presentar mejora de AP como buena detección de todas las caídas. Los cinco pozos tienen casos de prueba en ambas tareas.

## Verificaciones y límites

Formato y ejecución secuencial de 03/04 mediante Python con captura de display y Figure.show; no se ejecutó Jupyter ni se revisó visualmente el navegador. Se guardaron salidas sin errores y se generaron CSV/HTML. Se contrastaron todos los rezagos/objetivos con la fuente, ventanas por selección explícita de fechas, huecos, clases con petróleo cero, purgas de particiones/rondas y ausencia de objetivos en listas de entradas. El 03 se ejecutó desde raíz y notebooks/ con hashes idénticos; carga y rondas del 04 comprobadas desde notebooks/, sin repetir allí el ajuste. Excel originales sin cambios. El cálculo de desviación de ventana se corrigió tras detectar un residuo numérico; se repitió el entrenamiento con la corrección, sin cambiar el protocolo ni elegir ajustes por prueba.

El AED ya vio todo el historial: la prueba posterior no es totalmente ajena a decisiones exploratorias. No cambiar hiperparámetros/cortes mirando el examen ya visto. Los resultados se limitan a filas que pasan calidad y al campo/pozos observados; clasificación excluye partidas con petróleo cero. Sin calibración, intervalos ni política de abstención todavía. Correlación no es causalidad ni importancia predictiva.

Siguiente paso: leer 03/04 y revisar errores, cobertura y comportamiento por pozo; decidir un nuevo experimento o avanzar a estabilidad/incertidumbre, documentando cualquier cambio. Mantener la comparación con persistencia y no prometer mejoras no observadas.

## Lecturas debajo de resultados (2026-10-21)

Se agregaron cinco celdas Markdown en el 03 y diez en el 04, una después de cada celda de código, con lectura sencilla de la salida guardada. Incluyen tablas, barras, diagonal real/estimado y los cuatro casilleros de la matriz; distinguen AP de porcentaje de aciertos y el umbral de alarma de la caída del 20 %. Se explicita que 44 de las 45 caídas detectadas son de F-1 C y una de F-11; no hay detecciones de los otros tres pozos al corte elegido. Son interpretaciones de esta ejecución: revisarlas si se cambian datos/protocolo y se reejecuta.

Se preservaron exactamente cálculos y salidas existentes; sólo cambió la parte de exportación HTML para incluir las mismas lecturas debajo de tablas y gráficos. Se regeneró el HTML desde CSV y figuras ya guardados, sin volver a entrenar ni escribir predicciones. Formato, compilación y ubicación de las notas comprobados; exportador HTML ejecutado por separado. Sin Jupyter ni revisión visual de navegador. No cambiaron modelos ni decisiones.
