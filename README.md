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