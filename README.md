# Supplemental data for Berg et al. (2025)

This repository contains derived data products for [Berg et al. (2025)](https://www.biorxiv.org/content/10.1101/2025.10.09.680999v1).

For primary connectome data artefacts (segmentation, synapse detection, etc.), please see https://male-cns.janelia.org/.


## Sensorimotor Information Flow

#### [`maxflow_sensorimotor_edges.20250911T1351.parquet.tar.gz`](/supplemental_data/maxflow_sensorimotor_edges.20250911T1351.parquet.tar.gz)
Maxflow values across all neuron-neuron edges for all combinations of sensory modality and motor domains.

<details>
<summary>Example with code</summary>

Unzip the archive and read one of the sensory->motor flows:

```python
>>> import pandas as pd
>>> df = pd.read_parquet("maxflow_sensorimotor_edges.20250911T1351.parquet/sensory=chemosensory/motor=am/184b5da9b9f74cb78ea698fcbb4b43cf-0.parquet")
>>> df.head()
   weight    pre   post
0  0.0001  10001  10654
1  0.0001  10001  10924
2  0.0023  10002  10863
3  0.0015  10002  10927
4  0.0006  10005  11051
```
</details>

#### [`dnan_cluster_function_20250919.csv`](/supplemental_data/dnan_cluster_function_20250919.csv)
Cluster assignments by type for all descending and ascending neurons.

<details>
<summary>Example with code</summary>

```python
>>> import pandas as pd
>>> df = pd.read_csv("dnan_cluster_function_20250919.csv")
>>> df.head()
   bodyId  ...                            reference
0   10001  ...  Lima 2005, Kennedy and Broadie 2018
1   10010  ...  Lima 2005, Kennedy and Broadie 2018
2   10026  ...                                  NaN
3   10030  ...                                  NaN
4   10033  ...                                  NaN
```
</details>


## Graph traversal layers

#### [`sensory_network_traversal_model_layers.feather`](/supplemental_data/sensory_network_traversal_model_layers.feather)
Mean, median, minimum and maximum layers after 10,000 iterations of graph traversal; `layer_min` is the earliest traversal across all runs, `layer_max` is the latest; `layer_mean` is the average across all runs, `layer_median` is the median. Seeds were all sensory neurons (as per their `superclass`).

<details>
<summary>Example with code</summary>

```python
>>> import pandas as pd
>>> df = pd.read_feather("sensory_network_traversal_model_layers.feather")
>>> df.head()
    node  layer_min  layer_max  layer_mean  layer_median
0  10001          2          6    4.081481           4.0
1  10002          3          6    4.251852           4.0
2  10003          3          7    5.066667           5.0
3  10005          3          7    5.244444           5.0
4  10006          3          7    5.111111           5.0
```
</details>

## Optic lobe column assignment

#### [`optic-column-type-assignments.xlsx`](/supplemental_data/optic-column-type-assignments.xlsx)
For each visual column of the left and right optic lobe: IDs of the corresponding L1, R7 and R8 neurons, and column type (pale, yellow, edge, dorsal rim area).  Additional columns indicate the presence of identified vertical aMe12 and/or Tm5a branches which serve as anatomical markers to identify likely pale/yellow columns (see Methods).

<details>
<summary>Example with code</summary>

```python
>>> import pandas as pd
>>> df = pd.read_excel("optic-column-type-assignments.xlsx")
>>> df.head()
           column     L1      R7  ... aMe12_branch  Tm5a_branch Notes
0  ME_R_col_10_06  39722  230050  ...            1            0   NaN
1  ME_R_col_18_12  36897     -99  ...            1            1   NaN
2  ME_R_col_20_09  26900     -99  ...            1            1   NaN
3  ME_R_col_21_08  26152     -99  ...            1            1   NaN
4  ME_R_col_21_12  30138     -99  ...            1            1   NaN
```
</details>


## Male-female aligned edges

#### [`mcns_fw_edge_comp.feather`](/supplemental_data/mcns_fw_edge_comp.feather)
Edge weights between cross-matched male CNS and FlyWire cell types with arbors in the central brain. Connections made within the VNC portion of the male CNS have been excluded:
- `pre/post`: pre- and postsynaptic cross-matched cell type, respectively
- `weight_m/_f`: total synapses between the pre- and postsynaptic type in male and female, respectively
- `t`: t-statistics as described in Methods
- `p_corr`: p-values after false-discovery rate correction
- `verdict_corr`: edge dimorphism after fixing false-positives (connections between isomorphic types; “dimorphic” -> “isomorphic”) and false-negatives (connections involving sex-specific types; “isomorphic” -> “dimorphic”)

<details>
<summary>Example with code</summary>

```python
>>> import pandas as pd
>>> df = pd.read_feather("mcns_fw_edge_comp.feather")
>>> df.head()
                pre              post  ...    p_corr  verdict_corr
0  (PLP191,PLP192)b  (PLP191,PLP192)b  ...  0.177720    isomorphic
1  (PLP191,PLP192)b        5-HTPMPV03  ...  0.746035         noise
2  (PLP191,PLP192)b     AOTU044,LTe43  ...  0.500310         noise
3  (PLP191,PLP192)b           AOTU065  ...  0.746035         noise
4  (PLP191,PLP192)b          AVL006_a  ...  0.500310         noise
```
</details>


## Male-female mappings

#### [`mcns_fw_edge_comp_mappings.json`](/supplemental_data/mcns_fw_edge_comp_mappings.json)
The neuron-to-label assignment for male CNS and FlyWire neurons used to generate the cross-matched edges. Each unique label corresponds to a cross-matched group of neurons. In cases where the neurons have the exact same cell type name, the label correspond to that type. In cases where a group of neurons has different cell type names in male CNS and FlyWire, the label is chosen arbitrarily.

<details>
<summary>Example with code</summary>

```python
>>> import json
>>> with open("mcns_fw_edge_comp_mappings.json") as f:
...     data = json.load(f)
>>> list(data.items())[:2]
[('79556', 'ANG_GNG_94,AN_GNG_117'), ('85165', 'ANG_GNG_94,AN_GNG_117')]
```
</details>

## Synapse precision and recall

### [`male-cns-v0.9-synapse-tbar-precision-recall-by-roi.csv`](/supplemental_data/male-cns-v0.9-synapse-tbar-precision-recall-by-roi.csv)

The precision and recall of pre-synapse (t-bar) predictions based on validation data collected in 81 neuropil compartments.  This is the source data for the upper plot in Fig 1i of Berg _et al._ (2025).

### [`male-cns-v0.9-synapse-connection-precision-recall-by-roi.csv`](/supplemental_data/male-cns-v0.9-synapse-tbar-precision-recall-by-roi.csv)

The precision and recall of predicted pre-to-post synaptic connections based on validation data collected in 81 neuropil compartments.  This is the source data for the lower plot in Fig 1i of Berg _et al._ (2025).


## Citation

```bibtex
@article{Berg2025,
  author = {Berg,  Stuart and Beckett,  Isabella R and Costa,  Marta and Schlegel,  Philipp and Januszewski,  Michal and Marin,  Elizabeth C and Nern,  Aljoscha and Preibisch,  Stephan and Qiu,  Wei and Takemura,  Shin-ya and Fragniere,  Alexandra M C and Champion,  Andrew S and Adjavon,  Diane-Yayra and Cook,  Michael and Gkantia,  Marina and Hayworth,  Kenneth J and Huang,  Gary B and Kampf,  Florian and Katz,  William T and Lu,  Zhiyuan and Ordish,  Christopher and Paterson,  Tyler and Stuerner,  Tomke and Trautman,  Eric T and Whittle,  Catherine R and Burnett,  Laura E and Hoeller,  Judith and Li,  Feng and Loesche,  Frank and Morris,  Billy J and Pietzsch,  Tobias and Pleijzier,  Markus W and Silva,  Valeria and Yin,  Yijie and Ali,  Iris and Badalamente,  Griffin and Bates,  Alexander Shakeel and Bogovic,  John and Brooks,  Paul and Cachero,  Sebastian and Canino,  Brandon S and Chaisrisawatsuk,  Bhumpanya and Clements,  Jody and Crowe,  Arthur and de Haan Vicente,  Ines and Dempsey,  Georgia and Dona,  Erika and dos Santos,  Marcia and Dreher,  Marisa and Dunne,  Christopher R and Eichler,  Katharina and Finley-May,  Samantha and Flynn,  Miriam A and Hameed,  Imran and Hopkins,  Gary Patrick and Hubbard,  Philip M and Kiassat,  Ladann and Kovalyak,  Julie and Lauchie,  Shirley A and Leonard,  Meghan and Lohff,  Alanna and Longden,  Kit D and Maldonado,  Charli A and Mitletton,  Myrto and Moitra,  Ilina and Moon,  Sung Soo and Mooney,  Caroline and Munnelly,  Eva J and Okeoma,  Nneoma and Olbris,  Donald J and Pai,  Anika and Patel,  Birava and Phillips,  Emily M and Plaza,  Stephen M and Richards,  Alana and Rivas Salinas,  Jennifer and Roberts,  Ruairi J V and Rogers,  Edward M and Scott,  Ashley L and Scuderi,  Louis A and Seenivasan,  Pavithraa and Serratosa Capdevila,  Laia and Smith,  Claire and Svirskas,  Rob and Takemura,  Satoko and Tastekin,  Ibrahim and Thomson,  Alexander and Umayam,  Lowell and Walsh,  John J and Whittome,  Holly and Xu,  C Shan and Yakal,  Emily A and Yang,  Tansy and Zhao,  Arthur and George,  Reed and Jain,  Viren and Jayaraman,  Vivek and Korff,  Wyatt and Meissner,  Geoffrey W and Romani,  Sandro and Funke,  Jan and Knecht,  Christopher and Saalfeld,  Stephan and Scheffer,  Louis K and Waddell,  Scott and Card,  Gwyneth M and Ribeiro,  Carlos and Reiser,  Michael B and Hess,  Harald F and Rubin,  Gerald M and Jefferis,  Gregory S X E},
  title = {Sexual dimorphism in the complete connectome of the Drosophila male central nervous system},
  DOI = {10.1101/2025.10.09.680999},
  publisher = {Cold Spring Harbor Laboratory},
  year = {2025},
}
```
