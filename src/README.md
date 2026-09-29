**Simulation-Based Inference (SBI) Geological Toy Prototype**

Runnable prototype for Poster 2 (Application).

This project demonstrates Simulation-Based Inference (SBI) for a simplified geological problem: infer channel sinuosity from a synthetic 2D facies image using Neural Posterior Score Estimation (NPSE). The Python channel simulator is a proof-of-concept toy simulator. It is not a reproduction of RMS.

_Workflow_: 

1. Sample sinuosity S ~ Uniform(1.7,1.9).
2. Generate a 64 x 256 binary channel image.
3. Generate 1,000 (sinuosity, channel image) pairs.
4. Split them into 800 training and 200 held out samples.
5. Convert each image into a 256-dimensional channel-centroid summary.
6. Train NPSE on the 800 training pairs.
7. Infer posterior distributions for held-out channels.
8. Evaluate parameter recovery and posterior calibration.

_Files:_

- simulator.py: generates 1000 simulation pairs
- train.py: prepares the train/test split and trains NPSE
- inference.py: performs inference for one held-out channel
- evaluate.py: performs inference for all 200 held-out channels and saves posterior samples
- plot.ipynb: calculates MAE, RMSE, credible-interval coverage, and calibration.
- requirements.txt — required Python packages.

_Setup:_

  - Python 3.10 or 3.11 is recommended
  - Create a virtual environment: python -m venv .venv

_Install dependencies:_

- python -m pip install --upgrade pip
- pip install -r requirements.txt

_Run: Run the scripts from the project directory in this order._

1. Generate simulations: _python simulator.py_
Expected output shapes: _Sinuosity: (1000,) ; Images: (1000, 64, 256)_

2. Train NPSE: _python train.py_
The model is trained using 800 simulations. The remaining 200 simulations are held out for evaluation.

3. Inference for one held-out channel: _python inference.py_
This generates posterior samples of sinuosity conditioned on one unseen channel.

4. Evaluate all held-out channels: _python evaluate.py_
This performs inference for all 200 held-out channels and saves 200 posterior samples per channel.
Expected posterior-sample shape:(200, 200)

5. Analyze evaluation: _python evaluation.py_
This calculates parameter-recovery accuracy and posterior calibration without rerunning NPSE inference.

_Current Results_: From the current experiment:

_MAE  = 0.00738
RMSE = 0.00894_

90% credible interval coverage = 96.5%
193 / 200 true sinuosity values inside the 90% intervals

Calibration:
Expected: 10% | Observed: 7.0%
Expected: 20% | Observed: 19.0%
Expected: 30% | Observed: 29.0%
Expected: 40% | Observed: 45.5%
Expected: 50% | Observed: 59.0%
Expected: 60% | Observed: 70.5%
Expected: 70% | Observed: 82.5%
Expected: 80% | Observed: 90.5%
Expected: 90% | Observed: 96.5%

The posterior is somewhat conservative at higher credible levels.

_Limitations_

- The simulator is intentionally simplified and is not RMS.
- Only one parameter, sinuosity, is inferred.
- Images are compressed using a hand-designed channel-centroid summary.
- The sinuosity range [1.7, 1.9] and the simulator relationship are part of this toy proof of concept.

**The experiment demonstrates the SBI workflow rather than validation on realistic geological models.**
