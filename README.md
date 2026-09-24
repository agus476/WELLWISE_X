# WELLWISE-X

Proyecto de investigación para estudiar decisiones ACT / INVESTIGATE / ABSTAIN en operaciones de pozos con datos del campo Volve.

La etapa actual consiste en revisar la calidad de los datos y explorar la producción por pozo. El alcance se ampliará según los resultados del análisis.

## Organización

- `data/raw/`: Excel de producción y diccionario originales, sin modificaciones.
- `data/processed/`: datos derivados que se generen durante el trabajo.
- `notebooks/01_revision_datos.ipynb`: carga, controles iniciales y reportes automáticos con YData Profiling.
- `notebooks/02_exploracion.ipynb`: gráficos y exploración por pozo.
- `docs/`: documento del proyecto y cronograma.
- `outputs/`: gráficos y tablas exportados.

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

- Conservar los originales; guardar cualquier transformación en `data/processed/`.
- Diferenciar producción e inyección y revisar inconsistencias entre `FLOW_KIND` y `WELL_TYPE`.
- No convertir faltantes en ceros automáticamente ni eliminar valores atípicos sin documentar el criterio.
- Revisar huecos de calendario antes de construir variables temporales.
- Las unidades y definiciones pendientes del diccionario siguen pendientes de verificación.
- Los controles iniciales señalan registros para revisión; no limpian ni excluyen datos.

El objetivo predictivo y las reglas de decisión se definirán después de la exploración. El documento completo está en `docs/WELLWISE-X_Proyecto_y_Cronograma.docx`.

## Continuidad del trabajo

- [AGENTS.md](AGENTS.md): pautas de trabajo y cuidado de los datos.
- [Handoff](docs/HANDOFF.md): estado actual, verificaciones y próximos pasos.

Los Excel se encuentran únicamente en `data/raw/`; se retiraron las copias idénticas que habían quedado en la raíz.

## Reportes automáticos

La sección «Reportes automáticos con YData Profiling» del notebook 01 genera:

- `outputs/volve_production_profiling.html`: registros de producción.
- `outputs/volve_injection_profiling.html`: registros de inyección.

Actualizar las dependencias con `.\.venv\Scripts\python.exe -m pip install -r requirements.txt` y seleccionar ese entorno como kernel. Ejecutar el notebook y abrir los HTML en el navegador. No se necesita subir los datos a ningún servicio.

Los reportes complementan los controles específicos del notebook 01 y la exploración temporal por pozo del notebook 02. No realizan limpieza automática. Los HTML se regeneran al ejecutar la sección y quedan fuera del control de versiones mediante `.gitignore`.

El notebook 01 sigue el recorrido **carga y contexto → profiling → validaciones específicas → síntesis de hallazgos**. Una guía vincula las señales de los reportes con cada control; la tabla final calcula evidencia y próximos pasos para continuar en el notebook 02.
