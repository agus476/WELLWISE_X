# Handoff — WELLWISE-X

Actualizado: 2026-09-30.

## Alcance vigente

Predicción de producción, evaluación de confianza/abstención y clustering de comportamientos operativos. El plan incluye regresión y clasificación de caída futura, con etiqueta pendiente y confirmación de los requisitos supervisados con la tutora. Economía, VOI, OOD adicional y dashboard son extensiones. ACT, si se conserva, significa aceptar una predicción para consulta, sin recomendar intervenciones.

El detalle vigente está en `docs/ALCANCE.md` y `docs/WELLWISE-X_Proyecto_y_Cronograma.docx`. Sustituyen el alcance anterior, recuperable en Git.

## Implementado

- Bronze en `data/bronze/`: los dos Excel originales trasladados desde `data/raw/`, README de capa y manifiesto de procedencia conocida, tamaños y SHA-256.
- Notebooks 01 y 02: rutas de lectura actualizadas a Bronze. Código restante, salidas y notas existentes preservados.
- README, AGENTS y cronograma actualizados. Eliminado únicamente el marcador de la antigua carpeta vacía `data/processed/`.
- Silver y Gold pendientes: sin carpetas, esquemas ni transformaciones implementadas. No hay modelos ni datos limpios nuevos.

## Verificación de esta actualización

Los SHA-256 de ambos Excel coinciden con los originales del commit anterior. Se verificó que las únicas modificaciones en las celdas fueran las rutas y que las salidas guardadas permanecieran idénticas. Se comprobó la lectura diaria y mensual desde raíz y desde notebooks/; en esa comprobación se omitió exclusivamente la presentación IPython porque no está instalada en el runtime de validación. No se ejecutaron de nuevo los notebooks completos ni el profiling.

El Word se renderizó y sus cuatro páginas se revisaron visualmente. No se añadieron dependencias.

## Datos y pendientes

15.634 registros diarios: 9.161 de producción y 6.473 de inyección; siete pozos totales y seis con registros de producción. Mensual: 526 registros sin fila de unidades. Los controles previos señalan 18 cruces production/WI, horas fuera de rango, huecos de calendario y faltantes variables por pozo; revisar los notebooks antes de decidir exclusiones. Las unidades pendientes siguen sin confirmar.

Siguiente paso: revisar el EDA por pozo, acordar unidades, reglas de calidad y calendario, objetivo/horizonte y etiqueta supervisada. Con esas decisiones se podrá diseñar Silver. La reconciliación diaria/mensual sigue pendiente de incorporarse al notebook.
