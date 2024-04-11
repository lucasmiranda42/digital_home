---
title: "DeepOF"
summary: "DeepOF es una herramienta de postprocesado para series temporales de datos de comportamiento animal, obtenidas a través de DeepLabCut o SLEAP."
series: ["Software"]
weight: 1
aliases: ["/software/DeepOF/"]
tags: ["Software", "DeepOF", "DeepLabCut", "SLEAP", "Roedores", "Series temporales", "Postprocesado", "Videos", "Python", "Jupyter", "Machine Learning", "Deep Learning", "Comportamiento animal"]
author: ["Lucas Miranda"]
cover:
  image: deepof_cover.png
  alt: "DeepOF logo with text"
---

### DeepOF es un paquete de Python para procesar datos de trackeo de animales, obtenidos con [DeepLabCut](https://www.mackenziemathislab.org/deeplabcut#:~:text=DeepLabCut%E2%84%A2%20is%20an%20efficient,typically%2050%2D200%20frames) o [SLEAP](https://sleap.ai/).

Las herramientas que tenemos disponibles para cuantificar lo que hacen los animales evolucionaron significativamente en la última década. Desde la observación de animales en la naturaleza, la etología se desplazó significativamente hacia pruebas más simples y controladas en el laboratorio, generalmente midiendo respuestas sencillas a ciertos estímulos.

Avances recientes en visión por computadora y aprendizaje automático permitieron a los investigadores rastrear en detalle el movimiento de animales tanto en el laboratorio como en la naturaleza, sin la necesidad de etiquetas físicas. Típicamente, estos programas se basan en _transfer learning_ (aprendizaje por transferencia) a partir de redes neuronales preentrenadas en grandes conjuntos de datos de imágenes, que primero extraen representaciones de los fotogramas de video. Al etiquetar relativamente pocos fotogramas con partes del cuerpo de interés (unos 200 suelen bastar), las redes pueden luego ajustarse para rastrear estas partes del cuerpo en todos los videos a disposición. Abajo podés ver el flujo de trabajo general del [artículo original de DeepLabCut](https://www.nature.com/articles/s41593-018-0209-y). Modelos más modernos pueden rastrear partes del cuerpo de interés [incluso sin etiquetado previo](https://arxiv.org/abs/2203.07436), aunque siguen siendo menos precisas que estos métodos supervisados.

![Supervised pipeline in DeepOF](../deeplabcut.png)

De esta forma, transformando el video crudo en series temporales de partes del cuerpo rastreadas, estos programas (incluyendo los mencionados [DeepLabCut](https://www.mackenziemathislab.org/deeplabcut#:~:text=DeepLabCut%E2%84%A2%20is%20an%20efficient,typically%2050%2D200%20frames) y [SLEAP](https://sleap.ai/)) allanaron el camino para herramientas de análisis que pueden etiquetar y caracterizar automáticamente el comportamiento de muchas maneras innovadoras.

DeepOF es nuestra humilde contribución a este esfuerzo. El programa ofrece tanto funciones de anotación tanto supervisada como no supervisada, que permiten a los investigadores probar hipótesis sobre condiciones experimentales como el estrés, mutaciones genéticas y sexo, de una manera flexible.

En primer lugar, la anotación supervisada incluido usa una serie de reglas y modelos de aprendizaje automático preentrenados para detectar cuándo los animales que se están rastreando muestran alguno de una serie de patrones de comportamiento predefinidos.

![Supervised pipeline in DeepOF](../deepof_supervised.png)

De esta manera, podemos etiquetar tanto comportamientos individuales como sociales con solo unas pocas líneas de código! Veamos un ejemplo básico:

```python
import deepof.data
my_deepof_project_raw = deepof.data.Project(
                project_path=os.path.join("tutorial_files"),
                video_path=os.path.join("tutorial_files/Videos/"),
                table_path=os.path.join("tutorial_files/Tables/"),
                project_name="deepof_example_project",
)
supervised_annotation = my_deepof_project.supervised_annotation()
```

Así de fácil, anotamos todos nuestros videos con un conjunto de modelos preentrenados que pueden detectar comportamientos tales como encogerse, trepar, y distintas interacciones sociales.

Además, como mencionamos arriba, DeepOF también viene con un flujo de trabajo no supervisado, que modelos modelos de agrupamiento profundo para descubrir patrones de comportamiento sin definición previa, que también se puede ejecutar con unos pocos comandos:

```python
# Continuando desde arriba, creamos un conjunto de coordenadas para agrupar,
# removiendo variación posicional y rotacional que no nos interesa
coords = my_deepof_project.get_coords(center="Center", align="Spine_1")

# Luego preprocesamos estas coordenadas para hacerlas compatibles con el clustering
preprocessed_coords, global_scaler = coords.preprocess()

# Finalmente, entrenamos un modelo de clustering con estas coordenadas preprocesadas
trained_model = my_deepof_project.deep_unsupervised_embedding(
  preprocessed_object=preprocessed_coords
)
```

Por último, DeepOF incluye un pipeline de interpretabilidad para explorar qué son estos clusters obtenidos, basándonos tanto en Shapley Additive Explanations (SHAP) como en mapeos directos de clusters a fragmentos de video. Además, independientemente del tipo de análisis que elijas, DeepOF te ofrece un extenso conjunto de herramientas de análisis y visualización post-hoc. A continuación podés ver los resultados de SHAP para un cluster enriquecido en animales expuestos a estrés crónico, junto a fragmentos de video correspondientes.

![Unsupervised pipeline in DeepOF](../deepof_unsupervised.gif)

¡Eso es todo por ahora! En resumen, DeepOF es una herramienta versátil que puede ayudarte a extraer información significativa de tus datos de comportamiento animal. ¡Esperamos que la encuentres tan útil como nosotros!

### ¿Y ahora qué?

DeepOF está disponible en [GitHub](https://github.com/mlfpm/deepof) y [PyPI](https://pypi.org/project/deepof/). [Acá](https://deepof.readthedocs.io/en/latest/) podés encontrar documentación detallada, incluyendo instalación en distintos sistemas operativos y tutoriales. Si te interesa entrar en detalle, podés leer nuestras dos publicaciones. La primera es sobre el [software en sí](https://joss.theoj.org/papers/10.21105/joss.05394), mientras que la segunda describe cómo usamos DeepOF para [caracterizar un modelo de ratón de estrés crónico](https://www.nature.com/articles/s41467-023-40040-3), para la cual también hay un [blog post]()!.