# Sub6tommWave

MATLAB code for predicting mmWave beam selections from sub-6 GHz channel measurements.

This repository contains a deep learning experiment pipeline for using lower-frequency channel state information as input and predicting the best mmWave beam index. It is based on the sub-6 GHz to mmWave beam-selection workflow and includes scripts for data preparation, beam-codebook generation, model construction, training, testing, and rate/accuracy evaluation.

## Project Overview

The main experiment trains a neural network to classify the best mmWave beam from transformed sub-6 GHz channel data. The pipeline:

1. Loads sub-6 GHz and mmWave training data from `.mat` files.
2. Converts channel state information into an angle-delay profile representation.
3. Builds a beamforming codebook.
4. Trains a convolutional neural network classifier.
5. Tests the trained network across multiple SNR and transmit-power settings.
6. Reports top-1, top-3, and top-5 beam accuracy and achievable-rate metrics.

## Repository Structure

| File | Purpose |
| --- | --- |
| `main.m` | Main training and evaluation script |
| `dataPrep_train.m` | Prepares normalized training and validation data |
| `dataPrep_test.m` | Prepares test data and labels for evaluation |
| `buildNetconv4.m` | Defines the neural network architecture |
| `CSI2ADP_theta_N.m` | Converts CSI into an angle-delay profile representation |
| `UPA_codebook_generator.m` | Generates the RF beamforming codebook |

## Expected Data Layout

The scripts expect training and test `.mat` files under a `Data/` folder:

```text
Data/
  train/
    sub6Train_org_4_64.mat
    mmTrain_org_64_64.mat
  test/
    sub6test_LOSB_4_64.mat
    mmtest_LOSB_4_64.mat
```

Each `.mat` file should include a `channel` variable. NLOS experiments may also require `labels`.

## Requirements

- MATLAB
- Deep Learning Toolbox
- Communications Toolbox, for `awgn`
- Compatible sub-6 GHz and mmWave channel datasets

GPU execution can be configured through the options in `main.m`.

## Running the Experiment

1. Add the expected `.mat` files under `Data/train/` and `Data/test/`.
2. Open MATLAB from the repository root.
3. Review the experiment settings in `main.m`, especially:
   - `options.type`
   - `options.case`
   - `options.dataFile1`
   - `options.dataFile2`
   - `options.testFile1`
   - `options.testFile2`
   - `options.SNR`
4. Run:

```matlab
main
```

Results are saved using the experiment tag configured in `options.expTag`.

## Notes

The code is research-oriented and assumes the dataset has already been generated. If you adapt it to a new channel dataset, start by checking the dimensions expected in `main.m`, `dataPrep_train.m`, and `dataPrep_test.m`.
