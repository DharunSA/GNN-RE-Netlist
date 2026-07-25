# GNN-RE: Graph Neural Networks for Reverse Engineering of Gate-Level Netlists 🧬

![Python](https://img.shields.io/badge/Python-3.6.8-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-1.12.0-orange)
![PyTorch](https://img.shields.io/badge/PyTorch-1.1.0-red)
![License](https://img.shields.io/badge/License-MIT-green)

> **GNN-RE** is a generic, graph neural network (GNN)-based platform for functional reverse engineering of circuits.

This repository provides a code-level implementation and comprehensive methodology to extract and classify interconnected sub-circuits from unstructured gate-level netlists using GraphSAINT.

## 📖 Overview
Reverse engineering of gate-level netlists is a challenging problem in hardware security. GNN-RE:
- Represents and analyzes flattened/unstructured gate-level netlists as directed graphs.
- Automatically identifies the boundaries between the modules or sub-circuits implemented in such netlists.
- Classifies the sub-circuits based on their functionalities (Adders, Multipliers, Subtractors, Comparators, Control Logic).

### Project Materials
- **[Final Paper Details](./VLSI%20Final%20Paper.docx)**
- **Methodology Walkthrough Video**: *(Link to be added via Google Drive)*
- **Model Training Video**: *(Link to be added via Google Drive)*

## 📂 Repository Structure
```text
📦 VLSI Project
 ┣ 📂 Code/
 ┃ ┗ 📂 GNN-RE/
 ┃   ┣ 📂 Netlist_to_graph/    # Perl and Python parsing scripts
 ┃   ┣ 📂 RTL_Dataset/         # 37 Original Verilog Designs (Interconnected-Modules)
 ┃   ┣ 📜 TCAD.yml             # GraphSAINT hyperparameters
 ┃   ┗ 📜 README.md            # Parsing instructions & dataset details
 ┣ 📂 results/                 # Extracted performance metrics, loss curves, & graphs
 ┣ 📜 GNN_RE CODE.ipynb        # Complete Model Implementation and Experimentation Notebook
 ┣ 📜 VLSI Final Paper.docx    # Full technical documentation
 ┣ 📜 ML in VLSI.mp4           # Video presentation and walkthrough
 ┣ 📜 Model-Training video.mp4 # Video demonstration of model training phase
 ┣ 📜 .gitignore               # Ignored files configuration
 ┗ 📜 CONTRIBUTING.md          # Contribution guidelines
```

## ⚙️ Installation & Environment Setup

It is highly recommended to use Anaconda or Miniconda for setting up the environment.

```bash
conda create --name gnn_re_env python=3.6.8 tensorflow=1.12.0
conda activate gnn_re_env
conda install -c anaconda numpy=1.14.3 scipy=1.1.0 scikit-learn=0.19.1 pyyaml=3.12 cython=0.29.2
conda install -c conda-forge openmp=4.0
```

**GraphSAINT Requirement**
The node classification leverages GraphSAINT. You must compile the GraphSAINT extensions:
```bash
git clone https://github.com/GraphSAINT/GraphSAINT.git
cd GraphSAINT
python graphsaint/setup.py build_ext --inplace
```

## 🚀 How to Run

### 1. Netlist to Graph Conversion
We use custom Perl and Python parsers to map Synopsys synthesized gate-level Verilog to sparse graph structures:
```bash
cd Code/GNN-RE/Netlist_to_graph/Graphs_datasets/Interconnected-Modules/
perl ../../Parsers/netlist_to_graph_re.pl -i ../../Circuits_datasets/Interconnected-Modules > log.txt
python ../../Parsers/graph_parser.py
```

### 2. Model Training & Inference (Node Classification)
Train the model using the parameters specified in `TCAD.yml`:
```bash
python -m graphsaint.tensorflow_version.train \
       --data_prefix ../Netlist_to_graph/Graphs_datasets/Interconnected-Modules \
       --train_config ../TCAD.yml \
       --gpu -1 > log_training.txt
```
*Alternatively, you can explore the entire pipeline step-by-step using the provided `GNN_RE CODE.ipynb` notebook.*

## 📊 Results & Performance

The evaluation demonstrates the capability of Graph Neural Networks to handle complex, non-euclidean circuit structures. Significant metrics and graphs have been extracted to the [`results/`](./results) directory:

- **Accuracy & Validation Performance**: GNN-RE achieves superior node classification compared to standard ML heuristics. See `results/B_Accuracy_Performance.png`.
- **Loss Convergence**: The model converges smoothly across epochs. See `results/A_Loss_Convergence.png`.
- **GNN Depth Analysis**: Varying the depth of graph sampling layers (e.g., Depth-1, Depth-2) significantly impacts contextual feature propagation. See `results/C_GNN_Depth_Analysis.png`, `results/Depth 1 Final.png`, and `results/Depth-2 final.png`.
- **Pipeline Methodology**: Reference `results/Algorithm flow.png` and `results/overview_diagram.png` (in `Code/GNN-RE/`) for a graphical intuition of the extraction-to-classification flow.

## 📝 Citation
If this repository supports your research, please cite the foundational TCAD paper:
```bibtex
@ARTICLE{9530566,
  author={Alrahis, Lilas and Sengupta, Abhrajit and Knechtel, Johann and Patnaik, Satwik and Saleh, Hani and Mohammad, Baker and Al-Qutayri, Mahmoud and Sinanoglu, Ozgur},
  journal={IEEE Transactions on Computer-Aided Design of Integrated Circuits and Systems}, 
  title={{GNN-RE}: Graph Neural Networks for Reverse Engineering of Gate-Level Netlists}, 
  year={2021},
  doi={10.1109/TCAD.2021.3110807}
}
```

## 🤝 Acknowledgments
Special thanks to the authors of GNN-RE and GraphSAINT (Hanqing Zeng) for open-sourcing foundational graph methodology code.
