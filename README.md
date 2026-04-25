# Hack3D Preliminary Task

This repository is based on the Hack3D SIMP Topology Optimizer and extends the original codebase for the preliminary task in **Advanced Mechanical Design with Vibe Coding**.

## Overview

The project is a full-stack web application for 3D topology optimization.

- **Frontend:** React
- **Backend:** Flask + NumPy FEM/SIMP optimizer
- **Core goal:** allow users to define structural conditions, run topology optimization, visualize results, and export designs

This repository includes my implementation of the two required preliminary tasks.

---

## Completed Tasks

### Task 1: Directional Load Control

The system was extended to support additional load directions in both the frontend and backend.

Supported directions:

- +X
- -X
- +Y
- -Y
- +Z
- -Z

#### Changes made
- Updated the frontend UI in `frontend/src/App.js`
- Added multi-direction load buttons for X, Y, and Z directions
- Updated backend force mapping in `app.py`
- Added direction mapping logic so the selected load direction is translated into the correct FEM force axis and sign

#### Result
Users can now choose different loading directions directly from the web interface, and the backend correctly applies those forces during optimization.

---

### Task 2: STL Export for 3D Printing

The system was extended to export the optimized density field as an STL file for 3D printing.

#### Changes made
- Added STL export logic in `app.py`
- Added helper functions to convert thresholded density elements into voxel-based mesh geometry
- Added a new backend endpoint: `/export/stl`
- Added an `EXPORT STL` button in `frontend/src/App.js`

#### Result
After running topology optimization, the user can click `EXPORT STL` and download an `.stl` file such as:

- `optimized_design_threshold_0.5.stl`

The STL file can be opened in:

- Blender
- MeshLab
- Cura
- Windows 3D Viewer

---

## Project Structure

```text
hack3d-preliminary-task/
├── app.py
├── fem3d_numpy.py
├── simp_numpy.py
├── watermark.py
├── requirements.txt
├── optimized_design_threshold_0.5.stl
├── frontend/
│   ├── package.json
│   └── src/
│       └── App.js
└── README.md

## Important Files

- `app.py`  
  Flask backend, optimization streaming API, STL export API, watermark APIs

- `fem3d_numpy.py`  
  3D finite element solver

- `simp_numpy.py`  
  SIMP topology optimizer

- `frontend/src/App.js`  
  Main React frontend UI

---

## Features

### Topology Optimization
- 3D mesh resolution control
- volume fraction control
- SIMP penalty control
- iteration control
- fixed face selection
- load face selection
- load magnitude control
- density threshold visualization

### Directional Load Control
- +X / -X
- +Y / -Y
- +Z / -Z

### Visualization
- 3D optimized structure
- convergence history
- density distribution histogram
- live iteration feed

### Export
- image export
- STL export

### Watermark Lab
- watermark embedding
- watermark detection
- attack simulation

---

## Installation

### 1. Clone the repository

```bash id="9ji33o"
git clone https://github.com/zw4512-cloud/hack3d-preliminary-task.git
cd hack3d-preliminary-task

### 2. Backend setup

Install Python dependencies:

```bash id="t6uw2p"
python -m pip install -r requirements.txt
python -m pip install numpy-stl

### 3. Frontend setup

cd frontend
npm install

## Frontend Setup

```bash
cd frontend
npm install
```

## How to Run

### Start the Backend

From the project root:

```bash
python app.py
```

Backend runs at:

```text
http://127.0.0.1:5000
```

### Start the Frontend

From the `frontend` folder:

```bash
npm start
```

Frontend runs at:

```text
http://localhost:3000
```

## How to Use

### Run Topology Optimization

1. Open the frontend in the browser.
2. Choose a preset or customize parameters.
3. Select:
   - fixed face
   - load face
   - load direction
   - load magnitude
4. Click **RUN OPTIMIZATION**.
5. Wait for the result panels to appear.

### Export STL

1. Run optimization first.
2. Wait until the optimization result is displayed.
3. Click **EXPORT STL**.
4. Download the STL file.

## Notes on STL Export

The STL export is generated from the optimized density field.

- Elements with density above the selected threshold are kept.
- Each active element is converted into voxel-based mesh geometry.
- The generated voxel mesh is exported as a binary STL file.

This is a simple and practical approach for preliminary 3D printing export.

## Example Output Files

This repository includes or may include example outputs such as:

- optimization screenshots
- directional load test images
- STL example files

Example STL output:

```text
optimized_design_threshold_0.5.stl
```

## Technical Notes

### Directional Load Mapping

The frontend sends symbolic load directions such as:

- `x+`
- `x-`
- `y+`
- `y-`
- `z+`
- `z-`

The backend maps these directions to:

- force axis
- sign of total force

This allows the FEM solver to apply the correct distributed load.

### Why Some `+` / `-` Results May Look Similar

For a linear compliance-based topology optimization problem, changing only the sign of the load may still produce a similar optimized structure. This is expected in many symmetric linear settings and does not mean the directional load feature is incorrect.

## Submission Notes

This repository is public and contains:

- relevant code
- frontend and backend modifications
- STL export functionality
- instructions to run the project

Repository URL:

```text
https://github.com/zw4512-cloud/hack3d-preliminary-task
```

## Acknowledgment

This project is based on the Hack3D SIMP Topology Optimizer codebase and was extended for the preliminary task requirements.



---

## Final Task 1: Multiple Point Load Support

The system was extended to support **multiple point loads** inside the design domain.

### Implemented Features
- Users can add, remove, and modify multiple point loads in the frontend
- Each point load includes:
  - location `(x, y, z)` in the mesh
  - direction (`+X`, `-X`, `+Y`, `-Y`, `+Z`, `-Z`)
  - magnitude
- The backend reads the full `pointLoads` list from the request payload
- Each load is mapped to the nearest FEM node and applied to the global force vector
- The frontend displays all current loads in a dedicated **MULTIPLE POINT LOADS** panel

### Frontend Changes
In `frontend/src/App.js`:
- replaced the previous single-load parameter structure with a `pointLoads` array
- added UI controls to:
  - add a new load
  - remove an existing load
  - edit load position, direction, and magnitude
- updated presets to use the new multi-load format

### Backend Changes
In `app.py`:
- added `apply_point_load(...)` to apply a load to the nearest FEM node
- updated `build_fem(data)` to read `pointLoads`
- applied all loads in a loop when multiple point loads are provided
- preserved backward compatibility with the old single-load mode as a fallback

### Result
The optimization system now supports multiple point loads and can run successfully with:
- a single point load
- multiple point loads
- modified load positions, directions, and magnitudes

This completes the required functionality for **Final Task 1**.


---

---

## Final Task 2: Interactive 3D Input Visualization

An interactive 3D input preview was added to the frontend for pre-optimization setup.

### Implemented Features
- supports standard 3D interaction:
  - rotation
  - pan
  - zoom
- displays key input information:
  - design domain
  - mesh/grid structure
  - fixed boundary condition
  - multiple point loads
- updates in real time as the user changes:
  - mesh resolution
  - fixed face
  - point load positions
  - point load directions
  - point load magnitudes

### Frontend Changes
In `frontend/src/App.js`:
- added a 3D input preview using React-based 3D rendering
- visualized the design domain as a box
- visualized the mesh/grid on all faces
- highlighted the constrained face
- displayed all point loads as directional arrows
- enabled interactive camera controls for rotation, pan, and zoom

### Result
Users can now inspect the full optimization setup in an interactive 3D view before running the solver, making the input configuration more intuitive and easier to understand.

---

## Final Task 3: Interactive 3D Output Visualization

An interactive 3D output visualization was added for post-optimization results.

### Implemented Features
- supports standard 3D interaction:
  - rotation
  - pan
  - zoom
- displays the optimized structure directly in the frontend
- uses density-based voxel visualization
- shows only elements above the selected density threshold
- allows threshold switching for:
  - `ρ > 0.1`
  - `ρ > 0.3`
  - `ρ > 0.5`
- includes an outer domain wireframe to help distinguish the optimized structure from removed/void regions

### Frontend Changes
In `frontend/src/App.js`:
- added an interactive 3D output viewer
- converted returned density data into voxel cubes
- rendered cubes above the selected density threshold
- added threshold-based filtering and density-based visual distinction
- enabled orbit interaction for result inspection

### Result
Users can now inspect the optimized structure in an interactive 3D view instead of relying only on static images, which makes the output easier to analyze and interpret.