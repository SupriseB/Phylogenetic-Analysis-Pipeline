# SARS-CoV-2 Analysis Pipeline

## Overview
This pipeline provides a step-by-step guide for analyzing SARS-CoV-2 sequences (FASTA format), including variant detection, lineage assignment, multiple sequence alignment, phylogenetic tree construction, and visualization.

## Requirements
Ensure that the following dependencies are installed:
- [Conda](https://docs.conda.io/en/latest/)
- [MAFFT](https://mafft.cbrc.jp/alignment/software/)
- [IQ-TREE](http://www.iqtree.org/)
- [Nextclade](https://github.com/nextstrain/nextclade)
- [Python](https://www.python.org/) with the `ete3` library for visualization

## Installation
Run the following commands to install the required tools:
```bash
conda install -c conda-forge nextclade
sudo apt-get install mafft
sudo apt-get install iqtree
```

## Usage

### 1. Detect Variants and Assign Lineages
Download the Nextclade dataset and analyze SARS-CoV-2 sequences:
```bash
nextclade dataset get --name sars-cov-2 --output-dir dataset/
nextclade run -D dataset/ -O nextclade_result sequence.fasta
```
Results, including lineage assignments and detected mutations, will be stored in the `nextclade_result` folder.

### 2. Align Sequences and Generate Consensus Genome
Perform multiple sequence alignment using MAFFT:
```bash
mafft --auto sequence.fasta > aligned_consensus_genomes.fasta
```

### 3. Build a Phylogenetic Tree
Construct a phylogenetic tree using IQ-TREE:
```bash
iqtree -s aligned_consensus_genomes.fasta -m GTR+G -bb 1000 -alrt 1000
```
This will generate a tree file named `aligned_consensus_genomes.fasta.treefile`.

### 4. Visualize the Phylogenetic Tree
#### Using iTOL
- Upload the generated tree file to [iTOL](https://itol.embl.de/).

#### Using FigTree
- Open the tree file in [FigTree](http://tree.bio.ed.ac.uk/software/figtree/).

#### Using Python (ete3 library)
Run the following Python script to visualize the tree:
```python
from ete3 import Tree
t = Tree("aligned_consensus_genomes.fasta.treefile", format=1)
t.show()
```

## Notes
- Ensure that the input FASTA file is properly formatted before running the pipeline.
- For large datasets, computation times may vary depending on system resources.

## License
This project is licensed under the MIT License.

