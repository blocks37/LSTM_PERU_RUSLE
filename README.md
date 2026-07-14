# Predicción de erosión hídrica en la cuenca del río Lurín

Integración de una red neuronal **LSTM multihorizonte** con el modelo espacial
**RUSLE** para proyectar la precipitación mensual, estimar el factor de
erosividad de la lluvia (R) y analizar la pérdida potencial de suelo en la
cuenca del río Lurín, Perú.

## Alcance

El notebook implementa el siguiente flujo:

1. Carga y depuración de precipitación mensual desde un archivo NetCDF.
2. Análisis exploratorio, climatología y descomposición STL.
3. Preprocesamiento con `log1p`, anomalías estacionales y escalamiento.
4. Entrenamiento de una LSTM con 24 meses de entrada y 12 meses de salida.
5. Evaluación hidrológica y análisis de residuos.
6. Pronóstico mensual 2026–2030 mediante un ensamble de semillas.
7. Estimación anual del factor R.
8. Cálculo espacial de RUSLE (`A = R · K · LS · C · P`) en Google Earth Engine.
9. Exportación de tablas, figuras y un reporte consolidado.

> **Importante:** el repositorio contiene el código, pero no distribuye el
> NetCDF de precipitación ni los activos de Earth Engine. Cada fuente conserva
> sus propias condiciones de acceso, uso y citación.

## Estructura

```text
tesis-lstm-rusle-lurin/
├── notebooks/
│   └── Tesis_LSTM_RUSLE_Lurin_v4.ipynb
├── data/
│   └── README.md
├── figures/
│   └── README.md
├── outputs/
│   └── README.md
├── .env.example
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Requisitos

- Python 3.10 o 3.11.
- Acceso a Google Earth Engine.
- Archivo NetCDF `PISCOp_m.nc` o una base equivalente con las variables y
  dimensiones descritas en [`data/README.md`](data/README.md).
- Un activo vectorial de la cuenca del río Lurín accesible desde Earth Engine.

## Instalación local

```bash
git clone https://github.com/TU_USUARIO/tesis-lstm-rusle-lurin.git
cd tesis-lstm-rusle-lurin
python -m venv .venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
cp .env.example .env
jupyter lab
```

## Publicación en GitHub

Crea un repositorio vacío en GitHub con el nombre `tesis-lstm-rusle-lurin` y,
desde esta carpeta, ejecuta:

```bash
git init -b main
git add .
git commit -m "Versión inicial del modelo LSTM-RUSLE"
git remote add origin https://github.com/TU_USUARIO/tesis-lstm-rusle-lurin.git
git push -u origin main
```

No marques la opción de crear automáticamente un README, `.gitignore` o
licencia en GitHub, porque esos archivos ya están incluidos.

Edita `.env` con tus rutas y recursos. Si ejecutas Jupyter desde una terminal,
puedes cargar sus valores antes de abrirlo:

```bash
set -a
source .env
set +a
```

En Windows PowerShell, define las mismas variables con `$env:NOMBRE="valor"`.

## Ejecución en Google Colab

1. Abre `notebooks/Tesis_LSTM_RUSLE_Lurin_v4.ipynb` en Colab.
2. Sube el NetCDF y define `PISCO_NC_PATH` con su ubicación.
3. Define `EE_PROJECT` y `CUENCA_ASSET` en el entorno.
4. Ejecuta las celdas en orden y completa la autenticación de Earth Engine.

Ejemplo de configuración dentro de una sesión temporal de Colab:

```python
import os

os.environ["PISCO_NC_PATH"] = "/content/PISCOp_m.nc"
os.environ["EE_PROJECT"] = "tu-proyecto-gee"
os.environ["CUENCA_ASSET"] = "projects/tu-proyecto/assets/cuenca_lurin"
```

## Entradas principales

| Recurso | Uso | Configuración |
|---|---|---|
| NetCDF mensual | Serie de precipitación 1981–2025 | `PISCO_NC_PATH` |
| Proyecto Earth Engine | Cómputo geoespacial | `EE_PROJECT` |
| Activo de la cuenca | Delimitación del análisis | `CUENCA_ASSET` |
| SoilGrids | Factor K | Catálogo de Earth Engine |
| SRTM e HydroSHEDS | Factor LS | Catálogo de Earth Engine |
| Landsat 8 | Factor C | Catálogo de Earth Engine |
| ESA WorldCover | Factor P | Catálogo de Earth Engine |

## Salidas esperadas

La ejecución genera series y métricas en CSV, gráficos de diagnóstico,
pronósticos, factores R y mapas de erosión para 2026–2030. Las salidas no se
versionan porque pueden ser pesadas y deben poder regenerarse desde el notebook.

## Reproducibilidad y advertencias

- La semilla principal se fija en `42`; el módulo de incertidumbre utiliza un
  ensamble de diez semillas.
- Los productos de Earth Engine pueden cambiar si se actualizan las colecciones
  de origen o los permisos del activo de la cuenca.
- RUSLE estima pérdida media potencial de suelo por erosión laminar y en
  surcos; no representa directamente erosión en cárcavas ni transporte y
  deposición de sedimentos.
- Antes de publicar resultados, valida unidades, resolución espacial,
  representatividad del punto meteorológico y aplicabilidad local de las
  funciones usadas para R, K, LS, C y P.

## Licencia

El código se distribuye bajo la licencia MIT. Los datos y productos externos no
quedan cubiertos por esta licencia.

## Cómo citar

Si utilizas este trabajo, cita el repositorio y la tesis asociada indicando
autor, año, título, institución, versión del código y fecha de consulta. Completa
estos datos en GitHub cuando la ficha bibliográfica definitiva esté disponible.
