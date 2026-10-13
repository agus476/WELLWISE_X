# WELLWISE-X

**Pregunta del proyecto:** ¿podemos predecir la producción futura de un pozo y reconocer de antemano cuándo conviene desconfiar del pronóstico? Compararemos un modelo que pronostica siempre con una regla que deriva casos inciertos a revisión. Mediremos tanto el error de los pronósticos aceptados como la proporción de casos que se aceptan. La regresión es el núcleo; clasificación de caída futura y clustering completan el trabajo de EnergIA Digital y ayudan a investigar los fallos. [Alcance y plan de trabajo](docs/ALCANCE.md).

## Estado y alcance

La implementación actual llega hasta **Bronze**: fuentes originales con trazabilidad y notebooks de revisión/exploración. Todavía no hay modelos entrenados ni conclusiones finales del EDA. Silver y Gold quedan pendientes de diseño y no se implementan ni se crean sus carpetas.

- `data/bronze/`: Excel originales, README de la capa y `manifest.json` con tamaños y SHA-256.
- `notebooks/01_revision_datos.ipynb`: revisión y reportes de calidad.
- `notebooks/02_exploracion.ipynb`: exploración por pozo.
- `docs/ALCANCE.md`: idea vigente, límites, estado y plan de trabajo; misma propuesta en Word.
- `docs/WELLWISE-X_Proyecto_y_Cronograma.docx`: documento actualizado.
- `outputs/`: reportes exploratorios; no constituyen Gold.

Los archivos se trasladaron de `data/raw/` a Bronze sin alterar sus bytes. Se retiró la carpeta vacía `data/processed/`. Antes de crear Silver se acordarán reglas de calidad, calendario y unidades. La regla de revisión se refiere a la confiabilidad de un pronóstico; el proyecto no recomienda intervenciones físicas.

## Preparar el entorno

Desde la raíz del proyecto, en PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

Abrir los notebooks y ejecutar sus celdas en orden. También se pueden abrir en VS Code seleccionando el entorno `.venv` como kernel.

## Datos y criterios iniciales

El libro contiene `Daily Production Data` y `Monthly Production Data`. La hoja mensual tiene una segunda fila de unidades que se omite al cargar los registros. El diccionario contiene las hojas `Diario` y `Mensual` y debe consultarse antes de interpretar unidades.

- Conservar los originales; no persistir transformaciones hasta definir Silver.
- Diferenciar producción e inyección y revisar inconsistencias entre `FLOW_KIND` y `WELL_TYPE`.
- No convertir faltantes en ceros automáticamente ni eliminar valores atípicos sin documentar el criterio.
- Revisar huecos de calendario antes de construir variables temporales.
- Las unidades y definiciones pendientes del diccionario siguen pendientes de verificación.
- Los controles iniciales señalan registros para revisión; no limpian ni excluyen datos.

El objetivo predictivo, horizonte, etiqueta de clasificación y reglas de revisión se definirán después de la exploración. El alcance vigente está en `docs/ALCANCE.md` y `docs/WELLWISE-X_Proyecto_y_Cronograma.docx`.

## Continuidad del trabajo

- [AGENTS.md](AGENTS.md): pautas de trabajo y cuidado de los datos.
- [Handoff](docs/HANDOFF.md): estado actual, verificaciones y próximos pasos.

Los Excel se encuentran únicamente en `data/bronze/`; se retiraron las copias idénticas que habían quedado en la raíz.

## Reportes automáticos

La sección «Reportes automáticos con YData Profiling» del notebook 01 genera:

- `outputs/volve_production_profiling.html`: registros de producción.
- `outputs/volve_injection_profiling.html`: registros de inyección.

Actualizar las dependencias con `.\.venv\Scripts\python.exe -m pip install -r requirements.txt` y seleccionar ese entorno como kernel. Ejecutar el notebook y abrir los HTML en el navegador. No se necesita subir los datos a ningún servicio.

Los reportes complementan los controles específicos del notebook 01 y la exploración temporal por pozo del notebook 02. No realizan limpieza automática. Los HTML se regeneran al ejecutar la sección y quedan fuera del control de versiones mediante `.gitignore`.

El notebook 01 sigue el recorrido **carga y contexto → profiling → validaciones específicas → síntesis de hallazgos**. Una guía vincula las señales de los reportes con cada control; la tabla final calcula evidencia y próximos pasos para continuar en el notebook 02.
