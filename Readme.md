# Every Click Has an Outcome

> Convertir datos históricos de comercio electrónico en decisiones que mejoren la experiencia del cliente y el desempeño del negocio.

## Descripción del proyecto

El comercio electrónico en Colombia mueve **$145,4 mil millones de dólares** y más de **684 millones de transacciones anuales** (año de referencia: 2025). A pesar de este volumen, muchas organizaciones no aprovechan estos datos de forma estratégica para transformar información en ventajas competitivas.

Este proyecto trabaja sobre el dataset histórico de una plataforma de comercio electrónico que registra transacciones —compra, pago, envío y experiencia del cliente— entre **2022 y 2024**. Actualmente esta información se almacena, pero no se explota de forma sistemática para entender el comportamiento de compra ni anticipar problemas como las devoluciones.

El proyecto busca transformar estos datos históricos en información accionable: identificar patrones de compra, predecir devoluciones de productos y apoyar decisiones de negocio basadas en evidencia.

### Pregunta analítica central

> ¿Cómo pueden los datos históricos de transacciones en una plataforma de comercio electrónico utilizarse para identificar patrones de compra, predecir devoluciones de productos y apoyar la toma de decisiones que mejoren la experiencia del cliente y el desempeño del negocio?

### Enfoque del proyecto

Patrones de compra, predicción de devoluciones y decisiones basadas en evidencia.

### Alineación con los ODS

**ODS 9 · Meta 9.b** — Fortalecer capacidades tecnológicas e innovación para elevar la competitividad de los sectores productivos.

## Contexto del problema

El comercio electrónico en Colombia crece a un ritmo acelerado y genera grandes volúmenes de datos de clientes, productos y transacciones. Aun así, muchas organizaciones no aprovechan esta información de forma estratégica para transformar datos en ventajas competitivas.

Las compras son cada vez más frecuentes y de menor valor, mientras las devoluciones y las ineficiencias logísticas afectan la rentabilidad. Variables como categoría, precio, método de pago, canal, tiempo de entrega y ciudad pueden influir tanto en la devolución de productos como en la satisfacción del cliente.


## Fuentes de datos

**Fuente principal:** Dataset histórico de transacciones de una plataforma de comercio electrónico (`03_comercio_electronico_transacciones.csv`), con **8.080 registros** y **12 variables**.

### Variables disponibles

| Variable | Tipo de dato | Descripción |
|---|---|---|
| `id_transaccion` | Texto (`TXN-#######`) | Identificador único de cada transacción, generado automáticamente al confirmarse la compra |
| `fecha_compra` | Fecha (AAAA-MM-DD) | Fecha en que se realizó la compra, capturada al momento del checkout/pago |
| `id_cliente` | Numérico | Identificador único del cliente |
| `categoria_producto` | Categórica | Categoría del producto comprado (10 categorías) |
| `precio_cop` | Numérica (float) | Precio del producto en pesos colombianos |
| `metodo_pago` | Categórica | Billetera digital, Contraentrega, PSE, Tarjeta crédito, Tarjeta débito |
| `canal` | Categórica | App móvil o Sitio web |
| `dispositivo` | Categórica | Android, Desktop o iOS |
| `ciudad_envio` | Categórica | Ciudad de destino del envío (10 ciudades) |
| `tiempo_entrega_dias` | Numérica (int) | Días que tomó la entrega |
| `calificacion_cliente_1a5` | Numérica | Calificación otorgada por el cliente (escala 1 a 5) |
| `producto_devuelto` | Categórica | Indicador de si el producto fue devuelto (Sí / No) |

## Stakeholders

Áreas y personas que se benefician o toman decisiones a partir de los resultados del proyecto:

| Área | Interés principal |
|---|---|
| **Gerencia General** | Definir estrategias para mejorar la rentabilidad y la competitividad del negocio |
| **Marketing** | Identificar patrones de compra, diseñar campañas más efectivas y aumentar la fidelización |
| **Comercial** | Optimizar estrategias de ventas y conocer las categorías con mayor desempeño |
| **Logística** | Reducir tiempos de entrega y devoluciones mediante una mejor planificación operativa |
| **Servicio al Cliente** | Comprender los factores de satisfacción y mejorar la calidad del servicio |
| **Clientes** | Recibir una experiencia de compra más fluida, entregas eficientes y menos inconvenientes |


## Equipo

| Rol | Integrante | Responsabilidades |
|---|---|---|
| **Líder del proyecto y Científica de Datos** | Ivanna Navas Barreto | Planificación del proyecto, definición metodológica, análisis exploratorio de datos (EDA), desarrollo y evaluación de modelos predictivos, interpretación de resultados y coordinación de la documentación y presentación final. |
| **Ingeniero y Analista de Datos** | Néstor Camargo Pinto | Recolección, limpieza y transformación de datos; preparación del conjunto para modelado; apoyo en el desarrollo de modelos; gestión del repositorio en GitHub y control de versiones. |

## Metodología / Stack tecnológico

El análisis sigue una metodología inspirada en **CRISP-DM**, desarrollada íntegramente en el notebook `Analisis_datos.ipynb`. Fases cubiertas hasta el momento:

1. **Comprensión del negocio** – contexto, objetivo de negocio y pregunta analítica.
2. **Comprensión de los datos** – exploración inicial (`.info()`, `.describe()`), evaluación de completitud, consistencia (formatos, duplicados de texto), trazabilidad (IDs faltantes/repetidos, formato, rangos de fecha) y detección de valores atípicos (IQR global y segmentado por categoría), con visualización mediante raincloud plots.
3. **Preparación de los datos**:
   - **Tratamiento de nulos:** verificación de aleatoriedad (supuesto MCAR) antes de decidir el tratamiento; eliminación listwise de registros sin llaves (`id_transaccion`, `fecha_compra`, `id_cliente`); comparación de eliminación vs. imputación por moda vs. imputación proporcional/estocástica para `dispositivo`.
   - **Tratamiento de duplicados:** identificación y eliminación de duplicados exactos, conservando la primera ocurrencia.
   - **Tratamiento de atípicos:** decisión de conservarlos por ser coherentes con precios reales de mercado.
   - **Normalización de variables categóricas:** unificación de formato de texto (`str.title()`).
4. **Auditoría de sesgos:** comparación estadística (prueba Chi², tamaño del efecto con Cramér's V) entre el dataset original y el dataset final, para confirmar que el proceso de limpieza no distorsionó la representatividad de las variables.
5. **Documentación de decisiones y supuestos:** trazabilidad completa de cada decisión tomada durante la preparación de los datos.

**Herramientas:**
- **Lenguaje:** Python 3
- **Manipulación de datos:** Pandas, NumPy
- **Visualización:** Matplotlib, ptitprince (raincloud plots)
- **Estadística:** SciPy (`chi2_contingency`, Cramér's V)
- **Entorno:** Jupyter Notebook (vía extensión de Jupyter en VS Code)

## Estructura del repositorio

Proyecto_1_C1/
├── docs/                                      # Documentación adicional del proyecto
├── 03_comercio_electronico_transacciones.csv  # Dataset histórico de transacciones
├── Analisis_datos.ipynb                       # Notebook principal de análisis
├── requirements.txt                           # Dependencias del proyecto
└── README.md


## Cómo ejecutar el proyecto

### Requisitos previos

- **Python 3.9+** instalado.
- **Git** instalado.
- **Visual Studio Code** con la extensión **Jupyter** (Microsoft) instalada.
- El archivo `03_comercio_electronico_transacciones.csv` debe estar ubicado en la raíz del repositorio, junto al notebook (así se referencia en el código: `pd.read_csv("03_comercio_electronico_transacciones.csv")`).

### 1. Clonar el repositorio

```bash
git clone https://github.com/Alejandralvn/Proyecto_1_C1.git
cd Proyecto_1_C1
```

### 2. Crear y activar un entorno virtual de Python

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Instalar las dependencias

```bash
pip install -r requirements.txt
```

> El notebook incluye una celda con `!pip install ptitprince` como alternativa. Si ya instalaste las dependencias vía `requirements.txt`, puedes omitir esa celda.

### 4. Registrar el entorno como kernel de Jupyter

```bash
python -m ipykernel install --user --name=venv --display-name "Python (Proyecto_1_C1)"
```

### 5. Abrir y ejecutar el notebook

1. Abre la carpeta del proyecto en **VS Code**.
2. Instala la extensión **Jupyter** (Microsoft) desde el marketplace si no la tienes.
3. Abre `Analisis_datos.ipynb`.
4. Selecciona el kernel **"Python (Proyecto_1_C1)"** en la esquina superior derecha.
5. Ejecuta las celdas en orden con `Shift + Enter`, o usa **"Run All"**.

> Ejecuta las celdas en orden secuencial: el notebook construye progresivamente los DataFrames `df` → `df_limpio` → `df_limpio2` → `df_preparado` → `df_final`, y varias celdas dependen de los DataFrames generados previamente.

### 6. Desactivar el entorno virtual (al terminar)

```bash
deactivate
```
