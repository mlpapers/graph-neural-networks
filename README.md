---
title: "Graph Neural Networks"
aliases: ["GNN", "Graph networks"]
tags:
  - deep-learning
  - graph
  - neural-networks
---

# Graph Neural Networks

## Foundations

- **The Graph Neural Network Model** - Learns node states from graph structure and attributes through recurrent updates; the original formulation uses a contraction condition to obtain a unique fixed point. [Scarselli et al. 2009](<https://usiena-air.unisi.it/handle/11365/26421> "The Graph Neural Network Model; IEEE TNN").
- **Message Passing Neural Networks** - Separates local message computation and node updates from a graph-level readout. Unifies several molecular prediction architectures within one framework. [Gilmer et al. 2017](<https://proceedings.mlr.press/v70/gilmer17a.html> "Neural Message Passing for Quantum Chemistry; ICML").
- **Graph networks** - Unifies computations on nodes, edges, and global graph attributes through shared update functions and permutation-invariant aggregation; connects graph structure to relational inductive biases. [Battaglia et al. 2018](<https://arxiv.org/abs/1806.01261> "Relational inductive biases, deep learning, and graph networks; arXiv").

## Random-walk embedding baselines

- **DeepWalk** - Treats short random walks as sequences and learns node vectors from their local contexts. Provides a graph-structure embedding baseline without a learned neighborhood feature aggregator. [Perozzi et al. 2014](<https://arxiv.org/abs/1403.6652> "DeepWalk: Online Learning of Social Representations; KDD").
- **node2vec** - Learns node vectors from biased random walks, with parameters controlling which neighborhoods are explored. Changes the context distribution used to train embeddings. [Grover and Leskovec 2016](<https://arxiv.org/abs/1607.00653> "node2vec: Scalable Feature Learning for Networks; KDD").

## Neighborhood aggregation

- **Spectral networks** - Extends convolution to graphs through filters in the graph Laplacian's eigenbasis; also studies locally connected architectures based on hierarchical clustering. [Bruna et al. 2013](<https://arxiv.org/abs/1312.6203> "Spectral Networks and Locally Connected Networks on Graphs; arXiv").
- **ChebNet** - Uses Chebyshev polynomials of the graph Laplacian to construct localized spectral filters, avoiding explicit eigendecomposition during filtering. [Defferrard et al. 2016](<https://arxiv.org/abs/1606.09375> "Convolutional Neural Networks on Graphs with Fast Localized Spectral Filtering; NeurIPS").
- **GCN** - Alternates learned feature transformations with degree-normalized neighborhood aggregation, including self-loops. Its semi-supervised setup uses graph structure and features beyond the labeled nodes. [Kipf and Welling 2017](<https://arxiv.org/abs/1609.02907> "Semi-Supervised Classification with Graph Convolutional Networks; ICLR").
- **GraphSAGE** - Samples neighborhoods and learns feature-aggregation functions that can embed unseen nodes or graphs sharing the input feature schema, instead of storing a separate learned vector for each node. [Hamilton et al. 2017](<https://arxiv.org/abs/1706.02216> "Inductive Representation Learning on Large Graphs; NeurIPS").
- **GAT** - Learns attention weights over graph neighborhoods to combine node features; multi-head attention provides several learned aggregations. The original attention is restricted to graph neighbors. [Veličković et al. 2018](<https://arxiv.org/abs/1710.10903> "Graph Attention Networks; ICLR").
    - **GATv2** - Reorders attention operations so neighbor rankings can depend on the query node, addressing the original GAT's static attention ranking. [Brody et al. 2022](<https://arxiv.org/abs/2105.14491> "How Attentive are Graph Attention Networks?; ICLR").
- **Principal Neighbourhood Aggregation** - Combines multiple neighborhood statistics with degree-dependent scaling. Addresses information lost when a single aggregator summarizes continuous node features. [Corso et al. 2020](<https://arxiv.org/abs/2004.05718> "Principal Neighbourhood Aggregation for Graph Nets; NeurIPS").

## Diffusion and propagation

- **PPNP / APPNP** - Separates neural prediction from personalized PageRank propagation. APPNP approximates propagation iteratively, extending information flow without stacking more learned feature transformations. [Gasteiger et al. 2019](<https://arxiv.org/abs/1810.05997> "Predict then Propagate: Graph Neural Networks meet Personalized PageRank; ICLR").
- **Graph Diffusion Convolution** - Constructs a new graph by sparsifying generalized diffusion, including personalized PageRank or heat kernels. Subsequent message passing uses these diffusion-derived connections. [Gasteiger et al. 2019](<https://arxiv.org/abs/1911.05485> "Diffusion Improves Graph Learning; NeurIPS").

## Expressivity and higher-order structure

- **GIN and the 1-WL limit** - With enough layers and injective aggregation, updates and readout, can match 1-WL. The sum construction assumes countable features and bounded multisets; training need not find these functions, and 1-WL is not a complete isomorphism test. [Xu et al. 2019](<https://arxiv.org/abs/1810.00826> "How Powerful are Graph Neural Networks?; ICLR").
- **Higher-order GNNs** - Relates GNNs to Weisfeiler-Leman refinement and extends aggregation to sets of nodes. Higher-order representations distinguish structures that ordinary node-level message passing can miss. [Morris et al. 2019](<https://arxiv.org/abs/1810.02244> "Weisfeiler and Leman Go Neural: Higher-order Graph Neural Networks; AAAI").
- **Provably Powerful Graph Networks** - Uses matrix-valued representations, multilayer perceptrons, and matrix multiplication to exceed standard message-passing expressivity under the paper's graph-isomorphism analysis. [Maron et al. 2019](<https://arxiv.org/abs/1905.11136> "Provably Powerful Graph Networks; NeurIPS").
- **Equivariant Subgraph Aggregation Networks** - Represents a graph as a set of selected subgraphs and aggregates them equivariantly. Subgraph selection affects expressivity; sampling reduces the added computation. [Bevilacqua et al. 2022](<https://arxiv.org/abs/2110.02910> "Equivariant Subgraph Aggregation Networks; ICLR").
- **Graph Substructure Networks** - Augments message passing with structural features from subgraph-isomorphism counts. Expressivity depends on the chosen substructures; computing those counts adds preprocessing work. [Bouritsas et al. 2023](<https://arxiv.org/abs/2006.09252> "Improving Graph Neural Network Expressivity via Subgraph Isomorphism Counting; TPAMI").

## Depth and normalization

- **Different limits** - 1-WL concerns structural discrimination; oversquashing concerns information transport; oversmoothing concerns representation collapse. Stronger aggregation, rewiring and normalization address different limits. [Xu et al. 2019](<https://arxiv.org/abs/1810.00826> "How Powerful are Graph Neural Networks?; ICLR"), [Alon and Yahav 2021](<https://arxiv.org/abs/2006.05205> "On the Bottleneck of Graph Neural Networks and its Practical Implications; ICLR"), [Li et al. 2018](<https://arxiv.org/abs/1801.07606> "Deeper Insights into Graph Convolutional Networks for Semi-Supervised Learning; AAAI").
- **Oversmoothing** - Interprets GCN propagation as Laplacian smoothing; repeated smoothing can make node representations difficult to distinguish. Describes a depth-related failure mechanism, not a universal limit on GNN depth. [Li et al. 2018](<https://arxiv.org/abs/1801.07606> "Deeper Insights into Graph Convolutional Networks for Semi-Supervised Learning; AAAI").
- **Jumping Knowledge Networks** - Combines representations from different layer depths for each node, allowing the effective neighborhood range to vary instead of using only the final layer. [Xu et al. 2018](<https://arxiv.org/abs/1806.03536> "Representation Learning on Graphs with Jumping Knowledge Networks; ICML").
- **GCNII** - Adds initial residual connections and identity mappings to graph convolution, retaining input information through deep propagation. Studies these mechanisms as remedies for degradation with depth. [Chen et al. 2020](<https://arxiv.org/abs/2007.02133> "Simple and Deep Graph Convolutional Networks; ICML").
- **DropEdge** - Randomly removes graph edges at each training epoch to regularize graph convolution and alter propagation. Restores the input graph for evaluation; analyzes conditions that slow oversmoothing. [Rong et al. 2020](<https://arxiv.org/abs/1907.10903> "DropEdge: Towards Deep Graph Convolutional Networks on Node Classification; ICLR").
- **PairNorm** - Centers and rescales node features to keep their total pairwise squared distance fixed across layers. Counters representation collapse without preserving every individual pairwise distance. [Zhao and Akoglu 2020](<https://arxiv.org/abs/1909.12223> "PairNorm: Tackling Oversmoothing in GNNs; ICLR").
- **Oversquashing** - Studies bottlenecks when messages from expanding neighborhoods are compressed into fixed-size node vectors, hindering long-range signals even when a model can fit local dependencies. [Alon and Yahav 2021](<https://arxiv.org/abs/2006.05205> "On the Bottleneck of Graph Neural Networks and its Practical Implications; ICLR").
- **GraphNorm** - Normalizes features within each graph, with a learnable shift that controls how much mean information is removed. Addresses information loss from ordinary instance normalization on regular graphs. [Cai et al. 2021](<https://arxiv.org/abs/2009.03294> "GraphNorm: A Principled Approach to Accelerating Graph Neural Network Training; ICML").

## Learning and rewiring graph structure

- **Learning Discrete Structures** - Jointly learns GCN parameters and a probability distribution over edges through bilevel optimization. Applies when the supplied graph is missing, incomplete, or corrupted. [Franceschi et al. 2019](<https://proceedings.mlr.press/v97/franceschi19a.html> "Learning Discrete Structures for Graph Neural Networks; ICML").
- **Curvature and oversquashing** - Relates graph bottlenecks to message attenuation through an edge-curvature analysis. Proposes curvature-based rewiring to improve information flow across those bottlenecks. [Topping et al. 2022](<https://arxiv.org/abs/2111.14522> "Understanding over-squashing and bottlenecks on graphs via curvature; ICLR").
- **FoSR** - Selects added edges using a first-order approximation to spectral-gap improvement. Treats original and added edges as distinct relations so the model can retain information about the input graph. [Karhadkar et al. 2023](<https://arxiv.org/abs/2210.11790> "FoSR: First-order spectral rewiring for addressing oversquashing in GNNs; ICLR").

## Global attention

- **Graphormer** - Adds degree-based centrality, shortest-path distances, and path-edge encodings to Transformer attention, allowing graph-wide interactions to depend on graph structure. [Ying et al. 2021](<https://arxiv.org/abs/2106.05234> "Do Transformers Really Perform Bad for Graph Representation?; NeurIPS").
- **GraphGPS** - Combines positional or structural encodings, local message passing, and global attention. Scaling depends on the chosen attention mechanism; ordinary dense attention remains quadratic in node count. [Rampášek et al. 2022](<https://arxiv.org/abs/2205.12454> "Recipe for a General, Powerful, Scalable Graph Transformer; NeurIPS").
- **TokenGT** - Treats nodes and edges as tokens, augmenting them with orthonormal node identifiers and trainable type identifiers before a standard Transformer. Encodes graph connectivity through token representations. [Kim et al. 2022](<https://arxiv.org/abs/2207.02505> "Pure Transformers are Powerful Graph Learners; NeurIPS").
- **SignNet / BasisNet** - Encode spectral eigenvectors while respecting sign flips or basis changes within repeated-eigenvalue eigenspaces. Separate graph structure from arbitrary spectral coordinates. [Lim et al. 2023](<https://arxiv.org/abs/2202.13013> "Sign and Basis Invariant Networks for Spectral Graph Representation Learning; ICLR").
- **Exphormer** - Uses sparse attention with expander edges and virtual global nodes. Adds routes for graph-wide communication without requiring all-pairs attention. [Shirzad et al. 2023](<https://proceedings.mlr.press/v202/shirzad23a.html> "Exphormer: Sparse Transformers for Graphs; ICML").
- **PRiSM** - Jointly canonicalizes spectral signs and node order. For every fixed k, its k-WL counterexamples are simple-spectrum multigraphs. Canonicalization is discontinuous; repeated-eigenvalue handling is heuristic. [Hordan et al. 2026](<https://arxiv.org/abs/2605.23446v1> "Weisfeiler-Leman Is Incomplete on Simple Spectrum Graphs, so Canonicalize Them; NeurIPS").

## Graph pooling

- **DiffPool** - Learns soft assignments of nodes to clusters, producing coarsened graphs for subsequent layers. Adds hierarchical representations for graph-level prediction. [Ying et al. 2018](<https://arxiv.org/abs/1806.08804> "Hierarchical Graph Representation Learning with Differentiable Pooling; NeurIPS").
- **Graph U-Nets** - Pools by selecting nodes using learned projection scores; unpooling restores their positions. Builds an encoder-decoder architecture for node- and graph-level prediction. [Gao and Ji 2019](<https://proceedings.mlr.press/v97/gao19a.html> "Graph U-Nets; ICML").
- **MinCutPool** - Learns soft cluster assignments using a continuous relaxation of normalized min-cut. Produces differentiable graph pooling without recomputing a spectral decomposition for each graph. [Bianchi et al. 2020](<https://proceedings.mlr.press/v119/bianchi20a.html> "Spectral Clustering with Graph Neural Networks for Graph Pooling; ICML").

## Scaling to large graphs

- **FastGCN** - Interprets graph convolution as an integral transform and uses Monte Carlo importance sampling to approximate it. Enables minibatch training without expanding every receptive field in full. [Chen et al. 2018](<https://arxiv.org/abs/1801.10247> "FastGCN: Fast Learning with Graph Convolutional Networks via Importance Sampling; ICLR").
- **SGC** - Removes intermediate nonlinearities and collapses weight matrices, yielding fixed graph propagation followed by a linear classifier. A baseline for testing whether learned nonlinear propagation is needed. [Wu et al. 2019](<https://proceedings.mlr.press/v97/wu19e.html> "Simplifying Graph Convolutional Networks; ICML").
- **Cluster-GCN** - Partitions the graph and trains on subgraphs formed from clusters, restricting neighborhood computation within each minibatch to reduce memory and repeated neighbor expansion. [Chiang et al. 2019](<https://arxiv.org/abs/1905.07953> "Cluster-GCN: An Efficient Algorithm for Training Deep and Large Graph Convolutional Networks; KDD").
- **LADIES** - Conditions layer-wise importance sampling on nodes sampled in the next layer. Restricts candidates to their neighbors to preserve connections between sampled layers. [Zou et al. 2019](<https://arxiv.org/abs/1911.07323> "Layer-Dependent Importance Sampling for Training Deep and Large Graph Convolutional Networks; NeurIPS").
- **GraphSAINT** - Samples subgraphs and reweights aggregation and loss by sampling probabilities. Its unbiasedness analysis treats layers independently; it does not establish unbiased gradients through the nonlinear stack. [Zeng et al. 2020](<https://arxiv.org/abs/1907.04931> "GraphSAINT: Graph Sampling Based Inductive Learning Method; ICLR").
- **SIGN** - Precomputes features using several graph filters and trains a feedforward model on them. Moves graph propagation out of the training loop, trading preprocessing and feature storage for simpler minibatches. [Frasca et al. 2020](<https://arxiv.org/abs/2004.11198> "SIGN: Scalable Inception Graph Neural Networks; arXiv").

## Learning under heterophily

- **MixHop** - Mixes feature representations from neighborhoods at different distances, enabling operators such as differences between neighborhood scales. Studies how useful mixing patterns vary across graphs. [Abu-El-Haija et al. 2019](<https://arxiv.org/abs/1905.00067> "MixHop: Higher-Order Graph Convolutional Architectures via Sparsified Neighborhood Mixing; ICML").
- **H2GCN** - Separates self and neighbor embeddings, includes higher-order neighborhoods, and combines intermediate representations to handle graphs whose adjacent nodes may have different labels or features. [Zhu et al. 2020](<https://arxiv.org/abs/2006.11468> "Beyond Homophily in Graph Neural Networks: Current Limitations and Effective Designs; NeurIPS").
- **GPR-GNN** - Learns weights over propagation depths in generalized PageRank, adapting the graph filter to label and feature patterns. Evaluates both homophilous and heterophilous settings. [Chien et al. 2021](<https://arxiv.org/abs/2006.07988> "Adaptive Universal Generalized PageRank Graph Neural Network; ICLR").
- **FAGCN** - Uses self-gating to combine low- and high-frequency graph signals. Retains differences between neighboring nodes that low-pass smoothing can suppress. [Bo et al. 2021](<https://arxiv.org/abs/2101.00797> "Beyond Low-frequency Information in Graph Convolutional Networks; AAAI").
- **LINKX** - Embeds adjacency information and node features with separate networks before combining them with an MLP; a graph-learning baseline for weak homophily without repeated neighborhood propagation. [Lim et al. 2021](<https://arxiv.org/abs/2110.14446> "Large Scale Learning on Non-Homophilous Graphs: New Benchmarks and Strong Simple Methods; NeurIPS").
- **Reassessing heterophily benchmarks** - Finds duplicate-node leakage in Squirrel and Chameleon; introduces alternative benchmarks where standard GNNs are competitive. Low label homophily alone does not establish a need for a specialized model. [Platonov et al. 2023](<https://arxiv.org/abs/2302.11640> "A critical look at the evaluation of GNNs under heterophily: Are we really making progress?; ICLR").

## Link prediction and recommendation

[Evaluation](#benchmarks-and-evaluation): HeaRT compares endpoint-specific candidates.

- **Variational Graph Auto-Encoders** - Uses a GCN encoder, Gaussian latent node variables, and an inner-product decoder to reconstruct adjacency. An unsupervised baseline for link prediction. [Kipf and Welling 2016](<https://arxiv.org/abs/1611.07308> "Variational Graph Auto-Encoders; NeurIPS BDL Workshop").
- **SEAL** - Predicts links from local enclosing subgraphs with labels marking nodes' positions relative to the candidate pair. Removes the target edge from positive training subgraphs to avoid exposing the answer. [Zhang and Chen 2018](<https://arxiv.org/abs/1802.09691> "Link Prediction Based on Graph Neural Networks; NeurIPS").
- **LightGCN** - Learns user and item embeddings through linear propagation on the interaction graph and combines layers. Removes feature transformations and nonlinearities from graph convolution for collaborative filtering. [He et al. 2020](<https://arxiv.org/abs/2002.02126> "LightGCN: Simplifying and Powering Graph Convolution Network for Recommendation; arXiv").
- **Neural Bellman-Ford Networks** - Learns node-pair representations through generalized Bellman-Ford updates with neural initialization, message, and aggregation operators. Uses path information for link prediction. [Zhu et al. 2021](<https://arxiv.org/abs/2106.06935> "Neural Bellman-Ford Networks: A General Graph Neural Network Framework for Link Prediction; NeurIPS").

## Directed graph structure

- **MagNet** - Uses a complex Hermitian magnetic Laplacian: magnitudes encode undirected structure and phases encode edge direction. Supports spectral learning on directed graphs; symmetrizing adjacency would discard information that this construction retains. [Zhang et al. 2021](<https://arxiv.org/abs/2102.11391> "MagNet: A Neural Network for Directed Graphs; NeurIPS").

## Typed graphs

- **R-GCN** - Uses relation-specific transformations for multi-relational graphs, with parameter sharing to control model size. Applies the encoder to entity classification and link prediction. [Schlichtkrull et al. 2018](<https://arxiv.org/abs/1703.06103> "Modeling Relational Data with Graph Convolutional Networks; ESWC").
- **Heterogeneous Graph Attention Network** - Learns attention over neighbors connected by typed relation paths, then over path types. Separates node-level aggregation from weighting different semantic relationships. [Wang et al. 2019](<https://arxiv.org/abs/1903.07293> "Heterogeneous Graph Attention Network; WWW").
- **Heterogeneous Graph Transformer** - Conditions attention and message transformations on node and edge types; adds relative temporal encoding and heterogeneous graph sampling for large typed networks. [Hu et al. 2020](<https://arxiv.org/abs/2003.01332> "Heterogeneous Graph Transformer; WWW").

## Temporal graphs

- **TGAT** - Combines temporal neighborhood attention with continuous time encodings to produce time-dependent embeddings for observed and previously unseen nodes. [Da Xu et al. 2020](<https://arxiv.org/abs/2002.07962> "Inductive Representation Learning on Temporal Graphs; ICLR").
- **Temporal Graph Networks** - Represents dynamic graphs as timestamped events; combines node memory with graph-based embedding operators to track evolving interactions. [Rossi et al. 2020](<https://arxiv.org/abs/2006.10637> "Temporal Graph Networks for Deep Learning on Dynamic Graphs; arXiv").
- **DyGFormer / DyGLib** - Encodes historical first-hop interactions with neighbor co-occurrence features and sequence patches. Also supplies shared training and evaluation pipelines for comparing dynamic graph models. [Le Yu et al. 2023](<https://arxiv.org/abs/2303.13047> "Towards Better Dynamic Graph Learning: New Architecture and Unified Library; NeurIPS").

## Higher-order relations and topology

- **Hypergraph Neural Networks** - Incidence-based aggregation reduces to a normalized weighted clique-expansion operator with self-contributions. Encoding group relations alone does not guarantee higher-order expressivity. [Feng et al. 2019](<https://arxiv.org/abs/1809.09401> "Hypergraph Neural Networks; AAAI").
- **Simplicial message passing** - Passes messages over simplicial complexes, where edges, triangles and higher-dimensional simplices represent interactions; incorporates topology beyond ordinary node-edge connectivity. [Bodnar et al. 2021](<https://arxiv.org/abs/2103.03212> "Weisfeiler and Lehman Go Topological: Message Passing Simplicial Networks; ICML").

## Geometric and molecular graphs

- **Geometric graphs** - Positions add Euclidean symmetries beyond node relabeling. See [Geometric deep learning](https://mlpapers.org/geometric-deep-learning/#molecular-and-atomistic-representations) for DimeNet, NequIP and MACE; MeshGraphNets below connects message passing to adaptive-mesh simulation.
- **SchNet** - Uses continuous-filter convolutions for atoms at continuous positions; learns rotation-invariant energies and a differentiable potential for interatomic forces. [Schütt et al. 2017](<https://arxiv.org/abs/1706.08566> "SchNet: A continuous-filter convolutional neural network for modeling quantum interactions; NeurIPS").
- **EGNN** - Builds graph layers equivariant to rotations, translations, reflections, and node permutations, using scalar features and coordinate updates without higher-order tensor representations. [Satorras et al. 2021](<https://proceedings.mlr.press/v139/satorras21a.html> "E(n) Equivariant Graph Neural Networks; ICML").
- **GemNet** - Combines directed edge embeddings with two-hop geometric message passing for molecular prediction. Develops the architecture from an analysis of GNNs with spherical representations. [Gasteiger et al. 2021](<https://arxiv.org/abs/2106.08903> "GemNet: Universal Directional Graph Neural Networks for Molecules; NeurIPS").
- **MeshGraphNets** - Learns physical dynamics through message passing on meshes, with adaptive discretization. Studies fluid, structural, and cloth simulations on irregular domains. [Pfaff et al. 2021](<https://arxiv.org/abs/2010.03409> "Learning Mesh-Based Simulation with Graph Networks; ICLR").

## Self-supervised objectives

- **Deep Graph Infomax** - Trains a discriminator to distinguish real node-graph-summary pairs from corrupted pairs, providing a mutual-information-inspired objective for unsupervised node representations. [Veličković et al. 2019](<https://arxiv.org/abs/1809.10341> "Deep Graph Infomax; ICLR").
- **GraphCL** - Learns graph representations by contrasting augmented views; studies node dropping, edge perturbation, attribute masking, and subgraph sampling as task-dependent augmentation choices. [You et al. 2020](<https://arxiv.org/abs/2010.13902> "Graph Contrastive Learning with Augmentations; NeurIPS").
- **GRACE** - Contrasts node representations across two graph views formed by corrupting structure and attributes. Encourages agreement for the same node while contrasting other nodes. [Zhu et al. 2020](<https://arxiv.org/abs/2006.04131> "Deep Graph Contrastive Representation Learning; arXiv").
- **GraphMAE** - Masks node features and reconstructs them with a graph autoencoder using scaled cosine error. The pretraining target is feature reconstruction rather than adjacency reconstruction. [Hou et al. 2022](<https://arxiv.org/abs/2205.10803> "GraphMAE: Self-Supervised Masked Graph Autoencoders; KDD").
    - **GraphMAE2** - Extends masked feature reconstruction with multiple randomly re-masked decoding views and latent representation prediction. Regularizes reconstruction in both feature and embedding spaces. [Hou et al. 2023](<https://arxiv.org/abs/2304.04779> "GraphMAE2: A Decoding-Enhanced Masked Self-Supervised Graph Learner; WWW").
- **BGRL** - Learns node representations by predicting a target encoder's output across augmented graph views. Uses a bootstrap objective without explicit negative pairs. [Thakoor et al. 2022](<https://arxiv.org/abs/2102.06514> "Large-Scale Representation Learning on Graphs via Bootstrapping; ICLR").

## Transfer across graphs

- **Strategies for Pre-training GNNs** - Combines node- and graph-level pretraining to learn local and global representations. Shows cases where pretraining at only one level causes negative transfer. [Hu et al. 2020](<https://arxiv.org/abs/1905.12265> "Strategies for Pre-training Graph Neural Networks; ICLR").
- **ULTRA** - Conditions relation representations on relation interactions, enabling a pretrained link predictor to transfer across knowledge graphs with different entity and relation vocabularies. [Galkin et al. 2024](<https://arxiv.org/abs/2310.04562> "Towards Foundation Models for Knowledge Graph Reasoning; ICLR").
- **GraphAny** - Combines analytical linear graph predictors with learned attention across new feature and label spaces. Inference still uses a labeled subset of the target graph; no target gradient updates are required. [Zhao et al. 2025](<https://arxiv.org/abs/2405.20445> "Fully-inductive Node Classification on Arbitrary Graphs; ICLR").

## Graph generation

[Evaluation](#benchmarks-and-evaluation): graph-statistic and kernel choices affect generator rankings.

- **GraphRNN** - Decomposes graph generation into autoregressive node and edge decisions conditioned on the graph built so far. Evaluates generated graph sets through distributions of structural statistics. [You et al. 2018](<https://proceedings.mlr.press/v80/you18a.html> "GraphRNN: Generating Realistic Graphs with Deep Auto-regressive Models; ICML").
- **GraphAF** - Uses an autoregressive normalizing flow to generate molecular graphs. Combines parallel likelihood training with sequential sampling that can check atom valence as atoms and bonds are added. [Shi et al. 2020](<https://arxiv.org/abs/2001.09382> "GraphAF: a Flow-based Autoregressive Model for Molecular Graph Generation; ICLR").
- **DiGress** - Learns to reverse discrete noise on categorical node and edge attributes with a graph Transformer. Generates molecular and other graphs through successive denoising steps. [Vignac et al. 2023](<https://arxiv.org/abs/2209.14734> "DiGress: Discrete Denoising diffusion for graph generation; ICLR").
- **DeFoG** - Applies discrete flow matching to graph generation. Separates the training objective from sampling choices, allowing generation procedures to be changed without retraining the predictor. [Qin et al. 2025](<https://arxiv.org/abs/2410.04263> "DeFoG: Discrete Flow Matching for Graph Generation; ICML").

## Benchmarks and evaluation

- **Evaluation pitfalls** - Shows that data splits, hyperparameter tuning, and early stopping can change GNN rankings. Compares architectures under shared protocols rather than inherited results on a single split. [Shchur et al. 2018](<https://arxiv.org/abs/1811.05868> "Pitfalls of Graph Neural Network Evaluation; NeurIPS R2L Workshop").
- **Open Graph Benchmark** - Provides node, link, and graph prediction tasks with application-specific splits and metrics. Includes large graphs and distribution shifts beyond small citation-network benchmarks. [Hu et al. 2020](<https://arxiv.org/abs/2005.00687> "Open Graph Benchmark: Datasets for Machine Learning on Graphs; NeurIPS").
- **Long Range Graph Benchmark** - Introduces five tasks intended to test long-range graph interactions across molecular and image-derived graphs, with message-passing and Transformer baselines. [Dwivedi et al. 2022](<https://arxiv.org/abs/2206.08164> "Long Range Graph Benchmark; NeurIPS").
    - **Reassessing LRGB** - With roughly 500k parameters, tuned GCN/GINE/GatedGCN beat retuned GPS on both peptide tasks; a deeper prediction head explains much of the gain. GPS still leads these baselines on both superpixel tasks. [Tönshoff et al. 2023](<https://arxiv.org/html/2309.00367v2> "Where Did the Gap Go? Reassessing the Long-Range Graph Benchmark; arXiv").
    - **LRGB protocol** - Input normalization improves the compared vision results. The 2023 audit found raw rather than specified filtered MRR in PCQM-Contact code. Filtering other positive tails and self-loops changes scores. [Tönshoff et al. 2023](<https://arxiv.org/html/2309.00367v2> "Where Did the Gap Go? Reassessing the Long-Range Graph Benchmark; arXiv").
- **Graph-generation evaluation** - MMD rankings can reverse with descriptor, kernel, bandwidth or histogram choices. Matching degree or clustering summaries does not establish matching graph distributions; record metric settings. [O'Bray et al. 2022](<https://arxiv.org/html/2106.01098v3> "Evaluation Metrics for Graph Generative Models: Problems, Pitfalls, and Practical Solutions; ICLR").
- **HeaRT** - Ranks links against 500 endpoint-specific negatives selected by heuristics with random fill. Changing validation/test negatives, not training, changes rankings across seven graphs and makes simple baselines more competitive; this is not full-catalog retrieval. [Li et al. 2023](<https://arxiv.org/html/2306.10453v3> "Evaluating Graph Neural Networks for Link Prediction: Current Pitfalls and New Benchmarking; NeurIPS").
- **Temporal Graph Benchmark** - Moving averages beat TGN/DyRep on four node-affinity tasks (NDCG@10); persistence leads on trade. TGN leads mean link-prediction MRR on coin, comment and flight; CAWN, TCL and GraphMixer exhaust GPU memory. [Huang et al. 2023](<https://arxiv.org/html/2307.01026v2> "Temporal Graph Benchmark for Machine Learning on Temporal Graphs; NeurIPS").
- **Classic GNNs as strong baselines** - Reassesses GCN, GAT, and GraphSAGE against graph Transformers; tuning normalization, dropout, residual connections, and depth changes the comparison on studied node-classification datasets. [Luo et al. 2024](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/b10ed15ff1aa864f1be3a75f1ffc021b-Abstract-Datasets_and_Benchmarks_Track.html> "Classic GNNs are Strong Baselines: Reassessing GNNs for Node Classification; NeurIPS").

<a id="comparisons-to-run"></a>

## Software

- **PyTorch Geometric** - Graph data containers, message-passing layers, pooling operators, sampling, and dataset utilities for PyTorch. [docs](<https://pytorch-geometric.readthedocs.io/en/latest/>).
- **DGL** - Graph computation and message-passing APIs, heterogeneous graph support, sampling, and distributed graph-learning tools. [docs](<https://www.dgl.ai/dgl_docs/>).
- **OGB tools** - Dataset loaders, prescribed splits, evaluators, and baseline implementations. Use each task's specified metric and candidate-set protocol when comparing link predictors. [docs](<https://ogb.stanford.edu/docs/home/>).

## Related topics

- [Geometric deep learning](https://mlpapers.org/geometric-deep-learning/)
- [Neural networks](https://mlpapers.org/neural-nets/)
- [Bayesian networks](https://mlpapers.org/bayesian-nets/)
