# CPC-Tree-and-Grouping-Pipeline

## Project Overview

This repository contains a collection of R scripts designed for processing and analyzing patent data based on the Cooperative Patent Classification (CPC) system. The pipeline focuses on downloading and cleaning CPC classification tables, generating hierarchical structures, and preparing data for analysis at various levels (Realgroup, Subclass, and Maingroup). This systematic approach aims to establish a foundation for patent data analysis, enabling efficient execution of diverse analytical tasks, including time-series analysis.

**This code aims to reconstruct the hierarchical structure of the CPC (Cooperative Patent Classification) system, including Class, Subclass, Maingroup, and Subgroup, and to assign a new Real_Group ID to each of the approximately 260,000 CPC entries. The Real_Group IDs are reconstructed based on the CPC's depth, resetting the inaccurate hierarchical structure of the existing Class, Subclass, Maingroup, and Subgroup levels to establish a precise upper and lower hierarchical structure.**

The pipeline comprises four primary modules:

1.  **MODULE 1: CPC Table Download and Preprocessing**
    *   Downloads the latest CPC classification table (Once every half year) in Excel format from the Korea Institute of Patent Information(KIPI) and imports it into R.
        ''The Korea Institute of Patent Information(KIPI) no longer provides Rawdata online. The "CPC분류표_섹션별_2022년8월" file, which contains 260,000 CPC Lists, must be received directly through The Korea Institute of Patent Information(KIPI).''
    *   Cleans the data by removing unnecessary columns and handling missing values.
    *   Outputs the refined data in an Excel file (`CPC_tree_description_202208_XLS.xlsx`).

2.  **MODULE 2: Realgroup Generation**
    *   Assigns a unique "Realgroup" ID to each CPC code based on the processed CPC table.
    *   The Realgroup ID reflects the hierarchical structure of the CPC codes, serving as a basis for detailed analysis.
    *   Outputs the data with added Realgroup IDs in an Excel file (`CPC_tree_description_202208(allcolumns_realgroup)_XLS.xlsx`) and a CSV file (`CPC_tree_description_202208(realgroup)_CSV.csv`).

3.  **MODULE 3: Subclass Data Generation**
    *   Extracts the Subclass level from the CPC codes.
    *   Outputs a data frame containing Subclass codes and their corresponding CPC codes as a CSV file (`CPC_tree_description_202208(subclass)_CSV.csv`).
    *   Provides foundational data for analysis at the Subclass level.

4.  **MODULE 4: Maingroup Data Generation**
    *   Extracts the Maingroup level from the CPC codes.
    *   Outputs a data frame containing Maingroup codes and their corresponding CPC codes as a CSV file (`CPC_tree_description_202208(subgroup)_CSV.csv`).
    *   Provides foundational data for analysis at the Maingroup level.

## Data Flow

The data flow within the pipeline is as follows:

1.  **Input:** CPC classification table Excel file downloaded from KIPO (`CPC분류표_섹션별_2022년8월.xlsx`, located in the `Input/` directory).
2.  **MODULE 1 Output:** Refined CPC data (Excel file: `CPC_tree_description_202208_XLS.xlsx`, located in the `Output/` directory).
3.  **MODULE 2 Input:** Uses the output of `MODULE 1` to generate Realgroup IDs.
4.  **MODULE 2 Output:** CPC data with added Realgroup IDs (Excel file: `CPC_tree_description_202208(allcolumns_realgroup)_XLS.xlsx` and CSV file: `CPC_tree_description_202208(realgroup)_CSV.csv`, located in the `Output/` directory).
5.  **MODULE 3 Output:** Subclass data (`CPC_tree_description_202208(subclass)_CSV.csv`, located in the `Output/` directory).
6.  **MODULE 4 Output:** Maingroup data (`CPC_tree_description_202208(subgroup)_CSV.csv`, located in the `Output/` directory).

## Prerequisites

*   R (version 4.0 or higher)
*   Required R packages:
    *   `purrr`
    *   `readxl`
    *   `data.table`
    *   `writexl`

## Installation and Usage

1.  Clone this repository.
    ```bash
    git clone [repository_URL]
    ```
2.  Install the necessary R packages.
    ```R
    install.packages(c("purrr", "readxl", "data.table", "writexl"))
    ```
3.  Place the latest CPC classification table Excel file downloaded from KIPO in the `Input/` directory. (Filename: `CPC분류표_섹션별_2022년8월.xlsx`).
4.  Run each module script sequentially:
    ```R
    source("MODULE1.R")
    source("MODULE2.R")
    source("MODULE3.R")
    source("MODULE4.R")
    ```
5.  The output files will be stored in the `Output/` directory.

## Code Structure

*   **Input:** Folder containing the CPC classification table Excel file.
*   **Output:** Folder to store the results of the analysis.
*   **MODULE1.R:** Script to download and preprocess the CPC table.
*   **MODULE2.R:** Script to generate Realgroup IDs.
*   **MODULE3.R:** Script to generate Subclass data.
*   **MODULE4.R:** Script to generate Maingroup data.

## Additional Notes

*   Detailed code explanations are provided through comments within each module script.
*   Input file paths and output file formats can be modified as needed.

## Potential Enhancements

*   Add an automatic update feature for the latest version of the CPC data.
*   Develop additional modules for more complex analytical tasks (e.g., time-series analysis, network analysis).
*   Add features to visualize and analyze the data through a web interface.

## License

This project is licensed under the [Specify License].

## Contact

For any questions or suggestions, please feel free to contact us.
