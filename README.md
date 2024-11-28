# **Cross-Species BLASTp Analysis**

This repository contains the code to perform BLASTp analysis across multiple species for gene-specific datasets. The project automates querying each species against others using protein FASTA sequences, generating results in a CSV format for cross-species comparisons. This is designed to replace the manual use of the BLAST software, enabling faster and automated analyses for multiple genes.

---

## **Table of Contents**

1. [Overview](#overview)  
2. [Setup Instructions](#setup-instructions)  
3. [Usage](#usage)  
4. [Output Explanation](#output-explanation)  

---

## **Setup Instructions**

### **Prerequisites**

1. **Recommended: Use Jupyter Notebook on Microsoft Visual Code**:
   - For better interactivity and visualization, it is highly recommended to run this project in Jupyter Notebook.
   - Download and Install [VS Code](https://code.visualstudio.com/download) (follow the instructions and download based on operating system and open VS code)
   - Next open the "Extension"s tab and Install the "Python" and Jupyter "Extensions".
   - Also is is recommended to install the extensions "Excel Viewer" and "Rainbow CSV" for easy results viewing.

2. **Opening the git repository**
   - Click on the "Explorer" Tab and click on "Clone Repository".
   - In the repository name paste this link
     ```

     ```

2. **Install Anaconda**:
   - Download and install [Anaconda](https://www.anaconda.com/products/individual) (which includes Python and package management tools).
   - After installation, open the bash terminal in VS code.

3. **Create a Python Environment**:
   - Create a new conda environment for this project. Run the following command in the Jupyter Terminal:
   - Activate Anaconda:
     ```bash
     conda create --name blastp_env python=3.8
     ```
   - Activate the environment:
     ```bash
     conda activate blastp_env
     ```
4. **Importing github directory
4. **Required Libraries**: Ensure the following Python libraries are installed in your environment:
   - `os` (standard library, no installation required)
   - `time` (standard library, no installation required)
   - `pandas`: Install using:
     ```bash
     pip install pandas
     ```
   - `subprocess` (standard library, no installation required)
   - `Bio` (from Biopython): Install using:
     ```bash
     pip install biopython
     ```

5. **BLAST+ Suite**:
   - Download the BLAST+ toolkit (version `2.16.0+`) from [NCBI](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download).
   - Extract the downloaded archive. The extracted folder will be named `ncbi-blast-2.16.0+`.
   - Move the entire `ncbi-blast-2.16.0+` folder into the main project directory, where all other files (e.g., input Excel/CSV files, Jupyter notebooks) are located.

6. **Verify Installation**:
   - Ensure BLAST is correctly installed by running:
     ```bash
     ncbi-blast-2.16.0+/bin/blastp -version
     ```
   - If successful, you will see the version information for BLAST.

7. **Optional: Update PATH**:
   - If you prefer not to keep the BLAST folder in the project directory, you can add the `ncbi-blast-2.16.0+/bin` directory to your system's PATH environment variable. This will allow BLAST commands to be executed from any location.


### **Folder Structure**

Ensure the following structure for your project directory:

 ```
project_folder/
├── ncbi-blast-2.16.0+/      # BLAST+ executable folder
│   ├── bin/
│   │   ├── blastp           # BLASTp executable
│   │   ├── makeblastdb      # Database creation executable
│   │   └── other BLAST tools
│   └── README               # BLAST+ documentation
├── Dros_gene_names_full.xlsx  # Input file with gene names and IDs (this can be changed accordingly)
├── BLAST_Analysis.ipynb     # Jupyter notebook for running the analysis
```

---

## **Usage**

### **Step 1: Prepare Input Data**
1. **Input File Format**:
   - The input files must be in **Excel** format. CSV files can also be used, but the Pandas `read_excel` function in the code must be adjusted to `read_csv` accordingly.
   - Update the file name in the code to match your input file. In this example, we used `Dros_gene_names_full.xlsx`.
   - Each Excel file should contain gene-specific data, formatted as follows:

     | Gene Name       | Gene ID | Species       |
     |------------------|---------|---------------|
     | Ato (atonal)    | 12345   | Drosophila_X  |
     | Ank2 (Ankyrin)  | 67890   | Drosophila_Y  |

   - In some cases, the gene name can be used for the Gene ID (especially for *Drosophila melanogaster*), but we recommend using the Gene ID from the NCBI database to avoid errors.
   - Ensure the sheet name in the Excel file is set correctly. In this example, the sheet name is `Gene_names_ids`. If you use a different sheet name, update the code accordingly.

2. **Organize Input Files**:
   - Place your Excel file in the same folder as the Jupyter notebook to ensure they are accessible during execution.

---

### **Step 2: Run the Script**

#### **1. Verifying All Libraries**  
- This cell just verifies that all libraries needed are installed and working properly.

#### **2. Checking That Gene IDs Are Correct**  
- Reads the gene names and IDs from the specified Excel file and checks if they exist on the NCBI site.
- If there are any that are not found, they are printed. This is useful for debugging since the download code takes a long time.

#### **3. Downloading the FASTA File for Each Protein**  
- Downloads the FASTA files from NCBI according to each species, with all protein orthologs.
- Downloads FASTA files into:
  - A **grouped folder**, grouped by gene for BLAST.
  - An **individual folder**, where they are not grouped, for tools like MEGA.
- This takes quite long depending on your genes since many files need to be downloaded.

#### **4. Creating the BLAST Databases**  
- Organizes the FASTA files into BLAST-compatible databases.
- These BLAST databases are critical for sequence comparison tasks, allowing you to run local BLAST searches.

#### **5. Running BLAST Using One Query Species**  
- Runs cross-species BLAST. The query species is chosen by typing it into the input box after running the cell. Keep in mind to use exact spelling and to replace any spaces with `_`.
- Produces two CSV files:
  - One where the chosen species was compared against itself. This is to check for any errors in the BLAST.
  - Another showing the BLAST results where the query species was BLASTed against all other species in the dataset.

---

## **Output Explanation**

### **1. Results Directory**
- Results will be saved in the `cross_species_blast_results` directory.

### **2. File Types**
- **Gene-Specific Results**: A CSV file for each gene, summarizing BLAST results for all species. Results are organized in subdirectories by gene name. Example structure:
     ```
     blast_results_species_A_vs_all/Gene_name/
     ├── Species_A_self_comparison.csv
     ├── Species_A_vs_all.csv
     ```
     Example files for a gene `Ato (atonal)`:
     ```
     blast_results_species_A_vs_all/Ato_(atonal)/
     ├── Species_A_self_comparison.csv
     ├── Species_A_vs_all.csv
     ```
     Each file summarizes BLASTp results for a specific species against all others in the dataset.


### **3. CSV Contents**
- **Columns**:
- Gene Name
- Query Species
- Target Species
- % Identity
- Alignment Length
- Bit Score
- And other alignment statistics

---

### **Notes**
- Ensure accurate species and gene names to avoid mismatches.
- Processing time varies depending on the dataset size. For larger datasets, use high-performance hardware.

