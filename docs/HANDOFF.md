# Handoff — WELLWISE-X

Actualizado: 2026-10-13.

## Alcance vigente

Predecir producción futura y reconocer de antemano cuándo conviene desconfiar del pronóstico. Comparar el error de pronosticar siempre con el error y la proporción de casos respondidos bajo una regla de revisión. Clasificación de caída futura y clustering completan la consigna. El alcance se explica en `docs/ALCANCE.md` y en su versión Word.

## Estado

- Los dos Excel originales están en `data/raw/`, sin cambiar sus bytes; `manifest.json` conserva tamaños y SHA-256. La carpeta anterior `data/bronze/` se retiró.
- Los notebooks 01 y 02 leen desde `data/raw/`. El 01 crea `reports/` al generar HTML; esa carpeta no se versiona. Se retiró `outputs/`, que sólo contenía un marcador vacío.
- No hay datos preparados persistidos, modelos entrenados ni conclusiones finales del análisis por pozo.

## Verificación y siguiente paso

Los SHA-256 y tamaños de ambos Excel coinciden con el manifiesto. Se ejecutó la primera celda de carga de ambos notebooks desde la raíz y desde `notebooks/`: 15.634 filas diarias y 526 mensuales. En esta comprobación se omitió sólo la importación de presentación de IPython, que no está instalada en el entorno de validación. No se ejecutaron los notebooks completos ni el profiling. El Word se renderizó y se revisaron sus dos páginas. El siguiente paso es completar el análisis por pozo, decidir objetivo y plazo del pronóstico, y documentar reglas para fechas, paradas, faltantes y unidades. La reconciliación diaria/mensual sigue pendiente.
