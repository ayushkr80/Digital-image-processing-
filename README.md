# Scanner Source Identification Using Sensor Pattern Noise

## What this project does

This project identifies the source scanner of a scanned TIFF image by extracting **Sensor Pattern Noise (SPN)** and training a classifier on noise-based features. The notebook workflow loads the dataset, extracts **non-overlapping 1024×768 patches**, denoises them, computes residual noise, builds **16-dimensional features**, and trains an **SVM** model for scanner classification.

## Main reference flow in the notebook

The notebook follows this sequence:

1. Install and import required libraries.
2. Enable large TIFF handling with `Image.MAX_IMAGE_PIXELS = None`.
3. Copy the dataset from `../datasets` into the working folder.
4. Count TIFF files and verify scanner labels.
5. Display a sample TIFF image.
6. Extract patches.
7. Denoise patches and compute residual noise.
8. Build row-pattern features.
9. Extract the final 16 features.
10. Train and evaluate the classifier.
11. Print accuracy, classification report, and confusion matrix.

## Dataset

The notebook reference uses:

- **4 scanner classes**
- **160 images at 1200 DPI**
- **24,270 total patches**
- **Scanner folders** such as:
  - `s1_epson_4490`
  - `s2_hp_scanjet_6300c_1_SG9CO270W5`
  - `s3_hp_scanjet_6300c_2`
  - `s4_hp_scanjet_8250`

The notebook also skips the `200` DPI folder and ignores the excluded scanner folders. The full dataset traversal and label verification are part of the notebook’s preprocessing stage. fileciteturn5file0

## Requirements

Install the Python packages used by the notebook:

```bash
pip install opencv-python numpy scipy matplotlib scikit-learn pywavelets pandas pillow notebook
```

## How to run the code

### 1) Put the dataset in the right place

The notebook expects a folder named `datasets` one level above the notebook directory, because it copies from:

```python
src = os.path.join('..', 'datasets')
```

So the usual structure should look like this:

```text
project/
├── notebook.ipynb
└── datasets/
    ├── scanner_dataset/
    └── scanner_dataset_students/
```

If your folder names are different, update the dataset path in the notebook before running it.

### 2) Open Jupyter Notebook

From the project folder, run:

```bash
jupyter notebook
```

Then open the scanner identification notebook.

### 3) Run the cells in order

Run the notebook from top to bottom without skipping cells. The early cells:

- install `opencv-python`
- import `PIL.Image`
- set `Image.MAX_IMAGE_PIXELS = None`
- test TIFF loading
- copy the dataset into the working directory
- print the TIFF file count
- display a sample image

These steps are important because the notebook reads large TIFF files and expects the dataset copied into the current working folder before patch extraction begins. The reference notebook reports **1,583 TIFF files** in the complete dataset and then narrows the experiment to **160 images**, producing **24,270 patches**. fileciteturn5file0

### 4) Let the notebook finish the preprocessing loop

The main processing loop extracts patches, denoises each patch, and stores the noise residuals. It prints progress and eventually reports:

- total processed patches
- total noise samples
- total labels

The reference notebook shows **24,270 processed patches** and **24,270 noise samples** at the end of preprocessing. fileciteturn5file0

### 5) Run feature extraction and classification

After preprocessing, the notebook:

- computes the row pattern
- extracts the 16 features
- standardizes the features
- splits the data with group-aware splitting
- trains an SVM
- prints the final accuracy and confusion matrix

The reference notebook’s final model uses a group-aware workflow and reports a test accuracy of **88.44%**. fileciteturn5file0

## Important runtime notes

- Keep `Image.MAX_IMAGE_PIXELS = None` enabled, otherwise large TIFFs may fail to open.
- Do not skip the dataset copy step if the notebook uses relative paths.
- If the notebook says no images were found, verify that the `scanner_dataset` folder is present in the working directory.
- If you rename any PNG files used for plots, update the notebook and report paths exactly.

## Expected outputs

When the notebook runs successfully, it should generate:

- sample TIFF visualization
- patch / denoised / noise preview
- row noise pattern plot
- 16-feature output
- accuracy report
- confusion matrix

## Notes for the report and presentation

The project report and presentation should stay consistent with the notebook results:

- **24,270 patches**
- **16-dimensional feature vector**
- **SVM classifier**
- **88.44% accuracy**

## Troubleshooting

### TIFF image not opening
Check that:

- the file is a valid TIFF
- `Image.MAX_IMAGE_PIXELS = None` is set
- the path in the notebook matches the actual file path

### Dataset not found
Confirm that:

- `../datasets` exists relative to the notebook
- `scanner_dataset` is inside that folder
- the notebook has copied the files into the current working directory

### Patch count is too low
This usually means:

- the wrong DPI folder was used
- images are smaller than the patch size
- some scanner folders were skipped incorrectly

## Final result

The notebook-based scanner identification pipeline extracts scanner-specific noise fingerprints and classifies them with an SVM. The reference workflow is designed around large TIFF inputs, patch-based noise extraction, and group-aware evaluation. fileciteturn5file0
