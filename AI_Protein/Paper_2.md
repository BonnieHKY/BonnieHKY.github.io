# RNA sequence design and protein–DNA specificity prediction with NA-MPNN

This paper introduces **Nucleic Acid MPNN (NA-MPNN)**, a unified message-passing neural network (MPNN) that addresses two critical nucleic acid inverse-folding tasks—*RNA sequence design* and *fixed-dock protein–DNA binding specificity prediction*—by treating proteins, DNA, and RNA as a single biopolymer graph. Unlike existing task-specific tools, NA-MPNN leverages diverse training data across both tasks to achieve broader applicability and state-of-the-art performance.

## <font color=teal>1. Background and Motivation</font>

Nucleic acid inverse folding involves predicting the most likely nucleic acid sequence for a fixed 3D structure (e.g., standalone RNA or protein–DNA complexes). This is essential for developing tools like aptamers, CRISPR scaffolds, and transcription factors, but prior methods had two key limitations:

- They were specialized (e.g., gRNAde/RhoDesign for RNA design, DeepPBS for protein–DNA specificity) and could not handle both tasks.
- Training was hindered by limited high-resolution RNA structures (only 8,961 RNA-containing entries in PDB vs. 235,538 protein entries as of Jan 2025) and fragmented datasets.

Inspired by the success of ProteinMPNN (for protein sequence design) and LigandMPNN (for ligand-aware design), the authors developed NA-MPNN to unify these tasks and overcome data constraints.

## <font color=teal>2. NA-MPNN Architecture</font>

NA-MPNN extends ProteinMPNN’s graph-based framework to include nucleic acids, with key innovations:

![P2_F1.png](D:\BonnieHKY.github.io\AI_Protein\Figures\P2_F1.png)

### <font color=teal>2.1 Core Graph Representation</font>

- **Nodes**: Represent protein residues, DNA bases, or RNA bases. Each node receives a *one-hot polymer-type embedding* (protein/DNA/RNA) to accelerate learning (vs. zero initial features in ProteinMPNN).

- **Edges**: Connect each node to its 32 nearest neighbors (vs. 48 in ProteinMPNN), with edge features derived from radial-basis-function embeddings of pairwise backbone atom distances.
  
  - for proteins the N, Cα, C, O, and virtual Cβ atoms
  - for nucleic acids, the phosphate atoms (P, OP1, OP2), sugar atoms (C1′–5′, O3′–5′, O2′ for RNA), and virtual side-chain N atom
  
  ![P2_F2.png](D:\BonnieHKY.github.io\AI_Protein\Figures\P2_F2.png)

- **Token Alphabet**: Expanded to unify DNA and RNA: shared tokens for canonical bases (DA/A, DC/C, DG/G, DT/U) plus an unknown token (DX/RX), enabling cross-learning between DNA and RNA contexts.
  
  > **Token** refers to the **basic discrete unit of data** that an AI model processes to learn patterns, make predictions, or generate outputs. It acts as a "bridge" between raw, continuous/complex data (e.g., text, images, biological sequences) and the model’s numerical computation framework—converting unstructured data into a format the model can interpret.
  > 
  > - For **proteins**: 21 tokens (one for each canonical amino acid + 1 for unknown residues).
  > - For **nucleic acids (DNA/RNA)**: 5 tokens (DA/A, DC/C, DG/G, DT/U, DX/RX—shared between DNA and RNA to enable cross-learning).

- **Label smoothing** across residue tokens: a small fraction of probability is taken from the "correct" biopolymer token and split among other *same-class tokens*. This reduces model overconfidence while preventing nonsensical "leakage" between protein and nucleic acid predictions
  
  > Label smoothing is a standard technique in supervised learning to **reduce model overconfidence**—a common problem where a model becomes too certain of its predictions (e.g., assigning a probability of 1.0 to the "correct" label and 0.0 to all others) and fails to generalize to new data.
  > 
  > In traditional classification (without smoothing), the "target label" for a correct class is a **one-hot vector**:
  > 
  > - For example, if the "true" residue token for a nucleic acid position is *DA/A* (adenine), the one-hot target would be `[1.0, 0.0, 0.0, 0.0, 0.0]` (where the indices correspond to NA-MPNN’s 5 nucleic acid tokens: DA/A, DC/C, DG/G, DT/U, DX/RX).
  > 
  > Label smoothing adjusts this by **redistributing a small fraction of the probability mass** (typically 5–10%) from the "correct" token to the "incorrect" tokens. For example, with 10% smoothing (ε=0.1):
  > 
  > - The correct token (*DA/A*) gets a probability of `1.0 - ε = 0.9`.
  > - The remaining `ε = 0.1` is split evenly among the 4 incorrect nucleic acid tokens: each gets `0.1 / 4 = 0.025`.
  > - The smoothed target vector becomes `[0.9, 0.025, 0.025, 0.025, 0.025]`.

### <font color=teal>2.2 Task-Specific Models</font>

NA-MPNN uses two variants (both optimized with cross-entropy loss but differing in supervision):

1. **Design Model**: Targets crystallographic sequences (one-hot vectors) to generate a single cohesive sequence matching the input backbone (e.g., standalone RNA or RNA in a protein context).
2. **Specificity Model**: Targets experimental *position probability matrices (PPMs)* (from assays like SELEX/PBM) to predict base frequency distributions at protein–DNA interfaces, rather than a single sequence.

## <font color=teal>3. Data and Training</font>

### <font color=teal>3.1 Datasets</font>

- **Design Task**: PDB entries with nucleic acids (resolution ≤3.5 Å, polymer length ≤6,000 residues), clustered at 80% nucleic acid sequence identity to ensure test-set independence. The final test set included 39 DNA-only, 88 RNA-only, 237 protein–DNA, and 139 protein–RNA complexes, plus subsets for RNA monomers (63 examples) and pseudoknots (10 examples).
- **Specificity Task**: Combined PDB crystal structures (with PPMs from JASPAR/HOCOMOCO) and 29,460 RFNA/RFAA-predicted protein–DNA complexes (from CIS-BP/TRANSFAC), filtered for structural confidence (interface pAE ≤6, complex pLDDT ≥0.85).

### <font color=teal>3.2 Data Augmentation (Specificity Model Only)</font>

To focus on protein-induced sequence preferences (not backbone artifacts):

- For nucleic acid-only complexes, assign uniform PPMs (0.25 probability per base).
- Stochastically drop protein chains (50% chance) and replace PPMs with uniform values.
- Overwrite non-interface nucleic acid positions (＞5 Å from protein side chains) with uniform PPMs if no experimental data exists.

## <font color=teal>4. Key Performance Results</font>

### <font color=teal>4.1 RNA Sequence Design</font>

NA-MPNN demonstrated robust sequence recovery and **superior structural fidelity** compared to competitors (gRNAde, RhoDesign):

- **Sequence Recovery**: Median recovery of 57.4% (DNA-only), 60.5% (RNA-only), 58.6% (DNA + protein), and 55.4% (RNA + protein). It outperformed gRNAde (51.7% vs. 58.0% on RNA monomers) but trailed RhoDesign (66.7% vs. 58.0%) in raw recovery.
- **Structural Fidelity**: On pseudoknots, NA-MPNN designs had:
  - Higher predicted OpenKnot scores (83.2 vs. 81.7 for gRNAde, 72.9 for RhoDesign; measures secondary structure match).
  - Lower C1′-RMSD (9.0 Å vs. 11.6 Å/22.6 Å; closer to native tertiary structure).
  - Higher AlphaFold 3 pLDDT (56.2 vs. 51.6/44.2; higher structural confidence).
- **Experimental Validation**: In the OpenKnot pseudoknot design challenge (Round 6), NA-MPNN achieved a median experimental OpenKnot score of 89.9—surpassing gRNAde (80.7), wild-type sequences (87.0), and Eterna user designs (89.4).

### <font color=teal>4.2 Protein–DNA Specificity Prediction</font>

On a test set of 228 RFNA/RFAA complexes with experimental PPMs, NA-MPNN outperformed DeepPBS (the prior state-of-the-art):

- **Mean Absolute Error (MAE)**: Lower median MAE (0.53 vs. 0.86 overall; 0.52 vs. 0.87 for CIS-BP, 0.53 vs. 0.85 for TRANSFAC).
- **Cross-Entropy**: Lower median cross-entropy (1.00 vs. 1.44 overall; 0.97 vs. 1.45 for CIS-BP, 1.05 vs. 1.43 for TRANSFAC).

Notably, NA-MPNN required only backbone coordinates (no protein side-chain data), avoiding information leakage and making it a fast filter for DNA-binding protein design.

## <font color=teal>5. Discussion and Applications</font>

NA-MPNN’s unified framework enables:

- **De Novo RNA Design**: Combined with RFDpoly (a diffusion-based backbone generator), it creates novel RNAs and protein–DNA complexes verified by electron microscopy.
- **Transcription Factor Engineering**: Rapid screening of specificity profiles without exhaustive docking or side-chain sampling.
- **Future Extensions**: Potential for RNA-binding protein specificity prediction, single-stranded DNA design, and protein–nucleic acid construct design (not explored here).

## <font color=teal>6. Availability</font>

Code, model weights, and datasets (excluding licensed TRANSFAC data) are available at https://github.com/baker-laboratory/NA-MPNN.

In summary, NA-MPNN sets a new standard for nucleic acid inverse folding, unifying two critical tasks and enabling advances in synthetic biology and genome engineering.
