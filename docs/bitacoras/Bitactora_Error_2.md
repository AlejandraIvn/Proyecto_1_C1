# BITÁCORA DE ERROR #2
## Auditoría del Pipeline de Datos

**Proyecto:** *Every Click Has an Outcome* · Unidad 2 – Calidad de datos y auditoría de sesgos

**Integrantes:**
- Ivanna Navas Barreto — Líder del proyecto y Científica de Datos
- Néstor Camargo Pinto — Ingeniero y Analista de Datos


## 1. Contexto

Durante la Unidad 2 construimos un pipeline de limpieza para preparar un conjunto de datos histórico de una plataforma de comercio electrónico. El objetivo fue mejorar la calidad de la información sin cambiar la realidad de los datos. Antes de continuar con el modelado verificamos que las decisiones de limpieza no introdujeran sesgos ni modificaran de forma importante la distribución del dataset.


## 2. Decisiones de limpieza que podían afectar la distribución

| Decisión | ¿Qué hicimos? | Resultado |
|---|---|---|
| Normalización de texto | Unificamos `categoria_producto` y `ciudad_envio` porque existían valores escritos de diferentes formas (por ejemplo MODA y Moda). | Las frecuencias cambiaron ligeramente porque varias categorías se unieron, pero no se perdió información. |
| Cambio de tipo de dato | `id_cliente` pasó de float64 a Int64. | Solo cambió el formato de almacenamiento, no los valores. |
| Eliminación de nulos | Se eliminaron 741 registros con valores nulos en llaves críticas. | Estas variables identifican las transacciones y no era correcto inventar información. |
| Imputación de dispositivo | Comparamos eliminar registros, imputar por moda e imputación proporcional. | Elegimos la imputación proporcional porque conservó los registros y mantuvo la distribución original. |
| Duplicados | Se eliminaron 72 registros exactamente iguales. | Se evitó contar la misma transacción más de una vez. |
| Outliers de precio | Se detectaron con IQR por categoría y se conservaron. | Representaban precios reales del negocio. |
| Outliers de entrega | Se detectaron con IQR y también se conservaron. | No correspondían a errores de captura. |


## 3. Auditoría básica de sesgo

Después de terminar la limpieza revisamos si alguna decisión había cambiado la representatividad de los datos. Para esto comparamos las distribuciones antes y después de cada transformación y aplicamos pruebas estadísticas cuando fue necesario.

| Variable | Resultado | Cambio observado | Conclusión |
|---|---|---|---|
| `categoria_producto` | p<0.001 | <1 punto porcentual | Cambio explicado por unificación de nombres. |
| `ciudad_envio` | Chi²=383.68, V=0.1581 | 1.48 pp | Efecto pequeño, no representa un sesgo importante. |
| `metodo_pago` | p=0.9996 | 0.10 pp | Sin cambios relevantes. |
| `canal` | p=0.8629 | 0.15 pp | Distribución conservada. |
| `dispositivo` | p=0.9209 | 0.22 pp | La imputación proporcional mantuvo las proporciones. |
| `producto_devuelto` | p=1.0000 | 0.01 pp | Sin diferencias. |

El único resultado que inicialmente llamó la atención fue `ciudad_envio` porque la prueba Chi-cuadrado fue significativa. Sin embargo, al calcular el V de Cramér obtuvimos 0.1581, lo que indica un efecto pequeño. En otras palabras, el cambio fue consecuencia de corregir nombres escritos de forma diferente y no de alterar la composición de la muestra.

**Respuesta a la auditoría:** no encontramos evidencia de sobre o subrepresentación de grupos poblacionales ni de sesgos de muestreo o de reporte introducidos por el pipeline de limpieza. Las diferencias observadas fueron pequeñas y corresponden a mejoras en la calidad de los datos.


## 4. Ajustes de mejora aplicados

- Usar IQR por categoría para detectar precios atípicos en lugar de un IQR global.
- Seleccionar la imputación proporcional para dispositivo después de comparar tres métodos.
- Verificar el mecanismo de datos faltantes antes de decidir entre eliminar o imputar registros.


## 5. Relación con O'Neil (2016)

Uno de los principios planteados por Cathy O'Neil es que las decisiones tomadas sobre los datos deben ser transparentes y revisadas para evitar introducir sesgos. Por esta razón documentamos cada transformación, comprobamos su impacto y justificamos por qué era la mejor alternativa.


## 6. Conclusiones

El pipeline permitió obtener un conjunto de datos más limpio, consistente y confiable para las siguientes etapas del proyecto. La auditoría confirmó que las decisiones tomadas mejoraron la calidad del dataset sin modificar de forma importante su distribución. Esto nos da mayor confianza para utilizar estos datos en análisis y modelos predictivos relacionados con el comercio electrónico.