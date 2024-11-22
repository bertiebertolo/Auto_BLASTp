# **Cross-Species BLASTp Analysis**

This repository contains the code to perform BLASTp analysis across multiple species for gene-specific datasets. The project automates querying each species against others using protein FASTA sequences, generating results in a CSV format for cross-species comparisons. Thisis designed to replace the manual use of the BLAST software, enabling faster and automated analyses for multiple genes.

---

## **Table of Contents**

1. [Overview](#overview)  
2. [Setup Instructions](#setup-instructions)  
3. [Usage](#usage)
---

## **Overview**

This project automates BLASTp (Protein BLAST) comparisons for genes across multiple species. The script is designed to:
- Check if the genes to be analysed are available on the NCBI database.
- Process protein FASTA files from a specified directory.
- Create BLASTp databases dynamically from the FASTA files.
- Perform BLASTp queries for each species against others in the dataset.
- Generate CSV files summarizing results, including alignment statistics such as percentage identity, bit score, and alignment length.

The primary objective is to streamline and automate the process of comparing gene homologs across species, ensuring a reproducible and scalable pipeline.

---

---

## **Setup Instructions**

### Prerequisites

1. **Required Libraries**: Ensure the following Python libraries are installed:
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

2. **BLAST+ Suite**:
   - Download the BLAST+ toolkit (version `2.16.0+`) from [NCBI](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download).
   - Extract the downloaded archive. The extracted folder will be named `ncbi-blast-2.16.0+`.
   - Move the entire `ncbi-blast-2.16.0+` folder into the main project directory, where all other files (e.g., input Excel/CSV files, Jupyter notebooks) are located.

3. **Verify Installation**:
   - Ensure BLAST is correctly installed by running:
     ```bash
     ncbi-blast-2.16.0+/bin/blastp -version
     ```
   - If successful, you will see the version information for BLAST.

4. **Optional: Update PATH**:
   - If you prefer not to keep the BLAST folder in the project directory, you can add the `ncbi-blast-2.16.0+/bin` directory to your system's PATH environment variable. This will allow BLAST commands to be executed from any location.

---

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

## **Usage**

### Step 1: Run the Setup Verification in Jupyter Notebook

1. Open the Jupyter notebook (`BLAST_Analysis.ipynb`) in your preferred environment (e.g., JupyterLab, Jupyter Notebook).

2. Locate the first cell titled **"Verify Required Libraries and BLAST Installation"**.

3. Run the cell to verify:
   - All required Python libraries (`pandas`, `Bio`, etc.) are installed.
   - BLAST+ (`ncbi-blast-2.16.0+`) is properly set up in the project folder.

### Expected Output:
- If all dependencies and BLAST are available:

---

### Step 2: Prepare Input Data
1. **Input File Format**:
   - The input files must be in **Excel** format. CSV files can also be used, but the Pandas `read_excel` function in the code must be adjusted to `read_csv` accordingly.
   - Update the file name in the code to match your input file. In this example, we used `Dros_gene_names_full.xlsx`.
   - Each Excel file should contain gene-specific data, formatted as follows:

     | Gene Name       | Gene ID |
     |------------------|---------|
     | Ato (atonal)    | 12345   | 
     | Ato (atonal)    | 123456  |

   - In some cases the gene name can be used for Gene ID (especially for Drosophila Melongaster), but we recommend using the Gene  ID from the NCBI database to avoid errors.
   -  Ensure the sheet name in the Excel file is set correctly. In this example, the sheet name is `Gene_names_ids`. If you use a different sheet name, update the code accordingly.

2. **Organize Input Files**:
   - Place your Excel file in the same folder as the Jupyter notebook to ensure they are accessible during execution.

---

### Step 3: Run the Script
1. **Execute the Notebook**:
   - Open the Jupyter notebook in your preferred environment (e.g., JupyterLab, Jupyter Notebook).
   - Update any file paths in the code to match your input files.
   - Run the notebook cells sequentially to process the input data, create BLAST databases, and perform BLASTp comparisons.

2. **What the Notebook Does**:
   - Reads the gene names and IDs from the specified Excel file.
   - Downloads the FASTA files from NCBI according to each species.
   - Dynamically generates BLASTp databases.
   - Runs BLASTp queries for each gene and species, saving the results.

---

### Step 4: Check the Output
1. **Output Directory**:
   - Results will be saved in the `cross_species_blast_results` directory.

2. **File Types**:
   - **Gene-Specific Results**: A CSV file for each gene, summarizing BLAST results for all species. Results are organized in subdirectories by gene name. Example structure:
     ```
     cross_species_blast_results/Gene_name/
     ├── Species_A_vs_all.csv
     ├── Species_B_vs_all.csv
     ├── Species_C_vs_all.csv
     └── ...
     ```
     Example files for a gene `Ato (atonal)`:
     ```
     cross_species_blast_results/Ato/
     ├── Drosophila_melanogaster_vs_all.csv
     ├── Drosophila_simulans_vs_all.csv
     ├── Drosophila_yakuba_vs_all.csv
     └── ...
     ```
     Each file summarizes BLASTp results for a specific species against all others in the dataset.

   - **Master Results**: A consolidated CSV file containing all results across genes and species:
     ```
     cross_species_blast_results/master_blast_results.csv
     ```


3. **CSV Contents**:
   - Gene Name
   - Query Species
   - Target Species
   - % Identity
   - Alignment Length
   - Bit Score
   - And other alignment statistics

