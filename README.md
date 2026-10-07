# Multimodal Route Planning with MOEA/D

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![NetworkX](https://img.shields.io/badge/NetworkX-Graph%20modelling-BD4F00)](https://networkx.org/)
[![NumPy](https://img.shields.io/badge/NumPy-Numerical%20computing-013243?logo=numpy&logoColor=white)](https://numpy.org/)
[![pandas](https://img.shields.io/badge/pandas-Data%20processing-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualisation-11557C)](https://matplotlib.org/)

<h3 align="center">University of Minho<br>Master's Degree in Artificial Intelligence<br>Nature-Inspired Computing<br>2024/2025</h3>

---

<h3 align="center">Contributors</h3>

<div align="center">

| Name | Number |
|---|---|
| Diogo José Borges Dias | PG60245 |
| Diogo Lopes Azevedo | PG61217 |

</div>

### Project summary

This project addresses multimodal route planning in the Greater Porto public transport
network. It builds a directed graph from Metro do Porto and STCP GTFS data, adds
walking connections between nearby stops, and uses the MOEA/D evolutionary algorithm
to approximate routes that balance total travel time against CO₂ emissions. The
pipeline also generates test scenarios, evaluates the optimisation results, and
provides static, interactive, and spatial visualisations of the network and its
Pareto front.

## Main features and technical architecture

- **GTFS ingestion and graph construction** (`src/graph.py`)
  - Loads Metro do Porto and STCP stops and stop times from `data/gtfs/`.
  - Builds a `networkx.MultiDiGraph` with Metro, bus, and walking edges.
  - Stores travel time, distance, transport mode, and estimated CO₂ emissions on
    each edge.
  - Connects nearby stops using Haversine distance and a walking-speed model.
- **Multi-objective optimisation** (`src/moead.py`, `src/main.py`)
  - Uses MOEA/D with weighted decomposition, neighbourhoods, Tchebycheff-style
    scalarisation, graph-aware crossover, and mutation.
  - Evaluates valid routes according to travel time and CO₂ emissions.
  - Applies constraints for transfers and walking time.
  - Produces an approximate Pareto front and representative extreme solutions.
- **Scenario generation and evaluation** (`src/scenarios.py`,
  `src/evaluate_scenarios.py`)
  - Generates nine scenarios across easy, medium, and difficult distance ranges.
  - Reports Pareto-front size, travel-time statistics, CO₂ statistics, runtime,
    and generation counts.
- **Result visualisation**
  - `src/visualize.py` generates analysis figures in `figures/`.
  - `src/interactive_pareto.py` provides a Matplotlib-based explorer for
    selecting solutions and inspecting route details.
  - `src/export_graph_html.py` exports the multimodal graph to an interactive
    Leaflet HTML map at `output/graph.html`.

## Project structure

```text
.
├── data/
│   └── gtfs/
│       ├── mdp/              # Metro do Porto GTFS data
│       └── stcp/             # STCP GTFS data
├── figures/                  # Figures generated for the report
├── output/                   # Generated graphs, results, scenarios, and HTML
├── src/
│   ├── graph.py             # Multimodal graph construction
│   ├── moead.py             # MOEA/D implementation and route evaluation
│   ├── export_graph_html.py # Interactive graph export
│   ├── main.py              # Main optimisation pipeline
│   ├── scenarios.py         # Scenario generation
│   ├── evaluate_scenarios.py# Scenario evaluation
│   ├── interactive_pareto.py# Interactive Pareto-front explorer
│   └── visualize.py         # Result visualisation
└── README.md
```

## Installation and usage

### Requirements

- Python 3.10 or later (the project was developed with Python 3.10.19).
- The GTFS files available under `data/gtfs/mdp/` and `data/gtfs/stcp/`.

### Install

From the repository root, create and activate a virtual environment and install
the dependencies used by the source files:

```bash
python -m venv .venv
```

On macOS/Linux:

```bash
source .venv/bin/activate
```

On Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

```bash
python -m pip install --upgrade pip
python -m pip install networkx numpy pandas matplotlib
```

### Build the multimodal graph

Run this command from the repository root:

```bash
python src/graph.py
```

This reads the GTFS data and writes the base graph to
`output/graph_base.gpickle`.

### Run route optimisation

```bash
python src/main.py
```

The script prompts for the origin and destination coordinates in
`latitude,longitude` format. It writes:

- `output/moead_results.pkl` — complete optimisation results;
- `output/pareto_front.csv` — the approximate Pareto front.

The graph must be built before running this command.

### Generate scenarios and evaluate the algorithm

Generate the scenario set:

```bash
python src/scenarios.py
```

This creates `output/scenarios.pkl` and `output/scenarios.csv`. Then evaluate all
generated scenarios:

```bash
python src/evaluate_scenarios.py
```

The evaluation results are written to `output/evaluation_results.pkl` and
`output/evaluation_results.csv`.

### Generate visualisations

After running the optimisation pipeline:

```bash
python src/visualize.py
```

The generated plots are saved under `figures/`.

To inspect the Pareto front interactively:

```bash
python src/interactive_pareto.py
```

To export the graph as an interactive map:

```bash
python src/export_graph_html.py
```

Open `output/graph.html` in a browser. The map loads Leaflet and OpenStreetMap
tiles from their public CDNs, so an internet connection is required when viewing
the exported map.

## Academic context and authorship

This repository was developed collaboratively by **Diogo José Borges Dias
(PG60245)** and **Diogo Lopes Azevedo (PG61217)** as part of the *Nature-Inspired
Computing* course in the Master's Degree in Artificial Intelligence at the
University of Minho.

The work applies multi-objective optimisation, Pareto dominance, evolutionary
algorithms, MOEA/D, graph modelling, and multimodal route planning to a
real-world public transport network.
