## Data Transformation with MS Excel Power Query
The goal of this data transformation is to replace the data in the main table (Table 1) with data from another table (Table 2).
- The data in the main table (Table 1 - Raw Data) is incorrectly inserted.
- A new table (Table 2 - Correct Data) is created to track the accurate information.
![Screenshot 2024-06-16 142714](https://github.com/stephanyed/replace-data-from-another-table/assets/170702920/47b567bd-0fd0-4309-8c8c-a82f52b30500)

## Data
Table 1: The raw data is collected from different users via surveys and includes information on their energy and water usage.
Data Columns in Table 1: Year, ID, Factory Name, energyusage.value, energyusage.unit, waterusage.value, waterusage.unit
Table 2: Users might accidentally insert incorrect data during the survey and then send in the correct data after the survey period.
Data Columns in Table 2: Year, Factory Name, Data to correct columns (energyusage.value, ..., waterusage.unit)

## Benefits
Instead of MS Excel formulas, Power Query is used for data transformation. This approach:
- Avoids accidental edits to the code/formulas.
- Prevents manual edits to incorrect data.

## Methodology
- Load the raw data into Power Query as Table 1.
- Input the correct data into Table 2, including only the columns that need to be corrected.
- Ensure that the columns "Year" and "Factory Name" are included in Table 2, as they will be used to identify the data to be replaced.
- Load the correct data into Power Query as Table 2.
- Use "Table.FromRecords" to check if the data needing replacement is available in Table 2.
- Create the corrected table (Table 3) with the accurate data.

```bash
#"Correct data check" =
Table.FromRecords(Table.TransformRows(Table1 ,each let a= Table2 {[Year=[Year],Factory Name=[Factory Name]]}? in if a is null then _
else Record.TransformFields(_,Table.ToList(Table.SelectRows(Record.ToTable(a),each [Value]<>null and [Value]<>""),each {_{0},(x)=>_{1}}),1)))

```
