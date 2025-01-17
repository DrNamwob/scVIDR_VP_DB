Developed by Omar Kana <kanaomar@msu.edu>

CLI integration by David Filipovic <filipov4@msu.edu>

Maintained by David Filipovic <filipov4@msu.edu>

# scVIDR
Single Cell Variational Inference of the Dose Response (scVIDR) a  variational autoencoder tool used to predict expression of chemcial perturbations across cell types.

[![DOI](https://zenodo.org/badge/549268647.svg)](https://zenodo.org/badge/latestdoi/549268647)

## Publication
[Publication in Patterns](https://doi.org/10.1016/j.patter.2023.100817)

## Installation
```
git clone https://github.com/BhattacharyaLab/scVIDR.git
cd scVIDR
conda create -n scVIDR python=3.8.5
conda activate scVIDR
pip3 install -r requirements.txt
pip3 install geomloss==0.2.5
pip install torch==1.8.1+cu111 torchaudio==0.8.1 torchvision==0.9.1 -f https://download.pytorch.org/whl/torch_stable.html
```

## Data
To get the data directory for figure notebooks: 
https://drive.google.com/file/d/11fzDbp0B19Dy47MtD742Jl4Hz2bdSiiq/view?usp=sharing

Once you download `data.zip` copy it to `scVIDR/data` and unzip it there.


## VAE model training (single- and multi- dose models)

```
To train a single dose VAE model run the `scvidr_train.py single_dose` command (Figure 2 of the manuscript).

To train a multi dose VAE model run the `scvidr_train.py multi_dose` command (Figure 3 of the manuscript).


Both types of models expect an AnnData file in the h5ad format as input. In addition to the scRNAseq data, the obs table is also expected to contain a dose and a cell type column.
The single dose model expects at least two distinct doses, whereas the multi dose model expects at least three.

Command arguments are the same for both types of models and are listed below.

 

usage: `scvidr_train.py {single_dose/multi_dose} [-h] [--dose_column DOSE_COLUMN] [--celltype_column CELLTYPE_COLUMN] [--test_celltype TEST_CELLTYPE] [--treated_dose CONTROL_DOSE] [--treated_dose TREATED_DOSE] [--celltypes_keep CELLTYPES_KEEP] h5ad_data_file model_path`
```

Train a VAE model applicable to scGen and scVIDR using a h5ad input dataset

positional arguments:
```
  h5ad_data_file        The data file containing the raw reads in h5ad format

  model_path            Path to the directory where the trained model will be saved

```

model arguments:
```
  -h, --help            show this help message and exit
  --dose_column DOSE_COLUMN
                        Name of the column within obs dataframe representing the dose (default "Dose")
  --celltype_column CELLTYPE_COLUMN
                        Name of the column within obs dataframe representing the cell type (default "celltype")
  --test_celltype TEST_CELLTYPE
                        Name of the cell type to be left out for testing - surround by quotation marks for cell types containing spaces (default "Hepatocytes - portal"
  --control_dose CONTROL_DOSE
                        Control dose (default "0")
  --treated_dose TREATED_DOSE
                        Treated dose (default "30")
  --celltypes_keep CELLTYPES_KEEP
                        Cell types to keep in the dataset during training/testing - either a file containing list of cell types (one cell type per line) or semicolon separated list of cell types (surround in quotation marks) - default all available cell types
                        (default "ALL")
```


To train all the single dose models for all individual cell types used in the manuscript execute

```
python scvidr_train.py single_dose --celltypes_keep ../metadata/liver_celltypes --test_celltype "Hepatocytes - portal" ../data/nault2021_singleDose.h5ad "../data/VAE_Binary_Prediction_Dioxin_5000g_Hepatocytes - central.pt/"
python scvidr_train.py single_dose --celltypes_keep ../metadata/liver_celltypes --test_celltype "Hepatocytes - central" ../data/nault2021_singleDose.h5ad "../data/VAE_Binary_Prediction_Dioxin_5000g_Hepatocytes - portal.pt/"
python scvidr_train.py single_dose --celltypes_keep ../metadata/liver_celltypes --test_celltype "Cholangiocytes" ../data/nault2021_singleDose.h5ad "../data/VAE_Binary_Prediction_Dioxin_5000g_Cholangiocytes.pt/"
python scvidr_train.py single_dose --celltypes_keep ../metadata/liver_celltypes --test_celltype "Stellate Cells" ../data/nault2021_singleDose.h5ad "../data/VAE_Binary_Prediction_Dioxin_5000g_Stellate Cells.pt/"
python scvidr_train.py single_dose --celltypes_keep ../metadata/liver_celltypes --test_celltype "Portal Fibroblasts" ../data/nault2021_singleDose.h5ad "../data/VAE_Binary_Prediction_Dioxin_5000g_Portal Fibroblasts.pt/"
python scvidr_train.py single_dose --celltypes_keep ../metadata/liver_celltypes --test_celltype "Endothelial Cells" ../data/nault2021_singleDose.h5ad "../data/VAE_Binary_Prediction_Dioxin_5000g_Endothelial Cells.pt/"
```


To train all the multi dose models for all individual cell types used in the manuscript execute

```
python scvidr_train.py multi_dose --control_dose 0.0 --celltypes_keep ../metadata/liver_celltypes --test_celltype "Hepatocytes - central" ../data/nault2021_multiDose.h5ad "../data/VAE_Cont_Prediction_Dioxin_5000g_Hepatocytes - central.pt/"
python scvidr_train.py multi_dose --control_dose 0.0 --celltypes_keep ../metadata/liver_celltypes --test_celltype "Hepatocytes - portal" ../data/nault2021_multiDose.h5ad "../data/VAE_Cont_Prediction_Dioxin_5000g_Hepatocytes - portal.pt/"
python scvidr_train.py multi_dose --control_dose 0.0 --celltypes_keep ../metadata/liver_celltypes --test_celltype "Cholangiocytes" ../data/nault2021_multiDose.h5ad "../data/VAE_Cont_Prediction_Dioxin_5000g_Cholangiocytes.pt/"
python scvidr_train.py multi_dose --control_dose 0.0 --celltypes_keep ../metadata/liver_celltypes --test_celltype "Stellate Cells" ../data/nault2021_multiDose.h5ad "../data/VAE_Cont_Prediction_Dioxin_5000g_Stellate Cells.pt/"
python scvidr_train.py multi_dose --control_dose 0.0 --celltypes_keep ../metadata/liver_celltypes --test_celltype "Portal Fibroblasts" ../data/nault2021_multiDose.h5ad "../data/VAE_Cont_Prediction_Dioxin_5000g_Portal Fibroblasts.pt/"
python scvidr_train.py multi_dose --control_dose 0.0 --celltypes_keep ../metadata/liver_celltypes --test_celltype "Endothelial Cells" ../data/nault2021_multiDose.h5ad "../data/VAE_Cont_Prediction_Dioxin_5000g_Endothelial Cells.pt/"
```


**Note**: all of these models are available pretrained (same names as listed above)

## Single dose model prediction
```
usage: scvidr_predict.py single_dose [-h] [--model MODEL]
                                     [--dose_column DOSE_COLUMN]
                                     [--celltype_column CELLTYPE_COLUMN]
                                     [--test_celltype TEST_CELLTYPE]
                                     [--control_dose CONTROL_DOSE]
                                     [--treated_dose TREATED_DOSE]
                                     [--celltypes_keep CELLTYPES_KEEP]
                                     h5ad_data_file model_path output_path
```
Predict treatment condition using a pretrained scVIDR or scGEN model

positional arguments:
```
h5ad_data_file        The data file containing the raw reads in h5ad format
  model_path            Path to the directory where the trained model was
                        saved in the model training step
  output_path           Path to the driectory where the anndata will be output
                        to in an h5ad format
```
optional arguments:
```
  -h, --help            show this help message and exit
  --model MODEL         Use scVIDR or scGen for prediciton (defualt "scVIDR")
  --dose_column DOSE_COLUMN
                        Name of the column within obs dataframe representing
                        the dose (default "Dose")
  --celltype_column CELLTYPE_COLUMN
                        Name of the column within obs dataframe representing
                        the cell type (default "celltype")
  --test_celltype TEST_CELLTYPE
                        Name of the cell type to be left out for testing -
                        surround by quotation marks for cell types containing
                        spaces (default "Hepatocytes - portal"
  --control_dose CONTROL_DOSE
                        Control dose (default "0")
  --treated_dose TREATED_DOSE
                        Treated dose (default "30")
  --celltypes_keep CELLTYPES_KEEP
                        Cell types to keep in the dataset during
                        training/testing - either a file containing list of
                        cell types (one cell type per line) or semicolon
                        separated list of cell types (surround in quotation
                        marks) - default all available cell types (default
                        "ALL")
```
Example of single does prediction command:
```
python scvidr_predict.py single_dose ../data/nault2021_singleDose.h5ad ../data/VAE_Binary_Prediction_Dioxin_5000g_Hepatocytes\ -\ portal.pt/ ../data/SingleDose_TCDD \--model scVIDR --dose_column Dose --celltype_column celltype --test_celltype Hepatocytes - portal --control_dose 0 --treated_dose 30 --celltypes_keep ../metadata/liver_celltypes
```
## Multi dose model prediction
```
usage: scvidr_predict.py multi_dose [-h] [--model MODEL]
                                    [--dose_column DOSE_COLUMN]
                                    [--celltype_column CELLTYPE_COLUMN]
                                    [--test_celltype TEST_CELLTYPE]
                                    [--control_dose CONTROL_DOSE]
                                    [--treated_dose TREATED_DOSE]
                                    [--celltypes_keep CELLTYPES_KEEP]
                                    h5ad_data_file model_path output_path

```

positional arguments:
```
  h5ad_data_file        The data file containing the raw reads in h5ad format
  model_path            Path to the directory where the trained model was
                        saved in the model training step
  output_path           Path to the driectory where the anndata will be output
                        to in an h5ad format
```

optional arguments:
```
  -h, --help            show this help message and exit
  --model MODEL         Use scVIDR or scGen for prediciton (defualt "scVIDR")
  --dose_column DOSE_COLUMN
                        Name of the column within obs dataframe representing
                        the dose (default "Dose")
  --celltype_column CELLTYPE_COLUMN
                        Name of the column within obs dataframe representing
                        the cell type (default "celltype")
  --test_celltype TEST_CELLTYPE
                        Name of the cell type to be left out for testing -
                        surround by quotation marks for cell types containing
                        spaces (default "Hepatocytes - portal"
  --control_dose CONTROL_DOSE
                        Control dose (default "0")
  --treated_dose TREATED_DOSE
                        Treated dose (default "30")
  --celltypes_keep CELLTYPES_KEEP
                        Cell types to keep in the dataset during
                        training/testing - either a file containing list of
                        cell types (one cell type per line) or semicolon
                        separated list of cell types (surround in quotation
                        marks) - default all available cell types (default
                        "ALL")
```

Example of multidose prediction command:
```
python scvidr_predict.py multi_dose ../data/nault2021_multiDose.h5ad ../data/VAE_Cont_Prediction_Dioxin_5000g_Hepatocytes\ -\ portal.pt/ ../data/MultiDose_TCDD \--model scVIDR --dose_column Dose --celltype_column celltype --test_celltype Hepatocytes - portal --control_dose 0.0 --treated_dose 30.0 --celltypes_keep ../metadata/liver_celltypes
```
## Calculate Gene Scores
```
usage: scvidr_genescores.py [-h] [--dose_column DOSE_COLUMN]
                            [--celltype_column CELLTYPE_COLUMN]
                            [--test_celltype TEST_CELLTYPE]
                            [--control_dose CONTROL_DOSE]
                            [--treated_dose TREATED_DOSE]
                            [--celltypes_keep CELLTYPES_KEEP]
                            [--training_size TRAINING_SIZE]
                            h5ad_data_file model_path output_path
```
Interpret scVIDR predictions using ridge regression. Outputs CSV file of gene
scores.

positional arguments:
```
  h5ad_data_file        The data file containing the raw reads in h5ad format
  model_path            Path to the directory where the trained model was
                        saved in the model training step
  output_path           Path to the driectory where the gene scores will be
                        saved as a csv file
```

optional arguments:
```
  -h, --help            show this help message and exit
  --dose_column DOSE_COLUMN
                        Name of the column within obs dataframe representing
                        the dose (default "Dose")
  --celltype_column CELLTYPE_COLUMN
                        Name of the column within obs dataframe representing
                        the cell type (default "celltype")
  --test_celltype TEST_CELLTYPE
                        Name of the cell type to be left out for testing -
                        surround by quotation marks for cell types containing
                        spaces (default "Hepatocytes - portal"
  --control_dose CONTROL_DOSE
                        Control dose (default "0")
  --treated_dose TREATED_DOSE
                        Treated dose (default "30")
  --celltypes_keep CELLTYPES_KEEP
                        Cell types to keep in the dataset during
                        training/testing - either a file containing list of
                        cell types (one cell type per line) or semicolon
                        separated list of cell types (surround in quotation
                        marks) - default all available cell types (default
                        "ALL")
  --training_size TRAINING_SIZE
                        Number of samples generated from latent distribution
```

Example of calculating gene_scores:
```

python scvidr_genescores.py ../data/nault2021_multiDose.h5ad ../data/VAE_Cont_Prediction_Dioxin_5000g_Hepatocytes\ -\ portal.pt/ ../data/MultiDose_TCDD  --dose_column Dose --celltype_column celltype --test_celltype Hepatocytes\ -\ portal --control_dose 0.0 --treated_dose 30.0 --celltypes_keep ../metadata/liver_celltypes
```


## Notebooks for figures

figure       | notebook path| Description|
---------------| ---------------| ---------------|
| [*Figure 2*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/Figure2.ipynb)| notebooks/Figure2.ipynb| Single Dose TCDD| 
| [*Figure 3*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/Figure3.ipynb)| notebooks/Figure3.ipynb| Multi Dose TCDD|
| [*Figure 4*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/Figure4.ipynb)| notebooks/Figure4.ipynb| Gene Scores|
| [*Figure 5*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/Figure5.ipynb)| notebooks/Figure5.ipynb| Pseudodose|
| [*Supplemental Figure 2*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure2.ipynb)| notebooks/SupplementalFigure2.ipynb|PCA of $\delta$| 
| [*Supplemental Figure 3*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure3.ipynb)| notebooks/SupplementalFigure3.ipynb| Single Dose IFNB|
| [*Supplemental Figure 4*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure4.ipynb)| notebooks/SupplementalFigure4.ipynb| Multi Dose sciplex|
| [*Supplemental Figure 5*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure5.ipynb)| notebooks/SupplementalFigure5.ipynb| scVIDR Analysis TCDD|
| [*Supplemental Figure 6*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure6.ipynb)| notebooks/SupplementalFigure6.ipynb| scVIDR Analysis sciplex|
| [*Supplemental Figure 7*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure7.ipynb)| notebooks/SupplementalFigure7.ipynb| scVIDR cross study| 
| [*Supplemental Figure 8*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure8.ipynb)| notebooks/SupplementalFigure8.ipynb| scVIDR cross species|
| [*Supplemental Figure 9*](https://nbviewer.org/github/BhattacharyaLab/scVIDR/blob/main/notebooks/SupplementalFigure9.ipynb)| notebooks/SupplementalFigure9.ipynb| scVIDR equal scGen|

