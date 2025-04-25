Here is a detailed breakdown of the text on working with Excel spreadsheets using Python's `openpyxl` module, organized into a structured outline with key concepts, examples, and review questions.

---

### **Detailed Outline: Working with Excel Spreadsheets in Python**

#### **1. Introduction to Excel and OpenPyXL**
- **Purpose**: Automate Excel tasks (data copying, editing, analysis).
- **Key Tools**:
  - `openpyxl` module (reads/writes `.xlsx` files).
  - Compatible with LibreOffice Calc/OpenOffice Calc.
- **Basic Definitions**:
  - **Workbook**: An Excel file (`.xlsx`).
  - **Sheet/Worksheet**: Individual tabs within a workbook.
  - **Cell**: Box at a column/row intersection (e.g., `A1`).

#### **2. Setting Up OpenPyXL**
- **Installation**: 
  ```bash
  pip install openpyxl
  ```
- **Verification**:
  ```python
  import openpyxl  # No errors = successful install.
  ```

#### **3. Reading Excel Files**
- **Loading Workbooks**:
  ```python
  wb = openpyxl.load_workbook('example.xlsx')
  ```
- **Accessing Sheets**:
  - List sheets: `wb.get_sheet_names()`.
  - Get sheet by name: `wb.get_sheet_by_name('Sheet1')`.
  - Active sheet: `wb.get_active_sheet()`.

- **Accessing Cell Data**:
  - By coordinate: `sheet['A1'].value`.
  - By row/column: `sheet.cell(row=1, column=2).value`.
  - Cell attributes: `row`, `column`, `coordinate`.

- **Handling Data Types**:
  - Dates are returned as `datetime` objects.
  - Numeric/text values are stored as-is.

#### **4. Navigating Spreadsheets**
- **Sheet Dimensions**:
  - `sheet.get_highest_row()` → Last row number.
  - `sheet.get_highest_column()` → Last column number.
- **Converting Column Letters/Numbers**:
  - `openpyxl.cell.get_column_letter(27)` → `'AA'`.
  - `openpyxl.cell.column_index_from_string('AA')` → `27`.

- **Slicing Data**:
  - Range of cells: `sheet['A1':'C3']` (returns tuples of `Cell` objects).

#### **5. Project: Census Data Analysis**
- **Goal**: Calculate population/census tracts per county.
- **Steps**:
  1. Load workbook and sheet.
  2. Initialize a nested dictionary (`countyData`).
  3. Loop through rows, update counts:
     ```python
     countyData[state][county]['tracts'] += 1
     countyData[state][county]['pop'] += int(pop)
     ```
  4. Save results to `census2010.py` using `pprint.pformat()`.

#### **6. Writing Excel Files**
- **Creating Workbooks**:
  ```python
  wb = openpyxl.Workbook()  # New workbook.
  ```
- **Modifying Sheets**:
  - Rename: `sheet.title = 'New Name'`.
  - Save: `wb.save('filename.xlsx')`.
- **Adding/Removing Sheets**:
  - `create_sheet(index=0, title='Sheet1')`.
  - `remove_sheet(wb.get_sheet_by_name('Sheet1'))`.

#### **7. Advanced Features**
- **Cell Styling**:
  - Fonts: `Font(name='Calibri', bold=True, size=12)`.
  - Apply styles:
    ```python
    style_obj = Style(font=font_obj)
    sheet['A1'].style = style_obj
    ```
- **Formulas**:
  - Assign like values: `sheet['B9'] = '=SUM(B1:B8)'`.
  - Read results: Load with `data_only=True`.

- **Adjusting Rows/Columns**:
  - Set height/width:
    ```python
    sheet.row_dimensions[1].height = 70
    sheet.column_dimensions['B'].width = 20
    ```
  - Hide: Set width/height to `0`.

- **Merging Cells**:
  - Merge: `sheet.merge_cells('A1:D3')`.
  - Unmerge: `sheet.unmerge_cells('A1:D3')`.

- **Freeze Panes**:
  - Freeze rows/columns: `sheet.freeze_panes = 'A2'` (freezes row 1).

- **Charts**:
  - Steps:
    1. Create `Reference` object (cell range).
    2. Create `Series` object.
    3. Initialize chart (e.g., `BarChart()`).
    4. Append series, set position/size, add to sheet.

#### **8. Limitations**
- **OpenPyXL 2.1.4**:
  - Does not load charts from existing files.
  - Saved workbooks with charts lose them on reload.

---

### **Key Review Questions**
1. What method retrieves all sheet names in a workbook?
2. How would you access the value of cell `C5` using both coordinate and row/column methods?
3. Explain how to convert between column letters (e.g., `'AA'`) and numbers (e.g., `27`).
4. What is the purpose of the `data_only=True` argument in `load_workbook()`?
5. How do you apply bold formatting to cell `A1`?
6. Write code to set the formula `=SUM(A1:A10)` in cell `B2`.
7. How would you hide column `D` in a worksheet?
8. Describe the steps to create a bar chart from data in cells `A1:B10`.
9. Why might you use `pprint.pformat()` when saving dictionary data to a file?

---

### **Examples**
- **Styling Cells**:
  ```python
  from openpyxl.styles import Font, Style
  font_obj = Font(bold=True, size=14)
  style_obj = Style(font=font_obj)
  sheet['A1'].style = style_obj
  ```
- **Merging Cells**:
  ```python
  sheet.merge_cells('A1:D3')
  sheet['A1'] = 'Merged cells'
  ```

---

This outline and questions cover the core functionalities of `openpyxl`, from basic operations to advanced features like styling and charts, providing a comprehensive guide for automating Excel tasks with Python.
