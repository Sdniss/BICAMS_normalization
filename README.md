# BICAMS z-normalization

## Introduction

### AIMS - VUB

First of all, thank you very much for your interest in using our project on behalf of the Artificial Intelligence and Modelling in clinical Sciences (AIMS) lab, part of the Vrije Universiteit Brussel (VUB). We aim to contribute maximally to optimal clinical care in neurodegenerative disorders, with a special focus on Multiple Sclerosis, by performing relevant and advanced modelling on neurophysiological and brain imaging data. Moreover, in light of the prosper of the field and general understanding of our research, we do efforts to contribute to open, reproducible and transparant science by sharing code and actively practicing science communication on our [AIMS website](https://aims.research.vub.be).

### The project

This project is an extension to the paper of [Costers et al. 2017](https://doi.org/10.1016/j.msard.2017.08.018), which was published in Multple Sclerosis and Related Disorders. In short, the paper validated the Brief International Cognitive Assessment for Multiple Sclerosis (BICAMS) in a Belgian, Dutch-speaking population. It hereby provided regression-based norms for the 3 subtests of BICAMS:

- The Symbol Digit Modalities Test (SDMT)
- The California Verbal Learning Test, 2nd edition (CVLT-II)
- The Brief Visuospatial Memory Test Revised (BVMT-R)

The regression-based norms allow a raw score on any of the 3 cognitive tests stated above to be converted to a score that is corrected for three factors that were found in the paper to impact cognitive performance:

- Age
- Sex
- Educational status

In short, by correcting for these factors, the resulting z-score can be compared to cognitive scores without interference by them. 3 important phases can be distinguished:

1. Scaling of the raw scores
2. Predicting which score should normally be obtained by the subject according to their age, sex and education level. 
3. Obtain z-score: subtract the predicted score (2) from the scaled score (1), and divide by the residual error of the regression model

### Important considerations

Both the conversion table per test, used for the scaling of raw scores, and the fitting of the regression-line to yield the weights for the features within the regression model (age, age^2, sex, education level) rely on data from a sample of 97 Belgian, Dutch-speaking healthy controls. The demographics of this population (especially age and education, 43.52 ± 12.69 and 14.69 ± 1.61 (mean ± std) respectively) should be taken into account when converting a z-score for a subject. We highlight to be especially careful when calculating z-scores when a participant's characteristics have extreme values (either very low or very high) on either age or education level. 

Furthermore, testing conditions for this paper were very strict. E.g. for the SDMT, patients were not allowed to keep track of their progression on the test paper by using their fingers to indicate the symbol that needed to be converted into a digit. Please make sure that every subtest of BICAMS was administered with careful attention for correct execution. Moreover, only the Dutch version of CVLT-II is eligible for the z-normalization within this project.

## Deliverables

With this code, you can easily transform cognitive scores on BICAMS to z-scores by following the steps listed in the [Set up the environment + run the main script](#set-up-the-environment--run-the-main-script). A dataframe will be returned that you can subsequently use for your projects.

## Set up the environment + run the main script

General: Make sure to have any version of python 3 installed on your computer

### 1. Clone the repository to your local computer

Please open a terminal window in a folder that will subsequently contain the GitHub repo after running following command: `git clone https://github.com/Sdniss/BICAMS_normalization`. Subsequently, type `cd BICAMS_normalization` to enter that folder in the terminal.

### 2. Environment set-up: 

To be able to run the main script, we first have to set up the environment containing the correct dependencies that the code relies on. In the table below, please pick the column that accords with the operating system you are using and run the commands in the terminal. Under the `explanation` column, you can keep track of what operation is performed with the corresponding command.

| Step | Explanation                                                  | Mac                                    | Windows                             | Linux                                  |
| ---- | ------------------------------------------------------------ | -------------------------------------- | ----------------------------------- | -------------------------------------- |
| 1    | Create a virtual environment (`BICAMS_norm_venv`) within your local repository | `python3 -m venv BICAMS_norm_venv`     | `python3 -m venv BICAMS_norm_venv`  | `python3 -m venv BICAMS_norm_venv`     |
| 2    | Activate the virtual environment                             | `source BICAMS_norm_venv/bin/activate` | `BICAMS_norm_venv\Scripts\activate` | `source BICAMS_norm_venv/bin/activate` |
| 3    | Install all dependencies (`dependencies.txt`) within the virtual environment | `pip3 install -r dependencies.txt`     | `pip3 install -r dependencies.txt`  | `pip3 install -r dependencies.txt`     |

### 3. Prepare the dataframe

To perform the calculations for z-scores and impairment per domain, complete the following steps:

1. Prepare your dataframe to meet the following requirements:

   - Filename: `data_to_transform.xlsx` (excel file)

   - Column headers: `age`, `sex`, `education`, `sdmt`, `bvmt`, `cvlt`

     Note 1: please use exactly these column names in this order

     Note 2: only the 3 first columns are an absolute requirement. For the cognitive scores, please prepare your dataframe to only contain columns for which you have data. Hence, this can be a subset of the latter 3 columns, but should at least include one of them

   - Data:
     - age: years (integer)
     - sex: 
       - 1 = Male
       - 2 = Female
     - education (years of education): 
       - 6  = Primary school
       - 12 = High school 
       - 13 = Professional education 
       - 15 = Bachelor 
       - 17 = Master 
       - 21 = Doctorate
     - sdmt/bvmt/cvlt: raw score on the test
   
2. Upload your file to the `data` directory. It will replace the `data_to_transform.xlsx` that is currently there, and which is just mock data included by default

### 4. Run the main script

Run either `python BICAMS_script.py` or open jupyter notebook and use `BICAMS_script.ipynb`  if you are a jupyter notebook user

### 5. Extract the resulting dataframe

Extract the transformed data from the `data` folder: `transformed_data.xlsx`. Also checkout the description of what the output, `transformed_data.xlsx`, contains. It is located within the `data_descriptions` folder, and is called `transformed_data_description.txt`
