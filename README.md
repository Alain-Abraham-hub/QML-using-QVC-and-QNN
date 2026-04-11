# QML using QVC and QNN

Quantum Machine Learning using a Quantum Variational Circuit and a Quantum Neural Network.

## Project Overview

This project is a notebook-based quantum machine learning demo that classifies a synthetic 4x2 pixel dataset into two classes:

- `-1` for images with a horizontal line
- `+1` for images with a vertical line

The notebook walks through the full workflow:

1. Generate a synthetic dataset with NumPy.
2. Split the data into training and test sets.
3. Visualize sample images with Matplotlib.
4. Build a Qiskit feature map to encode the data.
5. Build a parameterized QNN ansatz.
6. Combine the feature map and ansatz into one circuit.
7. Define an observable for parity measurement.
8. Implement the forward pass and MSE loss.
9. Train the model with SciPy COBYLA.
10. Run the circuit on an IBM Quantum backend and evaluate accuracy.

## Requirements

- Python 3.10 or newer
- An IBM Quantum account if you want to run the backend step on real hardware

Install the dependencies with:

```bash
pip install -r requirements.txt
```

## How To Use

1. Create and activate a virtual environment.
2. Install the project dependencies from `requirements.txt`.
3. Open `Training_QNN.ipynb` in Jupyter or VS Code.
4. Run the notebook cells from top to bottom.
5. When you reach the IBM Quantum backend section, replace the API token in the notebook with your own IBM Quantum credentials.
6. Continue running the optimization and evaluation cells.

For local experimentation, the notebook also uses Qiskit’s statevector estimator so you can debug and train without immediately relying on hardware.

## Notebook Flow

### 1. Dataset generation

The first section creates a small synthetic dataset of flattened 4x2 images. Each sample contains either a horizontal or vertical line with small random noise added to the remaining pixels.

### 2. Data split and visualization

The notebook splits the data into training and test subsets, then plots example samples so you can verify the class labels visually.

### 3. Quantum circuit design

The feature map encodes the classical data into quantum states, and the ansatz adds trainable rotation and entangling layers.

### 4. Training loop

The model is trained by minimizing mean squared error between the circuit expectation values and the target labels. The optimization uses `scipy.optimize.minimize` with COBYLA.

### 5. IBM Quantum execution

After training, the circuit is transpiled for the selected IBM backend and evaluated with `EstimatorV2`.

## Project Files

- `Training_QNN.ipynb`: main notebook containing the full workflow
- `README.md`: project overview and usage instructions
- `requirements.txt`: Python dependencies for the notebook
- `Result Images/`: saved output images and figures

## Notes

- Keep IBM Quantum credentials out of version control.
- If you only want to explore the simulation path, you can run the notebook up to the local estimator training section.
- The notebook is intended to be executed sequentially because later cells depend on variables defined earlier.

## Docker

The project can also be run in a container.

Build the image:

```bash
docker build -t qml-qvc-qnn .
```

Or start it with Docker Compose:

```bash
docker compose up --build
```

Before using Docker Compose, set `QISKIT_IBM_TOKEN` in your shell or in a local `.env` file.

Start Jupyter inside the container:

```bash
docker run --rm -it -p 8888:8888 \
	-e QISKIT_IBM_TOKEN=your_ibm_token_here \
	qml-qvc-qnn
```

Then open the Jupyter URL printed in the terminal and run `Training_QNN.ipynb` from the container filesystem.

If you prefer to work from the local folder, Docker Compose mounts the workspace into the container so notebook changes are saved on your machine.


