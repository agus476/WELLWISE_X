# Bronze

Esta capa conserva los Excel originales, sin alterar bytes, hojas, unidades, valores ni filas. Los archivos se trasladaron desde `data/raw/`; su historial permanece en Git. `manifest.json` identifica cada fuente, tamaño y SHA-256. La fecha corresponde al registro en esta estructura, no a una descarga de Equinor.

Los notebooks leen Bronze y realizan controles en memoria. Separar la fila de unidades mensual para analizarla no modifica el original ni produce Silver. Los HTML y gráficos son reportes exploratorios en `outputs/`, no productos Gold.

No sobrescribir las fuentes. Una futura versión debe conservar la anterior y actualizar su trazabilidad. No añadir datos limpios, imputaciones, variables derivadas ni resultados de modelos en esta carpeta.

Silver y Gold quedan pendientes de diseño; no se crean carpetas ni contratos de esas capas en esta etapa. No se incorpora un pipeline ETL, Spark ni servicios externos para mover dos archivos locales.
