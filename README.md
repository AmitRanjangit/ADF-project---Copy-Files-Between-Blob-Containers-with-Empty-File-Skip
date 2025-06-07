# ADF-project---Copy-Files-Between-Blob-Containers-with-Empty-File-Skip

This project demonstrates how to use **Azure Data Factory (ADF)** to copy files from one Azure Blob Storage container to another, while automatically **skipping empty files**. It uses a combination of `Get Metadata`, `ForEach`, `Lookup`, `If Condition`, and `Copy Data` activities within a pipeline.

---

## Objective

To copy only non-empty files from a **source Azure Blob container** to a **destination container**, thereby avoiding unnecessary processing and storage of empty files.

---

## Technologies Used

- Azure Data Factory (ADF)
- Azure Blob Storage
- JSON-based pipeline configuration
- Delimited (CSV-style) file format

---

## Pipeline Design Overview

### Activities Used:

| Activity         | Purpose                                                                 |
|------------------|-------------------------------------------------------------------------|
| Get Metadata     | To list all files in the source container (`childItems`).              |
| ForEach          | To iterate over each file returned by Get Metadata.                    |
| Set Variable     | To store the current file name for each iteration.                     |
| Lookup           | To read the content of the current file and determine row count.       |
| If Condition     | To check if the file is non-empty (`row count > 0`).                   |
| Copy Data        | To copy the non-empty file to the destination container.               |

---

## Key Concepts Implemented

- **Dynamic file selection:** Uses `@item().name` to dynamically set the filename in each iteration.
- **Condition-based copy:** Uses an `IfCondition` activity to skip copying files with zero rows.
- **Variable usage:** Temporary storage of filename using `SetVariable`.
- **Reusability:** Single pipeline handles multiple files without hardcoding.

---

## Pipeline Flow

1. **Get Metadata Activity**  
   Retrieves the list of files (`childItems`) from the source container using the dataset `DelimitedText1`.

2. **ForEach Activity**  
   Loops through each file.

3. **Set Variable**  
   Stores the file name from current loop item.

4. **Lookup Activity**  
   Loads the file content based on the file name stored in the variable, and counts the number of rows.

5. **If Condition**  
   Checks if the row count from the Lookup is greater than 0.

6. **Copy Data Activity**  
   If true, the file is copied to the destination container using dataset `DelimitedText2`.

---

## File Format Details

- Files are in **CSV format** (delimited text).
- All text is **quoted** (`quoteAllText: true`).
- Output files have a `.txt` extension, although the format remains CSV.

---

## Dataset References

- **DelimitedText1** – Points to the source Blob container.
- **DelimitedText2** – Points to the destination Blob container.

---

## How to Use This Project

1. Create source and destination containers in Azure Blob Storage.
2. Upload multiple test CSV files to the source container, including some empty files.
3. Import this pipeline JSON into Azure Data Factory.
4. Configure the datasets (`DelimitedText1`, `DelimitedText2`) with correct container paths.
5. Trigger or debug the pipeline.
6. Only non-empty files will appear in the destination container.

---

## Benefits

- Avoids unnecessary storage of empty files.
- Saves on compute and I/O during data integration tasks.
- Ensures data quality by filtering out noise.

