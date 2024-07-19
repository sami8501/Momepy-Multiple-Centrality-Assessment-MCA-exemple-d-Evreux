# Momepy - Analyse des réseaux de voiries urbaines

Momepy est une bibliothèque Python dédiée à l'analyse des réseaux urbains, permettant d'évaluer la structure et la fonctionnalité des réseaux de rues. Ce README se concentre sur l'utilisation de Momepy pour réaliser une évaluation de la centralité multiple (Multiple Centrality Assessment - MCA) dans la ville d'Evreux, France.

## Installation

```bash
pip install momepy osmnx geopandas matplotlib
```

## Fonctionnalités principales

### 1. Importation des bibliothèques nécessaires

```python
import geopandas as gpd
import momepy
import osmnx as ox
import matplotlib.pyplot as plt
```

### 2. Récupération du réseau routier

```python
streets_graph = ox.graph_from_place('Evreux, France', network_type='walk')
streets_graph = ox.projection.project_graph(streets_graph)
```

### 3. Conversion en GeoDataFrame

```python
edges = ox.graph_to_gdfs(ox.get_undirected(streets_graph), nodes=False, edges=True,
                          node_geometry=False, fill_edge_geometry=True)
```

### 4. Visualisation du réseau

```python
f, ax = plt.subplots(figsize=(20, 20))
edges.plot(ax=ax, linewidth=0.2)
ax.set_axis_off()
plt.show()
```

### 5. Évaluation de la centralité

#### Closeness Centrality

- **Local Closeness** :

```python
primal = momepy.closeness_centrality(primal, radius=400, name='closeness400', distance='mm_len', weight='mm_len')
```

- **Global Closeness** :

```python
primal = momepy.closeness_centrality(primal, name='closeness_global', weight='mm_len')
```

#### Betweenness Centrality

- **Node-based** :

```python
primal = momepy.betweenness_centrality(primal, name='betweenness_metric_n', mode='nodes', weight='mm_len')
```

- **Edge-based** :

```python
primal = momepy.betweenness_centrality(primal, name='betweenness_metric_e', mode='edges', weight='mm_len')
```

#### Straightness Centrality

```python
primal = momepy.straightness_centrality(primal)
```

### 6. Visualisation des résultats de centralité

Pour chaque type de centralité, les résultats peuvent être visualisés sur une carte :

```python
nodes = momepy.nx_to_gdf(primal, lines=False)
f, ax = plt.subplots(figsize=(20, 20))
nodes.plot(ax=ax, column='closeness400', cmap='Spectral_r', scheme='quantiles', k=15, alpha=0.6)
ax.set_axis_off()
ax.set_title('closeness400')
plt.show()
```

### 7. Continuité dans les réseaux de rues

Momepy permet également d'évaluer la continuité des réseaux de rues à l'aide de l'algorithme COINS :

```python
continuity = momepy.COINS(streets)
stroke_gdf = continuity.stroke_gdf()
```

### 8. Visualisation de la continuité

```python
stroke_gdf.plot(stroke_gdf.length,
                figsize=(25, 25),
                cmap="viridis_r",
                linewidth=.5,
                legend=True,
                scheme="headtailbreaks"
               ).set_title("Continuité dans les réseaux de rues piétonnes de Montpellier", fontsize=15)
```

## Exemples d'utilisation

Le notebook fournit des exemples d'analyse de la centralité multiple pour la ville d'Evreux, illustrant :
- La récupération des données de réseau routier
- L'évaluation de différentes centralités (closeness, betweenness, straightness)
- La visualisation des résultats sur des cartes
- L'analyse de la continuité des réseaux de rues

## Licence

Momepy est distribué sous licence BSD 3-Clause.

## Contact

Pour plus d'informations, visitez la [documentation officielle de Momepy](http://docs.momepy.org/en/stable/references.html).
