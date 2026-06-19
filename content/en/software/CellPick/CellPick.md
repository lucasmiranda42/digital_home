---
title: "CellPick"
summary: A graphical tool for strategic cell selection and spatial annotation in spatial proteomics.
series: ["Software"]
weight: 1
aliases: ["/software/CellPick/"]
tags: ["Software", "CellPick", "Deep Visual Proteomics", "Spatial Transcriptomics"]
author: ["Lucas Miranda"]
website: https://cellpick.app
docs: https://cellpick.readthedocs.io/en/latest/
code: https://github.com/BorgwardtLab/CellPick
cover:
  image: /cellpick_cover.png
  alt: "CellPick logo"
---

### Strategic cell selection and positional annotation for spatial proteomics

[Website](https://cellpick.app) · [Documentation](https://cellpick.readthedocs.io/en/latest/) · [Code](https://github.com/BorgwardtLab/CellPick)

[CellPick](https://cellpick.app) is a graphical tool for selecting cells in laser-capture microdissection workflows such as [Deep Visual Proteomics](https://www.nature.com/articles/s41587-022-01302-5). These experiments can measure thousands of proteins per cell, but only for a limited number of cells per tissue section. Picking a representative, non-contiguous subset of cells is therefore a central step: random selection can cluster cells in dense areas, miss sparse regions, and select neighboring cells that may be damaged during microdissection.

CellPick formulates this task as a combinatorial optimization problem and exposes it through an interactive interface that does not require programming. Users can load multichannel microscopy images, cell segmentation masks, and optional cell labels; define active regions; and select cells that are spread across the tissue while avoiding contiguous shapes whenever possible. The software supports standard image inputs such as TIFF, CZI, and Zarr, as well as native integration with SpatialData and the broader scverse ecosystem.

![CellPick workflow overview](../cellpick_workflow.png)

*Figure 1. CellPick supports LMD-based spatial proteomics workflows by selecting non-contiguous, spatially representative cells, supporting category-constrained selections, annotating cells along user-defined spatial gradients, and exporting results for downstream analysis.*

The core selection routine is based on a k-center-style strategy that aims to maximize spatial coverage within the selected region. When categorical labels are available, such as cell types, CellPick can also select a specified number of cells from each category while maintaining the same spatial constraints. This makes it useful for experiments where balanced sampling across biological classes matters.

CellPick also supports positional annotation. Users can draw landmarks, such as central and portal veins in liver tissue, and the software computes a spatial gradient score for each cell based on its relative distance to those landmarks. These annotations can then be exported for downstream analyses, for example to correlate protein intensities with position along a tissue axis, as in our [single-cell spatial proteomics study of human liver zonation](https://www.nature.com/articles/s42255-026-01459-2).

![CellPick spatial gradient annotation](../cellpick_gradient.webp)

Outputs can be exported as XML for laser microdissection compatibility, CSV for downstream statistics, or SpatialData for integration into spatial omics pipelines. The software is available from the [project website](https://cellpick.app), with [documentation and tutorials](https://cellpick.readthedocs.io/en/latest/) and source code on [GitHub](https://github.com/BorgwardtLab/CellPick).
