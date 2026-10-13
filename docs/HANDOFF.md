# Handoff — WELLWISE-X

Actualizado: 2026-10-13.

## Alcance vigente

La pregunta central es si podemos predecir producción futura y reconocer antes de conocer el resultado cuándo un pronóstico será poco confiable. Compararemos el error de pronosticar siempre con el error y la cobertura de una regla que deriva casos inciertos a revisión. El plan también incluye clasificación de caída futura y clustering de comportamientos operativos para cubrir la consigna. Economía y dashboard son extensiones; no se recomiendan intervenciones físicas.

El alcance en lenguaje simple, los límites y el plan están en `docs/ALCANCE.md` y en su versión Word `docs/WELLWISE-X_Proyecto_y_Cronograma.docx`. Sustituyen las formulaciones anteriores, recuperables en Git.

## Implementado

- Bronze en `data/bronze/`: los dos Excel originales trasladados desde `data/raw/`, README de capa y manifiesto de procedencia conocida, tamaños y SHA-256.
- Notebooks 01 y 02: rutas de lectura actualizadas a Bronze. Código restante, salidas y notas existentes preservados.
- README, AGENTS y documento de alcance alineados con la pregunta central. Eliminado únicamente el marcador de la antigua carpeta vacía `data/processed/`.
- Silver y Gold pendientes: sin carpetas, esquemas ni transformaciones implementadas. No hay modelos ni datos limpios nuevos.

## Verificación de esta actualización

Los SHA-256 de ambos Excel coinciden con los originales del commit anterior. Se verificó que las únicas modificaciones en las celdas fueran las rutas y que las salidas guardadas permanecieran idénticas. Se comprobó la lectura diaria y mensual desde raíz y desde notebooks/; en esa comprobación se omitió exclusivamente la presentación IPython porque no está instalada en el runtime de validación. No se ejecutaron de nuevo los notebooks completos ni el profiling.

El Word vigente se renderizó y sus dos páginas se revisaron visualmente el 13/10. Se comprobó que el README, el alcance y las instrucciones usan la misma pregunta central. No se añadieron dependencias ni se modificaron los datos o notebooks.

## Datos y pendientes

15.634 registros diarios: 9.161 de producción y 6.473 de inyección; siete pozos totales y seis con registros de producción. Mensual: 526 registros sin fila de unidades. Los controles previos señalan 18 cruces production/WI, horas fuera de rango, huecos de calendario y faltantes variables por pozo; revisar los notebooks antes de decidir exclusiones. Las unidades pendientes siguen sin confirmar.

Siguiente paso: completar el EDA por pozo, acordar unidades, reglas de calidad y calendario, objetivo/horizonte y etiqueta supervisada. Con esas decisiones se podrá diseñar Silver. La reconciliación diaria/mensual sigue pendiente de incorporarse al notebook.
