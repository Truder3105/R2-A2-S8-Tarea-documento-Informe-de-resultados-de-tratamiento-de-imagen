# Informe de resultados de tratamiento de imagen

<p align="center">
  <strong>R2-A2-S8 · Tarea documento</strong><br>
  Análisis de condiciones de captura, diagnóstico de calidad y tratamiento de imágenes con OpenCV
</p>

<p align="center">
  <a href="Informe/Informe_R2-A2-S8_Tratamiento_de_Imagen.pdf">Informe PDF</a> ·
  <a href="informe_tratamiento_imagen_practica.ipynb">Cuaderno ejecutable</a> ·
  <a href="Fotos/">Conjunto de fotografías</a>
</p>

Este repositorio documenta una práctica de **percepción computacional** sobre cómo la iluminación y otras condiciones de captura afectan la calidad de una imagen. Se analiza un conjunto de fotografías de un mismo objeto, se identifica un caso problemático mediante indicadores medibles y se aplica un tratamiento elegido para responder a ese diagnóstico.

El proyecto incluye el informe académico en PDF, el cuaderno Jupyter con el código y sus salidas ejecutadas, las fotografías originales y el archivo comprimido de origen. El flujo permite inspeccionar tanto las decisiones técnicas como los resultados visuales y cuantitativos.

## Contenido

| Recurso | Descripción |
| --- | --- |
| [Informe en PDF](Informe/Informe_R2-A2-S8_Tratamiento_de_Imagen.pdf) | Documento de resultados, metodología, análisis, figuras, conclusiones y referencias. |
| [Cuaderno práctico](informe_tratamiento_imagen_practica.ipynb) | Implementación en Python/OpenCV. Contiene celdas ejecutadas y salidas que documentan el análisis. |
| [`Fotos/`](Fotos/) | Dieciséis archivos JPEG originales utilizados en la práctica. |
| [`Fotos/a.rar`](Fotos/a.rar) | Archivo comprimido que contiene el conjunto de fotografías. |

## Objetivo

Evaluar cómo la iluminación influye en la utilización de imágenes dentro de un sistema de percepción computacional y demostrar que el tratamiento debe partir de un problema observado. La práctica no busca aplicar filtros indiscriminadamente: compara alternativas, justifica una decisión y examina sus efectos y limitaciones.

## Conjunto de datos

El conjunto está compuesto por **16 fotografías** del mismo objeto, organizadas en el análisis como dos grupos descriptivos de ocho imágenes: baja iluminación e iluminación ambiente. Las imágenes son JPEG, RGB y tienen resolución de **4000 × 3000 píxeles (12 MP)**. El cuaderno genera un inventario por archivo, incluyendo dimensiones, tamaño y metadatos EXIF.

La comparación entre grupos sirve como diagnóstico de este conjunto; no constituye un experimento controlado de iluminación. Las fotografías también pueden variar en encuadre, ángulo, reflejos y otros detalles de captura. Por ello, los resultados deben interpretarse como evidencia del caso estudiado, no como una regla universal para cualquier cámara u objeto.

## Flujo de trabajo

1. **Carga e inventario:** localiza las imágenes, asigna identificadores y revisa formato, dimensiones, tamaño y EXIF.
2. **Diagnóstico:** calcula brillo medio, contraste, porcentaje de sombras y píxeles saturados, entropía, estimación de ruido, varianza del Laplaciano y características de bordes.
3. **Selección del caso:** identifica `I01` como la imagen problemática por su bajo brillo y alta proporción de píxeles en sombra; la contrasta visualmente con `I10`, una toma mejor expuesta de la misma cara del objeto.
4. **Comparación de alternativas:** evalúa ecualización global de luminancia, CLAHE, corrección gamma y combinaciones con reducción de ruido.
5. **Tratamiento elegido:** aplica Non-Local Means para reducir ruido y una corrección gamma calculada a partir del brillo de referencia del grupo de iluminación ambiente.
6. **Evaluación:** compara la imagen original y la tratada con métricas, histogramas, una región de interés del panel de conectores y mapas de bordes Canny.
7. **Aplicación al grupo:** ejecuta el mismo esquema de tratamiento sobre las ocho imágenes clasificadas como baja iluminación y registra sus cambios.

## Resultados destacados

| Indicador en `I01` | Original | Tratada | Lectura |
| --- | ---: | ---: | --- |
| Brillo medio (escala de gris 0–255) | 44,92 | 85,58 | La imagen se aclara y se aproxima al rango de referencia. |
| Píxeles en sombra (< 30) | 45,70 % | 8,49 % | Disminuyen las zonas subexpuestas. |
| Entropía (bits) | 6,790 | 7,286 | Aumenta la diversidad tonal registrada. |
| Ruido estimado (σ) | 1,126 | 0,584 | Disminuye la respuesta de alta frecuencia estimada. |
| Distancia de Bhattacharyya al histograma del grupo ambiente | 0,434 | 0,287 | La distribución tonal se acerca a la referencia usada. |

En el promedio del conjunto, las imágenes del grupo de baja iluminación pasan de **62,94 a 89,24** en brillo y de **30,17 % a 9,31 %** en píxeles de sombra después del tratamiento. Las imágenes de referencia del grupo con iluminación ambiente presentan un brillo medio de **97,78** y **10,63 %** de sombras.

Las cifras son resultados del cuaderno incluido y dependen del conjunto, de los parámetros elegidos y de las definiciones operativas de cada métrica. El estimador de ruido de alta frecuencia también puede responder a textura y bordes; no equivale a una medición aislada del ruido físico del sensor. El suavizado puede atenuar detalles finos. Además, la mejora de métricas de imagen **no demuestra por sí sola una mejora en la precisión de un clasificador**: este proyecto no entrena ni evalúa una CNN.

## Requisitos

- Python 3 y Jupyter Notebook o JupyterLab (también puede usarse Google Colab).
- Bibliotecas: `opencv-python` (`cv2`), `numpy`, `pandas`, `matplotlib` y `Pillow` (`PIL`).
- Para extraer el archivo RAR en una ejecución local, una herramienta compatible como **UnRAR**, **unar** o **7-Zip**. Alternativamente, se pueden extraer previamente las imágenes en una carpeta llamada `imagenes`.

El cuaderno registra las versiones utilizadas en la ejecución incluida: Python 3.12.14, OpenCV 5.0.0, NumPy 2.5.4 y pandas 3.0.1. Esas versiones describen la ejecución documentada; la compatibilidad puede variar según el sistema operativo y el entorno de Jupyter.

## Cómo ejecutar

### En un entorno local

1. Clona el repositorio y abre una terminal en su carpeta raíz.

   ```bash
   git clone https://github.com/Truder3105/R2-A2-S8-Tarea-documento-Informe-de-resultados-de-tratamiento-de-imagen.git
   cd R2-A2-S8-Tarea-documento-Informe-de-resultados-de-tratamiento-de-imagen
   ```

2. Instala las bibliotecas de Python si aún no están disponibles en el entorno:

   ```bash
   python -m pip install opencv-python numpy pandas matplotlib Pillow
   ```

3. Para que el cuaderno encuentre las entradas, elige una de estas opciones:
   - Copia `Fotos/a.rar` a la carpeta raíz del repositorio y asegúrate de contar con UnRAR, unar o 7-Zip disponible en el sistema; o
   - Extrae las fotografías dentro de una carpeta `imagenes/` en la raíz del repositorio. Se admite que estén en subcarpetas.

4. Abre `informe_tratamiento_imagen_practica.ipynb` desde la raíz del proyecto y ejecuta las celdas en orden. El cuaderno crea `resultados/` y guarda allí las imágenes original y tratada de `I01`.

> La ruta absoluta del equipo donde se preparó el cuaderno es específica de ese entorno. Para reproducir el análisis en otro equipo, utiliza una de las dos opciones de entrada anteriores.

### En Google Colab

1. Sube el cuaderno a Colab y ábrelo.
2. Ejecuta las celdas en orden.
3. Cuando el cuaderno lo solicite, carga `Fotos/a.rar` desde tu equipo, o coloca las fotografías sueltas según la indicación de la celda.
4. Descarga o guarda las figuras y los archivos generados en `resultados/` si necesitas conservarlos fuera de la sesión.

## Archivos generados

Al ejecutar el cuaderno se genera el directorio `resultados/`, con `I01_original.png` y `I01_tratada.png`. Las figuras de diagnóstico, histogramas y comparaciones se muestran como salidas del notebook. El cuaderno que ya está en el repositorio conserva las salidas de una ejecución previa; volver a ejecutarlo actualiza las salidas en el entorno activo.

## Estructura del repositorio

```text
├── Fotos/
│   ├── *.jpg.jpeg                         # 16 fotografías originales
│   └── a.rar                              # conjunto comprimido
├── Informe/
│   └── Informe_R2-A2-S8_Tratamiento_de_Imagen.pdf
├── informe_tratamiento_imagen_practica.ipynb
└── README.md
```

## Autores y contexto académico

**Julian Esteban Ballesteros Ortiz** · **Juan Diego Walteros Cortes**  
Universidad de Cundinamarca · Especialización en Inteligencia Artificial  
CADI Percepción Computacional — CAD2202023102  
Docente: Mónica María Fonseca Vigoya  
Fecha indicada en el informe: 13 de octubre de 2026

## Referencias de apoyo

- Planetachatbot. (s. f.). *Cómo empezar con reconocimiento de imagen y redes neuronales convolucionales en 5 minutos*. [Artículo](https://planetachatbot.com/como-empezar-con-reconocimiento-de-imagen-y-redes-neuronales-convolucionales-en-5-minutos/).
- Compression.ai. (s. f.). *How to measure quality of an image?* [Artículo](https://compression.ai/how-to-measure-the-quality-of-an-image/).
- Data Science. (s. f.). *Image Processing Class #5 — Edge and Contour*. [Artículo](https://medium.com/data-science/image-processing-class-egbe443-5-edge-and-contour-d5d410f4483c).
- *Convolutional Neural Networks Explained (CNN Visualized)*. (s. f.). [Video de YouTube](https://www.youtube.com/watch?v=pj9-rr1wDhM).
- OpenCV. (s. f.). *OpenCV documentation*. [Documentación oficial](https://docs.opencv.org/).

## Licencia y uso

El repositorio no declara una licencia de software. Antes de reutilizar o redistribuir el código, el informe o las fotografías, consulta con sus autores y respeta los derechos aplicables a los materiales de terceros incluidos como referencias.
