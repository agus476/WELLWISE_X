# Handoff — WELLWISE-X

Actualizado: 2026-09-22.

## Alcance acordado

El usuario pidió comenzar con una estructura simple: datos, notebooks y documentación. Ampliar según las necesidades del análisis. Todavía no se definieron el objetivo predictivo definitivo, la acción concreta de ACT ni la evaluación económica.

## Estado actual

- `README.md`: instrucciones de entorno, organización y criterios iniciales.
- `AGENTS.md`: pautas para futuros trabajos en el repositorio.
- `requirements.txt`: pandas, openpyxl, matplotlib, jupyterlab, ipykernel, ydata-profiling==4.18.4 y setuptools<81. Las demás versiones no están fijadas.
- `data/raw/`: los únicos Excel de trabajo, producción y diccionario originales.
- `docs/WELLWISE-X_Proyecto_y_Cronograma.docx`: propuesta completa y cronograma, con alcance recortado de ocho semanas.
- `notebooks/01_revision_datos.ipynb`: lectura diaria/mensual, diccionario, cobertura, duplicados, huecos de calendario, faltantes y señales de revisión.
- `notebooks/02_exploracion.ipynb`: selección de pozo, estadísticos y series de petróleo, agua y horas operativas. Selección inicial: F-12. Huecos visibles sin interpolar.
- `data/processed/`: sin dataset limpio. `outputs/`: destino de los HTML de profiling, ignorados por Git.
- No hay modelos, dashboard ni dataset limpio generado.

## Verificaciones realizadas

Las celdas de ambos notebooks se ejecutaron secuencialmente sin errores mediante Python, con matplotlib en modo no interactivo. En la revisión posterior, el notebook 01 ya tenía salidas guardadas por el usuario; se preservaron. No se validó la interfaz de Jupyter. Se creó `.venv` para instalar y comprobar YData Profiling.

Los Excel inicialmente estaban abiertos y quedaron duplicados en la raíz. En esta actualización se compararon sus hashes SHA-256 con `data/raw/`: eran idénticos. Se eliminaron únicamente los duplicados de la raíz y se conservaron los archivos de `data/raw/`.

## Hallazgos iniciales

- Diario: 15.634 registros, 24 columnas, siete pozos, del 2007-09-01 al 2016-12-01.
- 9.161 registros de producción y 6.473 de inyección; sin duplicados por código de pozo y fecha.
- 18 registros combinan `production` con `WELL_TYPE = WI`; requieren revisión.
- En producción: 1.141 registros con cero horas, 13 con más de 24 horas y uno con petróleo positivo y cero horas.
- F-12 y F-14 tienen 85 días ausentes cada uno entre su primera y última fecha.
- F-1 C tiene 97,7 % de faltantes de presión anular en sus registros de producción.
- Mensual: 526 registros. Todos coinciden en pozo/mes con una agregación diaria.
- Las sumas diarias de petróleo, gas, agua, agua inyectada y horas coinciden con los valores mensuales en los pares disponibles, salvo diferencias numéricas de redondeo. Este control se ejecutó durante la revisión inicial, pero todavía no está incorporado al notebook.

Estos resultados son controles iniciales, no una validación completa de calidad ni de unidades.

## Próximo paso sugerido

Ejecutar los notebooks con el entorno elegido, recorrer los pozos y completar las conclusiones. Definir criterios de calidad y selección de variables antes de limpiar datos. Si se solicita ampliar la auditoría, incorporar la reconciliación diaria/mensual al primer notebook.

No hay bloqueos de archivos pendientes. No asumir confirmadas las unidades del diccionario ni interpretar cambios operacionales como tratamientos causales. Para futuras políticas económicas será necesario documentar acciones, costos y supuestos.

## Incorporación de YData Profiling

Se agregó al notebook 01 una sección de reportes HTML separados por `FLOW_KIND` (producción e inyección), usando todos los registros de cada grupo, sin limpieza. Conserva controles específicos y el notebook 02. Correlaciones de Pearson descriptivas; interacciones gráficas masivas desactivadas. Se preservaron celdas y salidas previas. Leer notebooks extrayendo `source` del JSON para evitar cargar salidas HTML extensas al contexto.

Salidas previstas: `outputs/volve_production_profiling.html` y `outputs/volve_injection_profiling.html`. README y dependencias actualizados. Verificación completada: todas las celdas del notebook 01 ejecutadas secuencialmente en `.venv`, sin errores. Se generaron ambos HTML (aprox. 1,6 MB y 1,1 MB). Se conservaron las salidas anteriores del notebook sin incrustar los reportes. No se verificó visualmente el navegador. La versión 4.18.4 necesita `pkg_resources`; se agregó `setuptools<81` y se fijó la versión de profiling probada.

## Integración de la revisión

El notebook 01 ahora sigue carga/contexto y diccionario → profiling → guía de señales y controles → validaciones por pozo/flujo → síntesis calculada → conclusiones y notas. La guía no se presenta como una extracción de alertas del profiler. La síntesis calcula seis hallazgos desde los datos y conecta cada uno con su interpretación o próximo paso. Las correlaciones quedan para contraste por pozo en el notebook 02.

Se preservaron las celdas de código previas, sus salidas y las notas finales. Se verificaron todas las celdas en el nuevo orden, con regeneración de ambos HTML, sin errores; la nueva síntesis quedó lista para ejecutar, sin guardar su salida en el notebook. README actualizado. No se añadieron dependencias ni se modificaron datos originales.
