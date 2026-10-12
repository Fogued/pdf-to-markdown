# PDF to Markdown (Marker + Google Colab)

Cuaderno de Google Colab que convierte PDF, imágenes, DOCX, PPTX, XLSX, HTML y EPUB a Markdown, HTML o JSON con [Marker](https://github.com/VikParuchuri/marker). Puede usar la API gratuita de NVIDIA como LLM para mejorar tablas, fórmulas y texto.

Con salida en Markdown, el resultado se descarga en un ZIP listo para descomprimir en un vault de [Obsidian](https://obsidian.md): una carpeta por documento, con las imágenes en `images/` y las fórmulas en `$...$` y `$$...$$`.

## Uso

1. Abre [`PDF_to_Markdown.ipynb`](PDF_to_Markdown.ipynb) en Google Colab (*Archivo → Abrir cuaderno → GitHub* y pega la URL de este repositorio).
2. Activa la GPU: *Entorno de ejecución → Cambiar tipo de entorno → GPU T4*.
3. Ejecuta las celdas en orden:
   1. Comprobar la GPU.
   2. Instalar Marker (versión fijada `1.10.2`).
   3. Elegir las opciones.
   4. Conectar con la API de NVIDIA (solo si usas el LLM).
   5. Subir **uno o varios** documentos.
   6. Convertir.
   7. Empaquetar y descargar el ZIP.

En el paso 5 eliges el modo: si seleccionas un archivo se convierte solo ese; si seleccionas varios, se convierten todos en lote, cargando los modelos una sola vez.

## Opciones

| Opción | Qué hace |
|---|---|
| `USE_LLM` | Usa el modelo de NVIDIA para mejorar el resultado. Más lento y necesita API key. |
| `NVIDIA_MODEL` | Modelo multimodal (texto + imagen) de [build.nvidia.com](https://build.nvidia.com). |
| `OUTPUT_FORMAT` | `markdown` genera el paquete para Obsidian; `html` y `json` se descargan tal cual los crea Marker. |
| `FORCE_OCR` | Solo si el texto sale corrupto o el PDF es escaneado. |
| `PAGE_RANGE` | Páginas a convertir, empezando en 0 (`0-10`, `0,5-10`). Vacío = todo. |
| `REDO_INLINE_MATH` | Rehace las fórmulas dentro del texto (solo con `USE_LLM`). |

## API key de NVIDIA

Solo hace falta con `USE_LLM = True`. Consíguela gratis en [build.nvidia.com](https://build.nvidia.com) (empieza por `nvapi-`). Puedes guardarla como secreto de Colab con el nombre `NVIDIA_API_KEY` (icono de la llave en la barra lateral) o pegarla cuando el cuaderno la pida. La key no se guarda en el cuaderno.

## Por qué Marker 1.10.2

Marker 2.x necesita Docker (vLLM) o llama.cpp para sus modelos de layout, que no están disponibles en Colab, y falla con `docker binary not found`.
