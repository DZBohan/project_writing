My project focuses on cross-modal contrastive learning across three modalities: H&E images, Visium spot-level gene expression data, and CODEX subcellular- or pixel-level protein expression data. The primary goal is to train a foundation model that learns representations capturing tissue morphology, highly variable gene expression, spatial microenvironment context, and marker-level protein expression, for downstream functional modules.

Visium data and H&E images are acquired from the same tissue section. CODEX data are acquired from an adjacent section of the same tissue block. Therefore, the CODEX section may exhibit subtle morphological differences and may require additional alignment methods. I thus first focus on modeling H&E images and Visium data and incorporate the CODEX modality after the method is well established.

There are two core questions in encoding H&E images and Visium data. First, unlike previous foundation model training on single-cell transcriptomics, Visium encoding is spot-level rather than gene-level. This means that, for each spot, all highly variable genes must be encoded in a biologically meaningful spot-level representation (spot × feature), rather than each gene in a gene-level representation (gene × feature).

Second, if each spot is encoded only as a spot-level representation (spot × feature), each representation captures only the local gene expression state. It does not capture the spatial microenvironment or spatial context of that location. Therefore, spatial context, including local neighborhoods and global structure, must be injected into the embedding space representations.

For the first core question, I use a lightweight MLP to independently encode each spot’s HVG vector into a spot embedding.  
After Visium normalization and HVG selection (assuming 500 genes), the input for spot *i* is:

$$
x_i = [g_1, g_2, g_3, \dots, g_{500}]
$$

The input for a batch of spots is:

$$
X_b = [x_1, x_2, x_3, \dots, x_b]
$$

If the semantic space dimension is set to 256, the output is a set of 256-dimensional vectors:

$$
\{ z_i \}, \quad z_i \in \mathbb{R}^{256}
$$

where each vector represents the transcriptional state of the corresponding spot.