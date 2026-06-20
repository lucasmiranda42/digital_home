---
title: "CellPick"
summary: Una herramienta gráfica para selección estratégica y anotación espacial de células en proteómica espacial.
series: ["Software"]
weight: 1
aliases: ["/software/CellPick/"]
tags: ["Software", "CellPick", "Deep Visual Proteomics", "Spatial Transcriptomics"]
author: ["Lucas Miranda"]
website_es: https://cellpick.app
docs_es: https://cellpick.readthedocs.io/en/latest/
code_es: https://github.com/BorgwardtLab/CellPick
cover:
  image: /cellpick_cover.png
  alt: "Logo de CellPick"
---

### Selección estratégica y anotación posicional de células para proteómica espacial

[Sitio web](https://cellpick.app) · [Documentación](https://cellpick.readthedocs.io/en/latest/) · [Código](https://github.com/BorgwardtLab/CellPick)

[CellPick](https://cellpick.app) es una herramienta gráfica para seleccionar células en flujos de trabajo de microdisección láser, como [Deep Visual Proteomics](https://www.nature.com/articles/s41587-022-01302-5). Estos experimentos pueden medir miles de proteínas por célula, pero solo para un número limitado de células por sección de tejido. Por eso, elegir un subconjunto representativo y no contiguo de células es un paso central: una selección aleatoria puede agrupar células en zonas densas, perder regiones poco representadas y seleccionar células vecinas que podrían dañarse durante la microdisección.

CellPick formula esta tarea como un problema de optimización combinatoria y lo presenta en una interfaz interactiva que no requiere programar. La herramienta permite cargar imágenes de microscopía multicanal, máscaras de segmentación celular y etiquetas opcionales; definir regiones activas; y seleccionar células distribuidas a lo largo del tejido, evitando formas contiguas siempre que sea posible. El software soporta formatos de imagen comunes como TIFF, CZI y Zarr, además de integrarse de forma nativa con SpatialData y el ecosistema scverse.

![Resumen del flujo de trabajo de CellPick](/en/software/cellpick/cellpick_workflow.png)

*Figura 1. CellPick apoya flujos de trabajo de proteómica espacial basados en LMD mediante la selección de células no contiguas y espacialmente representativas, selecciones con restricciones por categoría, anotación de gradientes espaciales definidos por el usuario y exportación para análisis posteriores.*

El algoritmo principal usa una estrategia de tipo k-center que busca maximizar la cobertura espacial dentro de la región seleccionada. Cuando existen etiquetas categóricas, por ejemplo tipos celulares, CellPick también puede seleccionar un número específico de células por categoría manteniendo las mismas restricciones espaciales. Esto resulta útil en experimentos donde es importante balancear el muestreo entre clases biológicas.

CellPick también permite anotar posición espacial. El usuario puede dibujar landmarks, como venas centrales y portales en tejido hepático, y el software calcula un puntaje de gradiente espacial para cada célula según su distancia relativa a esos landmarks. Estas anotaciones se pueden exportar para análisis posteriores, por ejemplo para correlacionar intensidades proteicas con la posición a lo largo de un eje tisular, como en nuestro [estudio de proteómica espacial de célula única sobre zonación hepática humana](https://www.nature.com/articles/s42255-026-01459-2).

![Anotación de gradientes espaciales en CellPick](/en/software/cellpick/cellpick_gradient.webp)

Los resultados se pueden exportar como XML para compatibilidad con microdisección láser, CSV para análisis estadísticos posteriores o SpatialData para integrarse con pipelines de ómicas espaciales. El software está disponible en el [sitio web del proyecto](https://cellpick.app), con [documentación y tutoriales](https://cellpick.readthedocs.io/en/latest/) y código fuente en [GitHub](https://github.com/BorgwardtLab/CellPick).
