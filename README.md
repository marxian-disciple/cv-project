# Computer Vision Project

This repository contains the notebooks and scripts used for the computer vision project. The project uses the [HD-EPIC dataset](https://github.com/hd-epic/hd-epic-annotations) to extract video features, construct a dataset for future object-relevance prediction, train a temporal Transformer, and evaluate its performance.

## 1. Clone the Repository

Open a terminal in your preferred directory and run:

```bash
git clone https://github.com/marxian-disciple/cv-project.git
```

This will create a directory called `cv-project` inside your current directory.

Enter the repository:

```bash
cd cv-project
```

The repository contains the following files:

- `hd-epic-downloader.py` — script for downloading the HD-EPIC dataset.
- `Features.ipynb` — extracts feature vectors from video clips.
- `Dataset.ipynb` — checks and constructs the dataset.
- `Annotations.ipynb` — optional notebook for exploring and analysing the annotations.
- `CV.ipynb` — main notebook containing the model, training procedure, and evaluation.

## 2. Download the HD-EPIC Dataset

The project uses the HD-EPIC dataset. The downloader script creates a directory named `HD-EPIC` under the output path you specify.

From the `cv-project` directory, run:

```bash
python hd-epic-downloader.py .
```

The `.` specifies the current directory as the output path. The script will create an `HD-EPIC` directory inside `cv-project`.

Next, enter the downloaded dataset directory:

```bash
cd HD-EPIC
```

Download the videos for participants 7, 8, and 9:

```bash
python hd-epic-downloader.py . --videos --participants 7,8,9
```

This selects the video data for participants P07, P08, and P09.

For information about the downloader's available options, run:

```bash
python hd-epic-downloader.py -h
```

**Note:** The downloader's default behaviour is to download everything. The `--videos` and `--participants` arguments restrict the requested data to videos from the specified participants. The initial default download may therefore be unnecessary if you only require videos from these participants. If that is your intention, you can run the second command directly from the `cv-project` directory:

```bash
python hd-epic-downloader.py . --videos --participants 7,8,9
```

In that case, the downloader will create the `HD-EPIC` directory and download the selected videos there.

## 3. Download the Annotations

The HD-EPIC annotations are available in a separate repository:

https://github.com/hd-epic/hd-epic-annotations.git

Clone the annotations repository:

```bash
git clone https://github.com/hd-epic/hd-epic-annotations.git
```

This creates a directory named `hd-epic-annotations` in your current directory.

Create an `Annotations` directory inside the `HD-EPIC` dataset directory:

```bash
mkdir -p HD-EPIC/Annotations
```

Copy the contents of the cloned annotations repository into the new directory:

```bash
cp -r hd-epic-annotations/. HD-EPIC/Annotations/
```

If you followed the alternative download command above, your directory structure should now resemble:

```text
cv-project/
├── README.md
├── hd-epic-downloader.py
├── Features.ipynb
├── Dataset.ipynb
├── Annotations.ipynb
├── CV.ipynb
├── HD-EPIC/
│   ├── Videos/
│   └── Annotations/
│       └── ...
└── hd-epic-annotations/
    └── ...
```

The exact contents of the dataset and annotation directories depend on the downloader and annotation repository. The `hd-epic-annotations` directory is a separate clone and is not required after its contents have been copied, provided you do not need to update the annotations later.

## 4. Run the Notebooks

Run the notebooks in the following order.

### 4.1. `Features.ipynb` — Feature Extraction

This notebook extracts feature vectors from video clips using a pretrained VideoMAE model.

The resulting feature vectors provide visual representations of the video that can be used by the temporal Transformer. The notebook saves the extracted features for use in subsequent stages of the pipeline.

### 4.2. `Dataset.ipynb` — Dataset Construction and Validation

This notebook constructs and checks the dataset used for training and evaluation.

It brings together the extracted features and the relevant annotations, generates the necessary labels, and prepares the data for the model. It also includes checks intended to ensure that dataset construction has worked correctly.

Complete this notebook before proceeding to model training.

### 4.3. `Annotations.ipynb` — Annotation Analysis (Optional)

This notebook explores and analyses the HD-EPIC annotations.

It is optional and is intended to help you understand the annotations and the information they provide about objects and their temporal occurrences in the videos.

You can run it before or after the dataset construction notebook, depending on whether you want to explore the annotations before preparing the dataset.

### 4.4. `CV.ipynb` — Model Training and Evaluation

This is the main notebook for the computer vision project.

It contains the temporal Transformer model, the training procedure, and the evaluation code. The model uses the extracted visual features and constructed dataset to predict which objects will become relevant in the future and estimate when they will become relevant.

Run this notebook after the required features and dataset have been generated.

## 5. Important Notes

- **Dataset paths:** Check the paths configured at the beginning of each notebook. You may need to change them to match the location of your `HD-EPIC` directory.
- **Dependencies:** Install the Python packages required by the downloader and notebooks before running them. The notebooks may require additional dependencies beyond those used by the downloader.
- **Compute requirements:** Feature extraction and model training can be computationally demanding. A GPU-enabled environment, such as Google Colab, may be useful.
- **Storage:** Video files and extracted feature vectors can require substantial disk space. Ensure that sufficient storage is available before downloading and processing the dataset.
- **Execution order:** Run `Features.ipynb` and `Dataset.ipynb` before `CV.ipynb`. The model notebook depends on the artefacts generated during preprocessing.
- **Reproducibility:** Use the same train/validation/test split configuration when reproducing the experiments, rather than generating a new split for each run.
