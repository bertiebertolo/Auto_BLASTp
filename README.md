# **Cross-Species BLASTp Analysis**


---

## **Table of Contents**

1. [Overview](#overview)  
2. [Setup Instructions](#setup-instructions)  
3. [Usage](#usage)  
4. [Output Explanation](#output-explanation)  

---

## **Overview**

This repository automates cross-species BLASTp analysis for gene-specific datasets. The goal is to streamline and accelerate the process of querying protein sequences across multiple species, generating results in CSV format for easy comparison. By leveraging BLASTp and integrating it with Python scripting, this tool replaces the manual use of BLAST software, making large-scale analyses faster and more efficient. It is particularly useful for researchers conducting comparative genomics, functional annotation, or evolutionary studies across different species.

**Key Features:**
   - Automates the BLASTp querying process for multiple species.
   - Generates CSV output for cross-species protein comparisons.
   - Reduces manual intervention, improving efficiency.
   - Compatible with Jupyter Notebook for interactive usage.
   - This tool is ideal for anyone needing to perform cross-species protein analysis on gene datasets, simplifying the workflow and reducing computational overhead.

---

## **Setup Instructions**

### **Prerequisites**

1. **Recommended: Use Jupyter Notebook on Microsoft Visual Code**:
   - For better interactivity and visualization, it is highly recommended to run this project in Jupyter Notebook.
   - Download and Install [VS Code](https://code.visualstudio.com/download) (follow the instructions and download based on operating system and open VS code)
   - Next open the "Extension"s tab and Install the "Python" and "Jupyter" Extensions.
   - Also it is recommended to install the extensions "Excel Viewer" and "Rainbow CSV" for easy results viewing.

2. **Install Anaconda**:
   - Download and install [Anaconda](https://www.anaconda.com/products/individual) (which includes Python and package management tools).
   - Open Anaconda Navigator to verify the installation.
     
4. **Importing github directory**
   - Open the "Explorer" tab and click on "Clone Repository".
   - Paste the repository URL
     ```
     https://github.com/bertiebertolo/Auto_BLASTp.git
     
5. **Create a Python Environment**:
   - Create a new conda environment for this project. Run the following command in the VS code Terminal, this can be done by selecting "View > Terminal" from the menu bar:
   - Create the enviroment by copying and pasting this:
     ```bash
     conda create --name blastp_env python=3.10.15
     ```
   - Activate the environment:
     ```bash
     conda activate blastp_env
     ```
6. **Required Libraries**: Ensure the following Python libraries are installed in your environment, if none are installed paste the commands of all below:
   - `os` (standard library, no installation required)
   - `time` (standard library, no installation required)
   - `pandas`: Install on the VS code bash terminal using:
     ```bash
     pip install pandas
     ```
   - `subprocess` (standard library, no installation required)
   - `Bio` (from Biopython): Install using:
     ```bash
     pip install biopython
     ```
   - After this click select kernel on VS code and select the enviroment that you just created "blatsp_env" as your kernel.
7. **BLAST+ Suite**:
   - Download the BLAST+ toolkit (version `2.16.0+`) from [NCBI](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download).
   - Extract the downloaded archive. The extracted folder will be named `ncbi-blast-2.16.0+`.
   - Move the entire `ncbi-blast-2.16.0+` folder into the main project directory "Auto_BLASTp", where all other files (e.g., input Excel/CSV files, Jupyter notebooks) are located.

8. **Verify Installation**:
   - Ensure BLAST is correctly installed by running in the terminal:
     ```bash
     ncbi-blast-2.16.0+/bin/blastp -version
     ```
   - If successful, you will see the version information for BLAST.

9. **Optional: Update PATH**:
   - If you prefer not to keep the BLAST folder in the project directory, you can add the `ncbi-blast-2.16.0+/bin` directory to your system's PATH environment variable. This will allow BLAST commands to be executed from any location.


### **Folder Structure**

Ensure the following structure for your project directory:

 ```
Auto_BLASTp/
├── ncbi-blast-2.16.0+/      # BLAST+ executable folder
│   ├── bin/
│   │   ├── blastp           # BLASTp executable
│   │   ├── makeblastdb      # Database creation executable
│   │   └── other BLAST tools
│   └── README               # BLAST+ documentation
├── Gene_data.xlsx  # Input file with gene names and IDs (this can be changed accordingly)
├── BLAST_Analysis.ipynb     # Jupyter notebook for running the analysis
├── README.md                #This markdown File
```

---

## **Usage**

### **Step 1: Prepare Input Data**
1. **Input File Format**:
   - You can manually paste the data in the excel "Gene_data.xlsx" file directly or alternatively upload your own Excel File (note make sure to change the file name in the code) or use the same file name as this just make sure the format is the same.
   - CSV files can also be used but the read function in the code needs to be adjusted.
   - The Excel file should contain gene-specific data, formatted as follows:

     | Gene Name       | Gene ID | Species       |
     |------------------|---------|---------------|
     | Ato (atonal)    | 12345   | Drosophila_X  |
     | Ank2 (Ankyrin)  | 67890   | Drosophila_Y  |

   - In some cases, the gene name can be used for the Gene ID (especially for *Drosophila melanogaster*), but we recommend using the Gene ID from the NCBI database to avoid errors.
   - Ensure the sheet name in the Excel file is set correctly. In this example, the sheet name is `Gene_names_ids`. If you use a different sheet name, update the code accordingly.

3. **Organize Input Files**:
   - Place your Excel file in the same folder as the Jupyter notebook to ensure they are accessible during execution.

---

### **Step 2: Run the Script**

   -Before running your script please replace your e-mail in any spaces saying "youremail@example.com" this will be necessary for the entrez search

#### **1. Verifying All Libraries**  
- This cell just verifies that all libraries needed are installed and working properly.
- If there are any problems please refer back to the [Setup Instructions](#setup-instructions).

#### **2. Checking That Gene IDs Are Correct**  
- Reads the gene names and IDs from the specified Excel file and checks if they exist on the NCBI site.
- If there are any that are not found, they are printed. This is useful for debugging since the download code takes a long time.

#### **3. Downloading the FASTA File for Each Protein**  
- Downloads the FASTA files from NCBI according to each species, with all protein orthologs.
- Downloads FASTA files into:
  - A **grouped folder**, grouped by gene for BLAST.
  - An **individual folder**, this is a folder with all the FASTA files ungrouped.
- This step takes quite a long time especially if dealing with a lot of protein sequences.

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

