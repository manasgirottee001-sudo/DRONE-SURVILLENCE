# Drone DAA & Safe Zones - Simulation Project

**Author: Manas** | B.Tech CSE (AI & ML), Manav Rachna International Institute of Research and Studies (MRIIRS), Faridabad

A Python simulation of a drone that detects nearby obstacles and avoids restricted airspace (safe zones / no-fly zones) using a simple, explainable Detect-and-Avoid (DAA) algorithm.

> **Simulation only.** This project contains no code to communicate with real drones, aircraft radios, MAVLink, or any flight-control hardware. Do not deploy on real aircraft.

![Simulation result](docs/simulation_result.png)

## Features
- Drone model with position, speed and heading, stepped through time
- Circular obstacles with configurable radius
- Safe zones loaded from GeoJSON polygons (Shapely)
- Explainable DAA decision function: conflict detection, safe-zone check, avoidance suggestion
- Matplotlib visualisation of zones, obstacles and the flight path
- Unit tests for the core DAA logic

## How it works
At every time step the simulator asks the DAA function for a decision:

| Situation | Reason | Action |
|---|---|---|
| Drone is inside a safe zone | `in_safe_zone` | Turn 180 degrees, speed x 0.5 |
| Obstacle closer than min separation (5 units + obstacle radius) | `conflict` | Turn away from the obstacle (+10 degree conservative bias), speed x 0.8 |
| Otherwise | `clear` | No change |

The simulator applies the action, moves the drone, and records the trajectory for plotting.

## Project structure
```
.
├── src/
│   ├── sim.py          # Drone, DynamicObstacle, GeoJSON loader, scenario runner
│   ├── daa.py          # Conflict detection, safe-zone check, manoeuvre suggestion
│   └── visualize.py    # Matplotlib plotting
├── examples/run_sim.py # Runnable demo scenario
├── data/safe_zones.geojson
├── tests/test_daa.py   # Unit tests
├── docs/simulation_result.png
├── requirements.txt
└── LICENSE
```

## Quickstart
```bash
# 1. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Linux/Mac
venv\Scripts\activate           # Windows

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the example (opens a plot window and saves docs/simulation_result.png)
python examples/run_sim.py

# Run without opening a window
python examples/run_sim.py --no-show

# 4. Run the tests
python -m pytest tests
```

## Limitations and future work
- 2D only; no altitude, wind or physics
- Obstacles are static in the demo (no velocity model yet)
- Only the nearest obstacle is considered
- Purely reactive: no prediction of future collisions or goal-seeking after avoidance

## Tech stack
Python, Shapely, Matplotlib, GeoJSON, unittest/pytest

## License
MIT License. Based on the open-source template at https://github.com/Draguki/dronesurvailance.
