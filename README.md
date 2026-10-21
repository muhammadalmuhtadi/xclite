# xclite

**Version 1.0 Beta** · Windows x64

xclite is a lightweight spreadsheet for opening, editing, searching, filtering, and querying huge CSV files (tested up to 20 GB and over 100 million rows), as well as Excel (.xlsx) files.

> xclite is a **fork and modification of [GridCraft](https://github.com/storytold/gridcraft) by the ArtCraft Team and GridCraft contributors**. The formula engine (501 functions), number formatting, and XLSX reader/writer come from GridCraft (MIT OR Apache-2.0) and are used unchanged. On top of these, xclite adds a viewer for huge files, column caching, fast search, filtering, and SQL.

## Download and run

1. Download `release_xclite_betav.01.x64.zip` from the [Releases](https://github.com/muhammadalmuhtadi/xclite/releases) page.
2. Extract it, then run `xclite.exe`. No installation required.

Folder contents:

| File                            | Purpose                                                                                                                                                        |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `xclite.exe`                    | The application (about 22 MB).                                                                                                                                 |
| `duckdb.dll`                    | SQL and sorting engine. Keep it in the same folder as `xclite.exe`. The application will still run without this file, but SQL and sorting will be unavailable. |
| `cache\`                        | Cache for large files (row index and column cache). It can be deleted at any time; it will be recreated when the file is opened again.                         |
| `LICENSE-MIT`, `LICENSE-APACHE` | GridCraft licenses.                                                                                                                                            |

## Why it's lightweight

- **Files are not loaded into RAM.** Files are read directly from disk (memory-mapped); only the rows visible on screen are read.
- **Read once.** The first time a large file is opened, xclite reads it sequentially once and stores a row index and column cache (compressed Parquet, around 0.3–0.4× the CSV size). Subsequent openings are immediate.
- **The SQL engine loads only when needed.** `duckdb.dll` is loaded only when you run SQL or sort data.
- **Memory-efficient.** Uses around 50 MB when first opened; SQL memory is released one minute after the last query.

Example benchmark results on a synthetic 20 GB file: first opening takes around 3.5 minutes; searching all columns takes around 2 seconds; numeric filtering takes around 0.1 seconds.

## Features

- **Spreadsheet-style editing:** type, copy/cut/paste, Paste Special, autofill, insert and delete rows or columns, undo and redo. The original file is not changed until you save.
- **Formulas:** 501 worksheet functions from GridCraft, plus number formatting.
- **Named tables:** mark a range as a table, then change its name and range through **Manage Tables**.
- **Search or Analyze:**
  - Search for text across all columns.
  - Filter rows and use advanced filters (contains, equals, starts with, empty, and numeric rules). Conditions can be combined with AND/OR.
  - SQL / queries via DuckDB. Sheet columns are named A, B, …; each table becomes a view using its own column names.
- **Chained results:** each filter or query result opens in a new tab and automatically becomes a table. That table can be filtered or queried again, and joined with tables in other tabs.
- **Data preparation and formatting:**
  - number formatting;
  - **data types** per column (integer, decimal, date and time), so queries such as `status >= 2` work;
  - transform text with regex (remove or replace, with a live preview);
  - find and replace, and clean data.
- **Save** to CSV or XLSX.

## License and attribution

- **GridCraft:** © ArtCraft Team and GridCraft contributors, MIT OR Apache-2.0, https://github.com/storytold/gridcraft (commit `66ce6be`).
- **DuckDB** (`duckdb.dll`): MIT, https://duckdb.org
- egui/eframe, arrow/parquet, and other Rust crates: MIT and/or Apache-2.0.

## Author

**M Al Muhtady**

- Email: muhamadalmuhtadi@gmail.com
- GitHub: https://github.com/muhammadalmuhtadi
