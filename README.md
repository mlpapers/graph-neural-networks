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

- **[The Graph Neural Network Model](https://usiena-air.unisi.it/handle/11365/26421)** — Scarselli et al., 2009, IEEE TNN. Learns node states from graph structure and attributes through recurrent updates; the original formulation uses a contraction condition to obtain a unique fixed point.
- **[Graph networks](https://arxiv.org/abs/1806.01261)** — Battaglia et al., 2018 preprint. Unifies computations on nodes, edges, and global graph attributes through shared update functions and permutation-invariant aggregation; connects graph structure to relational inductive biases.
- **[Message Passing Neural Networks](https://proceedings.mlr.press/v70/gilmer17a.html)** — Gilmer et al., 2017, ICML. Separates local message computation and node updates from a graph-level readout. Unifies several molecular prediction architectures within one framework.

## Random-walk embedding baselines

- **[DeepWalk](https://arxiv.org/abs/1403.6652)** — Perozzi et al., 2014, KDD. Treats short random walks as sequences and learns node vectors from their local contexts. Provides a graph-structure embedding baseline without a learned neighborhood feature aggregator.
- **[node2vec](https://arxiv.org/abs/1607.00653)** — Grover & Leskovec, 2016, KDD. Learns node vectors from biased random walks, with parameters controlling which neighborhoods are explored. Changes the context distribution used to train embeddings.

## Neighborhood aggregation

- **[Spectral networks](https://arxiv.org/abs/1312.6203)** — Bruna et al., 2013 preprint. Extends convolution to graphs through filters in the graph Laplacian's eigenbasis; also studies locally connected architectures based on hierarchical clustering.
- **[ChebNet](https://arxiv.org/abs/1606.09375)** — Defferrard et al., 2016, NeurIPS. Uses Chebyshev polynomials of the graph Laplacian to construct localized spectral filters, avoiding explicit eigendecomposition during filtering.
- **[GCN](https://arxiv.org/abs/1609.02907)** — Kipf & Welling, 2017, ICLR. Alternates learned feature transformations with degree-normalized neighborhood aggregation, including self-loops. Its semi-supervised setup uses graph structure and features beyond the labeled nodes.
- **[GraphSAGE](https://arxiv.org/abs/1706.02216)** — Hamilton et al., 2017, NeurIPS. Samples neighborhoods and learns feature-aggregation functions that can embed unseen nodes or graphs sharing the input feature schema, instead of storing a separate learned vector for each node.
- **[GAT](https://arxiv.org/abs/1710.10903)** — Veličković et al., 2018, ICLR. Learns attention weights over graph neighborhoods to combine node features; multi-head attention provides several learned aggregations. The original attention is restricted to graph neighbors.
- **[GATv2](https://arxiv.org/abs/2105.14491)** — Brody et al., 2022, ICLR. Reorders attention operations so neighbor rankings can depend on the query node, addressing the original GAT's static attention ranking.
- **[Principal Neighbourhood Aggregation](https://arxiv.org/abs/2004.05718)** — Corso et al., 2020, NeurIPS. Combines multiple neighborhood statistics with degree-dependent scaling. Addresses information lost when a single aggregator summarizes continuous node features.

## Diffusion and propagation

- **[PPNP / APPNP](https://arxiv.org/abs/1810.05997)** — Gasteiger et al., 2019, ICLR. Separates neural prediction from personalized PageRank propagation. APPNP approximates propagation iteratively, extending information flow without stacking more learned feature transformations.
- **[Graph Diffusion Convolution](https://arxiv.org/abs/1911.05485)** — Gasteiger et al., 2019, NeurIPS. Constructs a new graph by sparsifying generalized diffusion, including personalized PageRank or heat kernels. Subsequent message passing uses these diffusion-derived connections.

## Expressivity and higher-order structure

- **[GIN and the 1-WL limit](https://arxiv.org/abs/1810.00826)** — Xu et al., 2019, ICLR. With enough layers, injective aggregation, updates and readout can match 1-WL. The sum construction assumes countable features and bounded multiset size.
- **[GIN guarantee](https://arxiv.org/html/1810.00826v3)** — The result requires suitable injective functions; it does not guarantee that a trained network learns them. Matching 1-WL still leaves some non-isomorphic graphs indistinguishable.
- **[Higher-order GNNs](https://arxiv.org/abs/1810.02244)** — Morris et al., 2019, AAAI. Relates GNNs to Weisfeiler–Leman refinement and extends aggregation to sets of nodes. Higher-order representations distinguish structures that ordinary node-level message passing can miss.
- **[Provably Powerful Graph Networks](https://arxiv.org/abs/1905.11136)** — Maron et al., 2019, NeurIPS. Uses matrix-valued representations, multilayer perceptrons, and matrix multiplication to exceed standard message-passing expressivity under the paper's graph-isomorphism analysis.
- **[Graph Substructure Networks](https://arxiv.org/abs/2006.09252)** — Bouritsas et al., 2020 preprint. Augments message passing with structural features from subgraph-isomorphism counts. Expressivity depends on the chosen substructures; computing those counts adds preprocessing work.
- **[Equivariant Subgraph Aggregation Networks](https://arxiv.org/abs/2110.02910)** — Bevilacqua et al., 2022, ICLR. Represents a graph as a set of selected subgraphs and aggregates them equivariantly. Subgraph selection affects expressivity; sampling reduces the added computation.

## Depth and normalization

- **Different limits** — [1-WL](https://arxiv.org/abs/1810.00826) concerns structural discrimination; [oversquashing](https://arxiv.org/abs/2006.05205) concerns information transport; [oversmoothing](https://arxiv.org/abs/1801.07606) concerns representation collapse. Stronger aggregation, rewiring and normalization address different limits.
- **[Oversmoothing](https://arxiv.org/abs/1801.07606)** — Li et al., 2018, AAAI. Interprets GCN propagation as Laplacian smoothing; repeated smoothing can make node representations difficult to distinguish. Describes a depth-related failure mechanism, not a universal limit on GNN depth.
- **[Oversquashing](https://arxiv.org/abs/2006.05205)** — Alon & Yahav, 2020 preprint; ICLR 2021. Studies bottlenecks when messages from expanding neighborhoods are compressed into fixed-size node vectors, hindering long-range signals even when a model can fit local dependencies.
- **[Jumping Knowledge Networks](https://arxiv.org/abs/1806.03536)** — Xu et al., 2018, ICML. Combines representations from different layer depths for each node, allowing the effective neighborhood range to vary instead of using only the final layer.
- **[GCNII](https://arxiv.org/abs/2007.02133)** — Chen et al., 2020, ICML. Adds initial residual connections and identity mappings to graph convolution, retaining input information through deep propagation. Studies these mechanisms as remedies for degradation with depth.
- **[DropEdge](https://arxiv.org/abs/1907.10903)** — Rong et al., 2020, ICLR. Randomly removes graph edges at each training epoch to regularize graph convolution and alter propagation. Restores the input graph for evaluation; analyzes conditions that slow oversmoothing.
- **[PairNorm](https://arxiv.org/abs/1909.12223)** — Zhao & Akoglu, 2020, ICLR. Centers and rescales node features to keep their total pairwise squared distance fixed across layers. Counters representation collapse without preserving every individual pairwise distance.
- **[GraphNorm](https://arxiv.org/abs/2009.03294)** — Cai et al., 2021, ICML. Normalizes features within each graph, with a learnable shift that controls how much mean information is removed. Addresses information loss from ordinary instance normalization on regular graphs.

## Learning and rewiring graph structure

- **[Learning Discrete Structures](https://proceedings.mlr.press/v97/franceschi19a.html)** — Franceschi et al., 2019, ICML. Jointly learns GCN parameters and a probability distribution over edges through bilevel optimization. Applies when the supplied graph is missing, incomplete, or corrupted.
- **[Curvature and oversquashing](https://arxiv.org/abs/2111.14522)** — Topping et al., 2022, ICLR. Relates graph bottlenecks to message attenuation through an edge-curvature analysis. Proposes curvature-based rewiring to improve information flow across those bottlenecks.
- **[FoSR](https://arxiv.org/abs/2210.11790)** — Karhadkar et al., 2023, ICLR. Selects added edges using a first-order approximation to spectral-gap improvement. Treats original and added edges as distinct relations so the model can retain information about the input graph.

## Global attention

- **[SignNet / BasisNet](https://arxiv.org/abs/2202.13013)** — Lim et al., 2022 preprint. Encode spectral eigenvectors while respecting sign flips or basis changes within repeated-eigenvalue eigenspaces. Separate graph structure from arbitrary spectral coordinates.
- **[Graphormer](https://arxiv.org/abs/2106.05234)** — Ying et al., 2021, NeurIPS. Adds degree-based centrality, shortest-path distances, and path-edge encodings to Transformer attention, allowing graph-wide interactions to depend on graph structure.
- **[GraphGPS](https://arxiv.org/abs/2205.12454)** — Rampášek et al., 2022, NeurIPS. Combines positional or structural encodings, local message passing, and global attention. Scaling depends on the chosen attention mechanism; ordinary dense attention remains quadratic in node count.
- **[TokenGT](https://arxiv.org/abs/2207.02505)** — Kim et al., 2022, NeurIPS. Treats nodes and edges as tokens, augmenting them with orthonormal node identifiers and trainable type identifiers before a standard Transformer. Encodes graph connectivity through token representations.
- **[Exphormer](https://proceedings.mlr.press/v202/shirzad23a.html)** — Shirzad et al., 2023, ICML. Uses sparse attention with expander edges and virtual global nodes. Adds routes for graph-wide communication without requiring all-pairs attention.

## Graph pooling

- **[DiffPool](https://arxiv.org/abs/1806.08804)** — Ying et al., 2018, NeurIPS. Learns soft assignments of nodes to clusters, producing coarsened graphs for subsequent layers. Adds hierarchical representations for graph-level prediction.
- **[Graph U-Nets](https://proceedings.mlr.press/v97/gao19a.html)** — Gao & Ji, 2019, ICML. Pools by selecting nodes using learned projection scores; unpooling restores their positions. Builds an encoder–decoder architecture for node- and graph-level prediction.
- **[MinCutPool](https://proceedings.mlr.press/v119/bianchi20a.html)** — Bianchi et al., 2020, ICML. Learns soft cluster assignments using a continuous relaxation of normalized min-cut. Produces differentiable graph pooling without recomputing a spectral decomposition for each graph.

## Scaling to large graphs

- **[SGC](https://proceedings.mlr.press/v97/wu19e.html)** — Wu et al., 2019, ICML. Removes intermediate nonlinearities and collapses weight matrices, yielding fixed graph propagation followed by a linear classifier. A baseline for testing whether learned nonlinear propagation is needed.
- **[Cluster-GCN](https://arxiv.org/abs/1905.07953)** — Chiang et al., 2019, KDD. Partitions the graph and trains on subgraphs formed from clusters, restricting neighborhood computation within each minibatch to reduce memory and repeated neighbor expansion.
- **[GraphSAINT](https://arxiv.org/abs/1907.04931)** — Zeng et al., 2020, ICLR. Samples subgraphs and reweights aggregation and loss by sampling probabilities. Its unbiasedness analysis treats layers independently; it does not establish unbiased gradients through the nonlinear stack.
- **[FastGCN](https://arxiv.org/abs/1801.10247)** — Chen et al., 2018, ICLR. Interprets graph convolution as an integral transform and uses Monte Carlo importance sampling to approximate it. Enables minibatch training without expanding every receptive field in full.
- **[LADIES](https://arxiv.org/abs/1911.07323)** — Zou et al., 2019, NeurIPS. Conditions layer-wise importance sampling on nodes sampled in the next layer. Restricts candidates to their neighbors to preserve connections between sampled layers.
- **[SIGN](https://arxiv.org/abs/2004.11198)** — Frasca et al., 2020 preprint. Precomputes features using several graph filters and trains a feedforward model on them. Moves graph propagation out of the training loop, trading preprocessing and feature storage for simpler minibatches.

## Learning under heterophily

- **[MixHop](https://arxiv.org/abs/1905.00067)** — Abu-El-Haija et al., 2019, ICML. Mixes feature representations from neighborhoods at different distances, enabling operators such as differences between neighborhood scales. Studies how useful mixing patterns vary across graphs.
- **[H2GCN](https://arxiv.org/abs/2006.11468)** — Zhu et al., 2020, NeurIPS. Separates self and neighbor embeddings, includes higher-order neighborhoods, and combines intermediate representations to handle graphs whose adjacent nodes may have different labels or features.
- **[Reassessing heterophily benchmarks](https://arxiv.org/abs/2302.11640)** — Platonov et al., 2023, ICLR. Finds duplicate-node leakage in Squirrel and Chameleon; introduces alternative benchmarks where standard GNNs are competitive. Low label homophily alone does not establish a need for a specialized model.
- **[GPR-GNN](https://arxiv.org/abs/2006.07988)** — Chien et al., 2021, ICLR. Learns weights over propagation depths in generalized PageRank, adapting the graph filter to label and feature patterns. Evaluates both homophilous and heterophilous settings.
- **[FAGCN](https://arxiv.org/abs/2101.00797)** — Bo et al., 2021, AAAI. Uses self-gating to combine low- and high-frequency graph signals. Retains differences between neighboring nodes that low-pass smoothing can suppress.
- **[LINKX](https://arxiv.org/abs/2110.14446)** — 2021. Embeds adjacency information and node features with separate networks before combining them with an MLP; a graph-learning baseline for weak homophily without repeated neighborhood propagation.

## Link prediction and recommendation

[Evaluation](#benchmarks-and-evaluation): HeaRT compares endpoint-specific candidates.

- **[Variational Graph Auto-Encoders](https://arxiv.org/abs/1611.07308)** — Kipf & Welling, 2016, Bayesian Deep Learning Workshop. Uses a GCN encoder, Gaussian latent node variables, and an inner-product decoder to reconstruct adjacency. An unsupervised baseline for link prediction.
- **[SEAL](https://arxiv.org/abs/1802.09691)** — Zhang & Chen, 2018, NeurIPS. Predicts links from local enclosing subgraphs with labels marking nodes' positions relative to the candidate pair. Removes the target edge from positive training subgraphs to avoid exposing the answer.
- **[Neural Bellman–Ford Networks](https://arxiv.org/abs/2106.06935)** — Zhu et al., 2021, NeurIPS. Learns node-pair representations through generalized Bellman–Ford updates with neural initialization, message, and aggregation operators. Uses path information for link prediction.
- **[LightGCN](https://arxiv.org/abs/2002.02126)** — He et al., 2020, SIGIR. Learns user and item embeddings through linear propagation on the interaction graph and combines layers. Removes feature transformations and nonlinearities from graph convolution for collaborative filtering.

## Directed graph structure

- **[MagNet](https://arxiv.org/abs/2102.11391)** — 2021. Uses a complex Hermitian magnetic Laplacian: magnitudes encode undirected structure and phases encode edge direction. Supports spectral learning on directed graphs; symmetrizing adjacency would discard information that this construction retains.

## Typed graphs

- **[R-GCN](https://arxiv.org/abs/1703.06103)** — Schlichtkrull et al., 2017 preprint; ESWC 2018. Uses relation-specific transformations for multi-relational graphs, with parameter sharing to control model size. Applies the encoder to entity classification and link prediction.
- **[Heterogeneous Graph Transformer](https://arxiv.org/abs/2003.01332)** — Hu et al., 2020, WWW. Conditions attention and message transformations on node and edge types; adds relative temporal encoding and heterogeneous graph sampling for large typed networks.
- **[Heterogeneous Graph Attention Network](https://arxiv.org/abs/1903.07293)** — Wang et al., 2019, WWW. Learns attention over neighbors connected by typed relation paths, then over path types. Separates node-level aggregation from weighting different semantic relationships.

## Temporal graphs

- **[TGAT](https://arxiv.org/abs/2002.07962)** — Xu et al., 2020, ICLR. Combines temporal neighborhood attention with continuous time encodings to produce time-dependent embeddings for observed and previously unseen nodes.
- **[Temporal Graph Networks](https://arxiv.org/abs/2006.10637)** — Rossi et al., 2020 preprint. Represents dynamic graphs as timestamped events; combines node memory with graph-based embedding operators to track evolving interactions.
- **[DyGFormer / DyGLib](https://arxiv.org/abs/2303.13047)** — Yu et al., 2023, NeurIPS. Encodes historical first-hop interactions with neighbor co-occurrence features and sequence patches. Also supplies shared training and evaluation pipelines for comparing dynamic graph models.

## Higher-order relations and topology

- **[Hypergraph Neural Networks](https://arxiv.org/abs/1809.09401)** — 2018. Incidence-based aggregation reduces to a normalized weighted clique-expansion operator with self-contributions. Encoding group relations alone does not guarantee higher-order expressivity.
- **[Simplicial message passing](https://arxiv.org/abs/2103.03212)** — 2021. Passes messages over simplicial complexes, where edges, triangles and higher-dimensional simplices represent interactions; incorporates topology beyond ordinary node–edge connectivity.

## Geometric and molecular graphs

- **Geometric graphs** — Positions add Euclidean symmetries beyond node relabeling. See [Geometric deep learning](https://mlpapers.org/geometric-deep-learning/#molecular-and-atomistic-representations) for DimeNet, NequIP and MACE; MeshGraphNets below connects message passing to adaptive-mesh simulation.
- **[SchNet](https://arxiv.org/abs/1706.08566)** — Schütt et al., 2017, NeurIPS. Uses continuous-filter convolutions for atoms at continuous positions; learns rotation-invariant energies and a differentiable potential for interatomic forces.
- **[EGNN](https://proceedings.mlr.press/v139/satorras21a.html)** — Satorras et al., 2021, ICML. Builds graph layers equivariant to rotations, translations, reflections, and node permutations, using scalar features and coordinate updates without higher-order tensor representations.
- **[GemNet](https://arxiv.org/abs/2106.08903)** — Gasteiger et al., 2021, NeurIPS. Combines directed edge embeddings with two-hop geometric message passing for molecular prediction. Develops the architecture from an analysis of GNNs with spherical representations.
- **[MeshGraphNets](https://arxiv.org/abs/2010.03409)** — Pfaff et al., 2020 preprint; ICLR 2021. Learns physical dynamics through message passing on meshes, with adaptive discretization. Studies fluid, structural, and cloth simulations on irregular domains.

## Self-supervised objectives

- **[Deep Graph Infomax](https://arxiv.org/abs/1809.10341)** — Veličković et al., 2019, ICLR. Trains a discriminator to distinguish real node–graph-summary pairs from corrupted pairs, providing a mutual-information-inspired objective for unsupervised node representations.
- **[GraphCL](https://arxiv.org/abs/2010.13902)** — You et al., 2020, NeurIPS. Learns graph representations by contrasting augmented views; studies node dropping, edge perturbation, attribute masking, and subgraph sampling as task-dependent augmentation choices.
- **[GraphMAE](https://arxiv.org/abs/2205.10803)** — Hou et al., 2022, KDD. Masks node features and reconstructs them with a graph autoencoder using scaled cosine error. The pretraining target is feature reconstruction rather than adjacency reconstruction.
- **[GRACE](https://arxiv.org/abs/2006.04131)** — Zhu et al., 2020 preprint. Contrasts node representations across two graph views formed by corrupting structure and attributes. Encourages agreement for the same node while contrasting other nodes.
- **[BGRL](https://arxiv.org/abs/2102.06514)** — Thakoor et al., 2021 preprint; ICLR 2022. Learns node representations by predicting a target encoder's output across augmented graph views. Uses a bootstrap objective without explicit negative pairs.
- **[GraphMAE2](https://arxiv.org/abs/2304.04779)** — Hou et al., 2023, WWW. Extends masked feature reconstruction with multiple randomly re-masked decoding views and latent representation prediction. Regularizes reconstruction in both feature and embedding spaces.

## Transfer across graphs

- **[Strategies for Pre-training GNNs](https://arxiv.org/abs/1905.12265)** — Hu et al., 2020, ICLR. Combines node- and graph-level pretraining to learn local and global representations. Shows cases where pretraining at only one level causes negative transfer.
- **[ULTRA](https://arxiv.org/abs/2310.04562)** — Galkin et al., 2023 preprint; ICLR 2024. Conditions relation representations on relation interactions, enabling a pretrained link predictor to transfer across knowledge graphs with different entity and relation vocabularies.
- **[GraphAny](https://arxiv.org/abs/2405.20445)** — Zhao et al., 2024 preprint; ICLR 2025. Combines analytical linear graph predictors with learned attention across new feature and label spaces. Inference still uses a labeled subset of the target graph; no target gradient updates are required.

## Graph generation

[Evaluation](#benchmarks-and-evaluation): graph-statistic and kernel choices affect generator rankings.

- **[GraphRNN](https://proceedings.mlr.press/v80/you18a.html)** — You et al., 2018, ICML. Decomposes graph generation into autoregressive node and edge decisions conditioned on the graph built so far. Evaluates generated graph sets through distributions of structural statistics.
- **[GraphAF](https://arxiv.org/abs/2001.09382)** — Shi et al., 2020, ICLR. Uses an autoregressive normalizing flow to generate molecular graphs. Combines parallel likelihood training with sequential sampling that can check atom valence as atoms and bonds are added.
- **[DiGress](https://arxiv.org/abs/2209.14734)** — Vignac et al., 2022 preprint; ICLR 2023. Learns to reverse discrete noise on categorical node and edge attributes with a graph Transformer. Generates molecular and other graphs through successive denoising steps.
- **[DeFoG](https://arxiv.org/abs/2410.04263)** — Qin et al., 2024 preprint; ICML 2025. Applies discrete flow matching to graph generation. Separates the training objective from sampling choices, allowing generation procedures to be changed without retraining the predictor.

## Benchmarks and evaluation

- **[Evaluation pitfalls](https://arxiv.org/abs/1811.05868)** — Shchur et al., 2018 preprint. Shows that data splits, hyperparameter tuning, and early stopping can change GNN rankings. Compares architectures under shared protocols rather than inherited results on a single split.
- **[Open Graph Benchmark](https://arxiv.org/abs/2005.00687)** — Hu et al., 2020, NeurIPS. Provides node, link, and graph prediction tasks with application-specific splits and metrics. Includes large graphs and distribution shifts beyond small citation-network benchmarks.
- **[HeaRT](https://arxiv.org/html/2306.10453v3)** — Li et al., 2023. Evaluates each link against 500 endpoint-specific negatives, chosen by heuristics with random fill when needed. Rankings change and simple baselines become more competitive across seven homogeneous-graph datasets.
- **[HeaRT protocol](https://arxiv.org/html/2306.10453v3)** — Hard negatives change validation/test, not training. ogbl-ppa uses 100,000 positives per split; tuning ranges differ between small and OGB graphs. This is neither full-catalog retrieval nor the official OGB evaluation protocol.
- **[Long Range Graph Benchmark](https://arxiv.org/abs/2206.08164)** — Dwivedi et al., 2022, NeurIPS. Introduces five tasks intended to test long-range graph interactions across molecular and image-derived graphs, with message-passing and Transformer baselines.
- **[Reassessing LRGB](https://arxiv.org/html/2309.00367v2)** — Tönshoff et al., 2023. With roughly 500k parameters, tuned GCN/GINE/GatedGCN beat retuned GPS on both peptide tasks; a deeper prediction head explains much of the gain. GPS still leads these baselines on both superpixel tasks.
- **[LRGB protocol](https://arxiv.org/html/2309.00367v2)** — Input normalization improves the compared vision results. The 2023 audit found raw rather than specified filtered MRR in PCQM-Contact code. Filtering other positive tails and self-loops changes scores.
- **[Temporal Graph Benchmark](https://arxiv.org/html/2307.01026v2)** — Huang et al., 2023, NeurIPS. Moving averages beat TGN and DyRep on all four node-affinity tasks by test NDCG@10. Persistence is best on trade; moving averages lead the other three tasks.
- **[TGB link results](https://arxiv.org/html/2307.01026v2)** — TGN has the highest mean test MRR on coin, comment and flight. CAWN, TCL and GraphMixer ran out of GPU memory. On coin, TGN scores 0.586 ± 0.037 versus EdgeBank’s 0.580; the mean ranking alone does not establish significance.
- **[Classic GNNs as strong baselines](https://proceedings.neurips.cc/paper_files/paper/2024/hash/b10ed15ff1aa864f1be3a75f1ffc021b-Abstract-Datasets_and_Benchmarks_Track.html)** — Luo et al., 2024, NeurIPS. Reassesses GCN, GAT, and GraphSAGE against graph Transformers; tuning normalization, dropout, residual connections, and depth changes the comparison on studied node-classification datasets.
- **[Graph-generation evaluation](https://arxiv.org/html/2106.01098v3)** — O’Bray et al., 2022, ICLR. MMD rankings can reverse with descriptor, kernel, bandwidth or histogram choices. Matching degree or clustering summaries does not establish matching graph distributions; record metric settings.

### Comparisons to run

- **Graph contribution** — Proposed: compare a feature-only MLP, SGC, GCN, and GraphSAGE on identical splits with matched tuning budgets. Specify which nodes, edges, and labels are visible during training. [SGC](https://proceedings.mlr.press/v97/wu19e.html), [evaluation pitfalls](https://arxiv.org/abs/1811.05868).
- **Depth and global attention** — Proposed: compare local message passing and GraphGPS on LRGB with shared encodings and tuning budgets; vary depth and record quality, memory, and runtime. [GraphGPS](https://arxiv.org/abs/2205.12454), [LRGB reassessment](https://arxiv.org/abs/2309.00367).
- **Temporal prediction** — Proposed: compare a simple baseline, TGAT, and TGN using chronological splits; record memory-update timing, negative sampling, and access to past edges. Keep future events out of prediction inputs. [TGB](https://proceedings.neurips.cc/paper_files/paper/2023/hash/066b98e63313162f6562b35962671288-Abstract.html).

## Software

- **[PyTorch Geometric](https://pytorch-geometric.readthedocs.io/en/latest/)** — Graph data containers, message-passing layers, pooling operators, sampling, and dataset utilities for PyTorch.
- **[DGL](https://www.dgl.ai/dgl_docs/)** — Graph computation and message-passing APIs, heterogeneous graph support, sampling, and distributed graph-learning tools.
- **[OGB tools](https://ogb.stanford.edu/docs/home/)** — Dataset loaders, prescribed splits, evaluators, and baseline implementations. Use each task's specified metric and candidate-set protocol when comparing link predictors.

## Related Topics

- **[Geometric deep learning](https://mlpapers.org/geometric-deep-learning/)** — Symmetry, geometric representations, equivariant architectures and atomistic models.
- **[Neural networks](https://mlpapers.org/neural-nets/)** — Architectures, optimization, and training methods.
- **[Bayesian networks](https://mlpapers.org/bayesian-nets/)** — Probabilistic graphical models and conditional independence.
