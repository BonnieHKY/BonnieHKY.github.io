# Robust deep learning–based protein sequence design using ProteinMPNN

### <font color=teal>1. Computational experimental design (Model development & In silico testing)</font>

#### <font color=teal>1.1 Training Data & Model Optimization</font>

- **Training Dataset**: 19,700 high-resolution single-chain structures from the Protein Data Bank (PDB, X-ray/cryo-EM, resolution <3.5 Å, <10,000 residues), split into train/validation/test sets (80/10/10) using the CATH protein classification system.
  
  > The **CATH system** (short for *Class, Architecture, Topology, Homology*) is a widely used, hierarchical database and classification framework for organizing protein structures based on their structural and evolutionary relationships. It plays a critical role in structural biology by systematically categorizing proteins from the Protein Data Bank (PDB)—the global repository of experimentally determined protein structures—enabling researchers to study protein evolution, function, and design.

- **Model Optimization Experiments** (summarized in Table 1):
  
  1. **Input Feature Enhancement**: Added interatomic distances (N, Cα, C, O, and virtual Cβ) to replace/reinforce dihedral angles, increasing sequence recovery from 41.2% (baseline) to 49.0%.
  
  2. **Encoder Edge Updates**: Modified the message-passing neural network (MPNN) to update both node and edge features, improving recovery to 43.1%.
     
     > <border-left-color: red>11</border-left-color>
     
     > ##### 1. What is MPNN?
     > 
     > A **Message Passing Neural Network (MPNN)** is a graph neural network (GNN) tailored for graph-structured data. In protein sequence design, it models proteins as graphs—with nodes representing amino acid residues/atoms and edges encoding spatial/chemical relationships (e.g., distances, interactions). Its core is "message passing," where nodes iteratively exchange information to update states, capturing both local and global structural dependencies.
     > 
     > ##### 2. How MPNNs Outperform Other Methods
     > 
     > - **Explicit structural graphing**: Directly encodes atomic interactions/geometric features (e.g., Cα–Cα distances), achieving higher sequence recovery rates.
     > - **Long-range dependency capture**: Uses message passing to model both local (e.g., secondary structure) and global (e.g., domain interactions) relationships, avoiding reliance on positional encodings (critical for multi-chain proteins).
     > - **Non-protein component integration**: Builds protein-ligand/metal interaction graphs (e.g., LigandMPNN), boosting sequence recovery in binding regions and optimizing binding interfaces.
     > - **Scalability & efficiency**: Generates sequences far faster by skipping intensive steps like Monte Carlo sampling, and generalizes well via large PDB dataset training.
  
  3. **Combined Optimization**: Merging feature enhancement and edge updates raised recovery to 50.5%.
  
  4. **Order-Agnostic Decoding**: Replaced fixed N→C terminal decoding with random permutations, enabling flexible design (e.g., fixed functional motifs) and boosting recovery to 50.8%.

- **Robustness Training**: Trained models with Gaussian noise (SD=0.02 Å) added to backbones to reduce overfitting to crystallographic details. This improved sequence recovery for AlphaFold-generated backbones (average IDDT >80) while slightly lowering recovery for unperturbed PDB structures.

#### 1.2 In Silico Performance Evaluation

- **Benchmark vs. Rosetta**: For 402 monomer backbones, ProteinMPNN achieved 52.4% native sequence recovery (vs. 32.9% for Rosetta) and was 200x faster (1.2 vs. 258.8 seconds per 100 residues on a single CPU).
- **Generalization Testing**: Evaluated on 690 monomers, 732 homomers, and 98 heteromers:
  - Median sequence recovery: 52% (monomers), 55% (homomers), 51% (heteromers); interface recovery: 53% (homomers), 51% (heteromers).
  - Recovery correlated with residue burial (90–95% in deep cores, 35% on surfaces), reflecting reliance on local geometric context.
- **Sequence Diversity & Quality Control**: Adjusted sampling temperature to balance diversity and recovery (higher temperature increased diversity with minimal recovery loss) and used averaged log probability of sequences to rank design quality.
