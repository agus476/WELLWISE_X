# Pautas para trabajar en WELLWISE-X

## Contexto y alcance

Proyecto de investigación de Data Science / Fundación YPF con datos de Volve. El objetivo es predecir producción futura, evaluar la confianza de las predicciones y explorar comportamientos operativos con clustering. La evaluación económica queda como extensión simulada.

La etapa actual es revisión y exploración de datos. El usuario eligió un esqueleto simple que se ampliará cuando sea necesario. No incorporar anticipadamente paquetes internos, modelos, dashboard, infraestructura ni dependencias sin una necesidad concreta del trabajo solicitado.

## Antes de trabajar

1. Leer `README.md` y `docs/HANDOFF.md`.
2. Revisar el estado real de los archivos; el handoff describe una sesión anterior y puede quedar desactualizado.
3. Consultar `docs/WELLWISE-X_Proyecto_y_Cronograma.docx` cuando se necesite contexto metodológico y `data/bronze/Diccionario_Volve.xlsx` para interpretar variables.

## Organización

- `data/bronze/`: fuentes originales. No sobrescribir ni limpiar estos archivos.
- Bronze conserva los Excel originales sin transformación, con inventario SHA-256. Silver y Gold quedan pendientes: no crear carpetas, esquemas, transformaciones ni datasets de esas capas hasta definirlos con el usuario.
- `notebooks/`: análisis en orden numérico, con celdas reproducibles y explicaciones en español.
- `docs/`: contexto y continuidad del proyecto.
- `outputs/`: tablas y gráficos exportados.
- `requirements.txt`: dependencias necesarias. No añadir herramientas por anticipación.

Usar rutas relativas a la raíz del proyecto y permitir ejecutar notebooks desde la raíz o desde `notebooks/`. No dejar copias de datasets en la raíz. Los archivos temporales de Excel (`~$*`) no son fuentes de datos; no manipularlos mientras la aplicación los esté usando.

## Criterios de análisis

- Separar producción e inyección y revisar coherencia entre `FLOW_KIND` y `WELL_TYPE`.
- Conservar códigos de identificación como texto.
- En la hoja mensual, separar la fila de unidades de los registros y tratar el texto `NULL` como faltante.
- No reemplazar faltantes por cero ni eliminar paradas o atípicos automáticamente.
- Revisar claves, fechas, huecos de calendario y disponibilidad por pozo antes de crear variables temporales.
- No asumir que el siguiente registro corresponde al siguiente día.
- No dar por confirmadas las unidades que el diccionario señala como pendientes. Volumen diario y caudal por hora operativa no son equivalentes.
- Documentar cualquier filtro, imputación o transformación y preservar trazabilidad al original.
- Distinguir observación, asociación y causalidad. Los archivos actuales no aportan un catálogo estructurado de intervenciones, costos y resultados causales.

Cuando se avance a modelos: definir objetivo y momento de decisión; usar evaluación temporal, transformaciones ajustadas sólo con entrenamiento e información disponible al momento de predecir. Medir error, cobertura y ancho de intervalos y riesgo frente a cobertura. ACT no autoriza una intervención física: si se conserva esa etiqueta, sólo significa aceptar una predicción. INVESTIGATE significa revisión humana, sin promesa de valor económico.

## Verificación y comunicación

Mantener los cambios proporcionales a la tarea. Para cambios en notebooks, verificar las celdas afectadas y la ejecución secuencial cuando corresponda. Para documentación, revisar rutas, consistencia y afirmaciones; no crear pruebas artificiales.

No afirmar que se entrenaron modelos, se ejecutó Jupyter o se guardaron resultados si eso no ocurrió. Informar qué se verificó y cualquier bloqueo real. Evitar borrar trabajo existente; antes de retirar duplicados de datos, comparar su contenido y conservar la fuente canónica.

## Continuidad

Actualizar `docs/HANDOFF.md` al completar cambios relevantes: estado actual, verificaciones, decisiones, pendientes y siguiente paso sugerido. Mantenerlo breve y factual; no usarlo como copia del historial de conversación. Las instrucciones actuales del usuario prevalecen sobre este documento y el handoff.
