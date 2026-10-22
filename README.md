# WELLWISE-X

**Pregunta del proyecto:** ¿podemos predecir la producción futura de un pozo y reconocer de antemano cuándo conviene desconfiar del pronóstico? Compararemos un modelo que pronostica siempre con una regla que deriva casos inciertos a revisión. Mediremos tanto el error de los pronósticos aceptados como la proporción de casos que se aceptan. La regresión es el núcleo; clasificación de caída futura y clustering completan el trabajo de EnergIA Digital y ayudan a investigar los fallos. [Alcance y plan de trabajo](docs/ALCANCE.md).

## Estado y alcance

Hay fuentes originales con trazabilidad, dos notebooks de revisión/exploración y un primer experimento supervisado: preparación común en el 03 y regresión/clasificación juntas en el 04. Los resultados son iniciales; falta evaluar incertidumbre, estabilidad y clustering.

En el primer experimento, la referencia «dentro de siete días, lo mismo que hoy» ganó en regresión. El bosque de clasificación mejoró AP (0,215 frente a 0,118), pero encontró sólo 45 de 164 caídas y generó 95 falsas alarmas en prueba. No se cambió la selección después de ver esos resultados. El 04 explica las medidas y muestra resultados separados por pozo.

- `data/raw/`: Excel originales, notas de uso y `manifest.json` con tamaños y SHA-256.
- `notebooks/01_revision_datos.ipynb`: revisión, vistas simplificadas, mapa de cobertura de los pozos y controles de calidad.
- `notebooks/02_exploracion.ipynb`: selección de pozos exclusivamente productores, preparación trazable y curvas de los cinco pozos; asociaciones del mismo día entre sensores, horas, gas/agua y petróleo, por pozo y con cobertura de pares. Conserva el calendario y conteo de cero horas del historial original como contexto.
- Las asociaciones del 02 tienen una celda por gráfico, selector de pozo, tabla de pares y conclusión. Su cierre propone grupos de variables y un procedimiento temporal que se implementa en 03/04: al cierre de t, predecir petróleo de t+7 días calendario y clasificar caída ≥20 % respecto de t cuando petróleo(t)>0. Los grupos se comparan en validación; las correlaciones del 02 no son importancias predictivas.
- El notebook 02 y el HTML explican los pasos con palabras simples y ejemplos: cómo leer cada dibujo, qué significan las correlaciones, qué pistas probar y cómo aprender y evaluar sin memorizar las respuestas.
- La sección 5 compara las once entradas entre sí, sin petróleo: un mapa por pozo, selector de todos los días/días operativos, pares disponibles y señal de revisión desde |Spearman| ≥0,85. Incluye un glosario básico de pozos y mediciones. Exporta `data/processed/relaciones_entre_entradas.csv`; no elimina variables ni entrena modelos.
- `notebooks/03_preparacion_modelos.ipynb`: genera entradas históricas y objetivos a siete días calendario, audita exclusiones y separa entrenamiento, validación y prueba por fechas con separación de respuestas futuras.
- `notebooks/04_modelos.ipynb`: reúne regresión y clasificación con referencias sencillas, Ridge/logística y bosques, búsqueda de hiperparámetros en tres rondas temporales y evaluación general/por pozo. Explicaciones en español para lectores sin conocimientos de petróleo.
- `data/processed/`: copia exploratoria del 02 y, desde el 03, `ejemplos_modelos_7d.csv`, `auditoria_ejemplos_7d.csv` y `reglas_modelos_7d.json`. Leer códigos NPD como texto y fechas como fechas. La copia exploratoria conserva marcas; el 03 documenta qué casos aparta para el primer experimento.
- `reports/modelos_7d/04_modelos.html`: resultados, gráficos y tablas del experimento, con CSV de métricas, predicciones y búsqueda de ajustes y JSON de selección. Se regenera al ejecutar el 04 y queda fuera de Git. Los modelos se ajustan al ejecutar el notebook; no se exportan binarios de modelos.
- `reports/02_exploracion_interactiva.html`: copia local de los gráficos, generada al ejecutar la última celda del notebook 02; no se versiona.
- `docs/ALCANCE.md`: idea vigente, límites, estado y plan de trabajo; misma propuesta en Word.
- `docs/WELLWISE-X_Proyecto_y_Cronograma.docx`: documento actualizado.

El notebook 02 conserva ceros, faltantes y paradas; sustituye por faltantes los valores numéricos no finitos y las horas fuera de [0, 24] sólo en la copia, con auditoría. El criterio de horas diarias sigue pendiente de confirmar. Marca petróleo negativo, petróleo positivo con cero horas y tipo WI sin eliminar filas. La selección exige exclusivamente `production` en todo el historial disponible del pozo. El 03 aparta las partidas y respuestas marcadas para el primer experimento, sin resolver definitivamente su interpretación. Las asociaciones no prueban causalidad ni capacidad predictiva. La regla de revisión se refiere a la confiabilidad de un pronóstico; el proyecto no recomienda intervenciones físicas.

## Preparar el entorno

Desde la raíz del proyecto, en PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

Abrir los notebooks y ejecutar sus celdas en orden. También se pueden abrir en VS Code seleccionando el entorno `.venv` como kernel. Para modelos: ejecutar 02 → 03 → 04. El 04 puede tardar unos minutos y muestra avance por grupo de variables/modelo. Sus salidas guardadas corresponden a una ejecución secuencial con Python, sin validar la interfaz de Jupyter.

Si el entorno ya existía, actualizar las dependencias con `.\.venv\Scripts\python.exe -m pip install -r requirements.txt` para disponer de Plotly. El HTML generado se abre en el navegador sin conexión a Internet.

Las tablas, estadísticas y gráficos se presentan en español, incluidos los controles de Plotly. El cuaderno 01 incluye equivalencias con los nombres originales del Excel; estos nombres se conservan en el código para mantener la trazabilidad.

## Datos y criterios iniciales

El libro contiene `Daily Production Data` y `Monthly Production Data`. La hoja mensual tiene una segunda fila de unidades que se omite al cargar los registros. El diccionario contiene las hojas `Diario` y `Mensual` y debe consultarse antes de interpretar unidades.

- Conservar los originales; documentar las reglas antes de guardar datos preparados.
- Diferenciar producción e inyección y revisar inconsistencias entre `FLOW_KIND` y `WELL_TYPE`.
- No convertir faltantes en ceros automáticamente ni eliminar valores atípicos sin documentar el criterio.
- Revisar huecos de calendario antes de construir variables temporales.
- Las unidades y definiciones pendientes del diccionario siguen pendientes de verificación.
- Los controles iniciales señalan registros para revisión; no limpian ni excluyen datos.

El primer experimento emite al cierre de t y estima petróleo del día exacto t+7. Clasifica caída ≥20 % sólo con petróleo(t)>0 y ambos volúmenes conocidos/no negativos. No usa datos futuros como entradas. El 03 aparta días de partida/objetivo marcados; conserva ceros válidos. Los modelos completan entradas faltantes sólo dentro del entrenamiento. Cortes comunes: validación desde 2014-08-30 y prueba desde 2015-09-05; se apartan ejemplos cuyas respuestas alcanzan el período siguiente. El AED ya observó el historial completo, por lo que prueba no es totalmente ajena a decisiones exploratorias. Las reglas de incertidumbre/abstención todavía no se implementaron. El estado vigente está en `docs/ALCANCE.md`; Word conserva la propuesta anterior.

## Continuidad del trabajo

- [AGENTS.md](AGENTS.md): pautas de trabajo y cuidado de los datos.
- [Handoff](docs/HANDOFF.md): estado actual, verificaciones y próximos pasos.

Los Excel se encuentran únicamente en `data/raw/`; se retiraron las copias idénticas que habían quedado en la raíz.
