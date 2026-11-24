# HTR2A Missense Variant and Molecular Dynamics Workshop

This repository contains a hands-on workshop on analyzing missense variants (MVs) in the human serotonin receptor **HTR2A (5-HT2A; UniProt P28223)**. The workshop combines variant-effect predictions from GPCRmd, SIFT, PolyPhen-2, and AlphaMissense with molecular dynamics (MD) visualization and distance measurements in VMD.

The practical focuses on variant **C227Y**, which disrupts a conserved cysteine involved in the disulfide bridge between extracellular loop ECL2 and transmembrane helix 3.

## Learning objectives

- Inspect and process missense-variant data downloaded from GPCRmd.
- Compare pathogenicity predictions from SIFT, PolyPhen-2, and AlphaMissense.
- Identify the structural role of a conserved cysteine in HTR2A.
- Visualize wild-type and C227Y molecular dynamics trajectories in VMD.
- Quantify and compare residue or atom distances across simulation frames.
- Discuss how molecular-level effects can inform therapeutic approaches.

## Repository structure

```text
.
├── code/
│   └── analysing_MVs.ipynb       # Main Python/Jupyter analysis notebook
├── data/
│   ├── gpcrmd_mvs.json           # GPCRmd variant table for HTR2A
│   ├── AF-P28223-F1-aa-substitutions.csv
│   ├── structureWT.psf           # Wild-type VMD structure
│   ├── WT_traj.dcd               # Wild-type molecular dynamics trajectory
│   ├── structureC227Y.psf        # C227Y VMD structure
│   └── C227Y_traj.dcd            # C227Y molecular dynamics trajectory
├── results/
│   ├── disulfide_bond_WT.csv     # Precomputed VMD distance measurements for WT
│   └── disulfide_bond_C227Y.csv  # Precomputed VMD distance measurements for C227Y
├── images/                       # Figures used in the notebook
```

## Molecular dynamics data availability

- The **wild-type (WT) molecular dynamics simulation** is publicly available in GPCRmd: [GPCRmd entry 849](https://www.gpcrmd.org/view/849/).
- The **C227Y mutated-system molecular dynamics simulation** is available on demand. Please contact us if you need access to the trajectory and associated structure files.
- The WT and C227Y distance measurements have already been computed and are included in this repository as `results/disulfide_bond_WT.csv` and `results/disulfide_bond_C227Y.csv`. These precomputed data allow the distance analysis to be completed without the C227Y trajectory.

## Requirements

### For the Python analysis

- Python 3
- Jupyter Notebook or JupyterLab
- The following Python packages:
  - `pandas`
  - `seaborn`
  - `matplotlib`

### For molecular visualization

- [VMD](https://www.ks.uiuc.edu/Research/vmd/) (Visual Molecular Dynamics)

## Setup

### Option 1: Create a local environment

From the repository root, create and activate a virtual environment called 'MV_analysis', install the dependencies, and start Jupyter:

```bash
cd /your_working_path/GPCRmd_workshop_variants
conda create -p /your_working_path/GPCRmd_workshop_variants/env/MV_analysis
conda activate /your_working_path/GPCRmd_workshop_variants/env/MV_analysis
conda install jupyter pandas seaborn
jupyter notebook

```

### Option 2: Use an existing Python environment

If you already have a Python environment with the required packages, you can start Jupyter directly:

```bash
jupyter lab
```

Keep the notebook in `code/` when running it because its relative paths use `../data` and `../results`.

## Running the notebook

Run `code/analysing_MVs.ipynb` from top to bottom. The notebook:

1. Loads GPCRmd missense-variant data into a pandas DataFrame.
2. Loads AlphaMissense scores for HTR2A.
3. Compares SIFT, PolyPhen-2, and AlphaMissense classifications.
4. Identifies MVs affecting residue position 227.
5. Loads WT and C227Y distance measurements exported from VMD.
6. Creates a frame-by-frame comparison plot and reports mean distances.

The AlphaMissense CSV uses amino-acid variant identifiers as row indices, allowing the predictions to be joined to the GPCRmd variant table.

## VMD molecular dynamics exercise

The WT trajectory can be obtained from [GPCRmd entry 849](https://www.gpcrmd.org/view/849/). The C227Y trajectory is available only on demand, but it is not required for the workshop because the precomputed WT and C227Y residue-distance data are already available in `results/`.

1. Launch VMD and load `data/structureWT.psf`.
2. Load `data/WT_traj.dcd` on top of the WT structure.
3. Load `data/structureC227Y.psf` and then `data/C227Y_traj.dcd`.
4. In **Graphics > Representations**, create these representations for both systems:
   - `protein` using **NewCartoon**
   - `resname ZOT` using **Licorice**
   - `resid 227` using **Licorice**
   - `resid 148` using **Licorice**
5. Play both trajectories and compare the movement and spacing of residues 148 and 227.

### Measuring the distance

- Use VMD's distance-label tool and select the sulfur atom of C148 and the sulfur atom of C227 in the WT system.
- For C227Y, measure the distance from C148 to the oxygen atom of Y227.
- In **Graphics > Labels**, select **Bonds**, open the relevant bond graph, and export the complete frame series.
- Save the WT measurements as `results/disulfide_bond_WT.csv`.
- Save the C227Y measurements as `results/disulfide_bond_C227Y.csv`.

Both exported files contain frame numbers and distances in angstroms. The notebook merges and plots these measurements.

## Interpretation

The conserved C148-C227 disulfide bridge anchors extracellular loop ECL2 to transmembrane helix 3. The C227Y substitution removes the cysteine sulfur involved in this bridge and can alter loop stability and receptor conformation.

The workshop prompts participants to compare the predictions with the measured simulation behavior and consider whether stabilizing ligands, such as pharmacological chaperones, could restore local structure or receptor function.

## Data sources

- **GPCRmd:** genetic-variant and molecular-dynamics resources for GPCRs.
- **AlphaFold Protein Structure Database:** AlphaMissense predictions for HTR2A (UniProt P28223).
- **SIFT and PolyPhen-2:** missense-variant impact predictions displayed in GPCRmd.

## References

- GPCRmd: https://www.gpcrmd.org/
- AlphaFold: https://alphafold.ebi.ac.uk/
- VMD: https://www.ks.uiuc.edu/Research/vmd/
- C227Y functional impact study: https://www.sciencedirect.com/science/article/abs/pii/S0014299917305940
