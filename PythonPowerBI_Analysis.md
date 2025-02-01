# Integrating RDF Databases with Power BI for Enhanced Data Insights

## Project Overview
This project is a **consultancy project** aimed at providing solutions to **client companies targeting medium-sized businesses**. It focuses on integrating RDF databases with Power BI to enhance data insights. By leveraging **GraphDB** as the preferred RDF database, the team addressed data silos and improved analytics capabilities, offering a scalable solution tailored to the client's needs.

## Implementation Details
- **Data Transformation:** Used SPARQL queries to extract and convert RDF data into a **star schema** for better integration with Power BI.
- **Power BI Integration:** Exported data as CSV files for seamless loading into Power BI.
  
## My Contribution
This was a **group project**, and my primary contribution was in **developing Python scripts** that automated:
- **Data extraction** from GraphDB using SPARQL queries.
- **Data transformation and preprocessing** to optimize Power BI integration.
- **Custom visualizations** using NetworkX and Pandas for exploratory analysis.

## Power BI Dashboard & Limitations
The Power BI dashboard in this project was developed using **Python's NetworkX and Matplotlib libraries** for advanced graph visualizations. However, since these libraries are **not supported in Power BI Service (online version)**, the dashboard **cannot be published as a public Power BI link**.

## Key Takeaways
The project demonstrated how **knowledge graphs** can enhance **BI reporting**, offering deeper insights and encouraging businesses to adopt **RDF-based analytics solutions**.

---
📌 **Technologies Used:** Python, SPARQL, GraphDB, Power BI, NetworkX, ZoomCharts, Matplotlib


### Examples of Python Code:
[▶️Review the sample video(click)](https://youtu.be/0tWV6iNT5uw)


### Example code 1.
![Top 5 Actor Movie COnnections and Comment Impact](https://github.com/user-attachments/assets/f2b68582-2d88-470f-9cb5-526de62531cf)
Figure 1. Screenshot of the 'Top 5 Actor Movie COnnections and Comment Impact' dashboard

---

### `script.py`
```python
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Load and preprocess data
df_actor = dataset[['actor_name', 'movie_title', 'country_name', 'commentCount']].copy()
df_actor['TotalComments'] = df_actor.groupby('movie_title')['commentCount'].transform('sum')

# Select top 5 actors
top_actors_per_country = (
    df_actor.groupby(['country_name', 'actor_name'])['commentCount']
    .sum()
    .reset_index()
    .groupby('country_name')
    .apply(lambda x: x.nlargest(5, 'commentCount'))
    .reset_index(drop=True)
)

# Create network graph
G = nx.Graph()
for _, row in df_filtered.iterrows():
    G.add_node(row['actor_name'], type='actor')
    G.add_node(row['movie_title'], type='movie', weight=row['TotalComments'])
    G.add_node(row['country_name'], type='country')
    G.add_edge(row['actor_name'], row['movie_title'], weight=row['commentCount'])
    G.add_edge(row['movie_title'], row['country_name'], weight=row['TotalComments'])

# Visualize graph
plt.figure(figsize=(30, 18))
nx.draw_networkx(G, pos, node_size=5000, node_color=node_colors, font_size=10, font_weight='bold')
plt.title("Actor-Movie-Country Network Graph")
plt.show()
```

### Example code 2.
![Top Actors by Movie Participation: Collaboration Network](https://github.com/user-attachments/assets/d2b683f5-5596-4111-b3b0-74b84ef7d548)
Figure 2. Screenshot of the 'Top Actors by Movie Participation: Collaboration Network' dashboard

### `script.py`
```python
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Process actor collaborations
df_actors = dataset[['actor_name', 'movie_title', 'role_type', 'country_name', 'genre_name']]
collaboration_pairs = df_actors.groupby('movie_title')['actor_name'].apply(lambda x: pd.Series(x).value_counts().index.tolist()).reset_index()
collaboration_pairs = pd.DataFrame([(a, b) for actors in collaboration_pairs['actor_name'] for a in actors for b in actors if a < b],
                                   columns=['actor_name', 'collaborator'])

# Create network graph
G = nx.Graph()
for _, row in actor_movie_count.iterrows():
    G.add_node(row['actor_name'], weight=row['movieCount'], role_type=role_type, node_type='actor')

for _, row in collaboration_count.iterrows():
    G.add_edge(row['actor_name'], row['collaborator'], weight=row['collaborationCount'], edge_type='collaboration')

# Add country and genre connections
for _, row in df_actors.iterrows():
    G.add_edge(row['actor_name'], row['country_name'], edge_type='actor-country_name')
    G.add_edge(row['actor_name'], row['genre_name'], edge_type='actor-genre_name')

# Visualize graph
plt.figure(figsize=(30, 18))
nx.draw_networkx_nodes(G, pos, node_size=node_sizes, node_color=colors)
nx.draw_networkx_edges(G, pos, width=edge_widths, edge_color='black')
nx.draw_networkx_labels(G, pos, labels=labels, font_size=23)
plt.show()
```
