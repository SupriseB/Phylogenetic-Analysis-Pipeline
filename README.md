# README: SARS-CoV-2 Phylogenetic Analysis Pipeline

This pipeline enables the detection of SARS-CoV-2 variants, lineage assignment, multiple sequence alignment, and phylogenetic tree construction using Nextclade, MAFFT, and IQ-TREE.

## Prerequisites
Ensure you have the necessary tools installed before running the pipeline.

### Installation
You can install the required tools using the following commands:

```bash
# Install Nextclade (via conda)
conda install -c conda-forge nextclade

# Install MAFFT (for alignment)
sudo apt-get install mafft

# Install IQ-TREE (for phylogenetic tree building)
sudo apt-get install iqtree
```

## Step 1: Detect Variants and Assign Lineages

### A. Detecting Variants with Nextclade

1. Prepare the FASTA file containing your SARS-CoV-2 sequences.
2. Download the required Nextclade dataset:

   ```bash
   nextclade dataset get --name sars-cov-2 --output-dir dataset/
   ```

3. Run Nextclade to assign lineages and detect mutations:

   ```bash
   nextclade run -D dataset/ -O nextclade_result sequence.fasta
   ```

   The output will contain lineage assignments, mutations, and other useful information for each sequence, available in the `nextclade_results` folder.

## Step 2: Create Aligned Sequences and Generate a Consensus Genome

Perform multiple sequence alignment using MAFFT:

```bash
mafft --auto sequence.fasta > aligned_consensus_genomes.fasta
```

## Step 3: Build a Phylogenetic Tree

### A. Identify the Best Model
First, determine the best substitution model using IQ-TREE:

```bash
iqtree -s aligned_consensus_genomes.fasta -m MF -bb 1000 -alrt 1000
```

### B. Construct the Phylogenetic Tree
Once the best model is identified (e.g., `GTR+F+R2`), use the following command to build the tree:

```bash
iqtree -s aligned_consensus_genomes.fasta -m GTR+F+R2 -bb 1000 -alrt 1000 -redo
```

- `-m GTR+F+R2` specifies the substitution model.
- `-bb 1000` runs bootstrapping with 1000 replicates.
- `-alrt 1000` calculates approximate likelihood ratio test (aLRT) values.

This will generate a phylogenetic tree (`aligned_consensus_genomes.fasta.treefile`) for visualization.

## Step 4: Visualize the Phylogenetic Tree
You can visualize the tree using tools such as:

- **iTOL (Interactive Tree of Life):** Web-based tool for tree visualization. Upload your tree file to [iTOL](https://itol.embl.de/).
- **FigTree:** A desktop application for phylogenetic tree visualization.

---

### Citation & Acknowledgments
If you use this pipeline in your research, please cite the respective tools:
- **Nextclade**: https://github.com/nextstrain/nextclade
- **MAFFT**: https://mafft.cbrc.jp/alignment/software/
- **IQ-TREE**: http://www.iqtree.org/

For further inquiries, feel free to reach out!

