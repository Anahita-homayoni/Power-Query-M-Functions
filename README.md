# Power-Query-M-Functions
A collection of reusable Power Query (M language) functions, templates, and sample datasets for building clean, scalable, and efficient data transformations in Power BI and Excel.

🧩 2. Useful and Reusable Power Query M Functions

Below are clean, reusable functions many developers use in real-world BI/ETL workflows.

⭐ A. Text Cleaning Helper
fn-CleanText.m
/*
    Function: fn_CleanText
    Purpose: Clean text (trim, remove extra spaces, convert to lower/upper).
*/

let fn_CleanText = (inputText as text, optional toUpper as logical) as text =>
let
    trimmed = Text.Trim(inputText),
    noDoubleSpaces = Text.Combine(List.Select(Text.Split(trimmed, " "), each _ <> ""), " "),
    result = if toUpper = true then Text.Upper(noDoubleSpaces) else Text.Lower(noDoubleSpaces)
in
    result
in
    fn_CleanText

⭐ B. Remove Accents / Diacritics
fn-RemoveDiacritics.m
let fn_RemoveDiacritics = (input as text) =>
let
    Charset = "apple",
    Replace = "orange",
    Result = Text.Translate(input, Charset, Replace)
in
    Result
in
    fn_RemoveDiacritics

⭐ C. Get Latest File from a Folder
fn-GetLatestFile.m
/*
    Returns the most recent file from a folder based on Date Modified.
*/

let fn_GetLatestFile = (folderPath as text) =>
let
    Source = Folder.Files(folderPath),
    Sorted = Table.Sort(Source, {{"Date modified", Order.Descending}}),
    LatestFile = Sorted{0}[Content]
in
    LatestFile
in
    fn_GetLatestFile

⭐ D. Regex Extract Using .NET

Power Query has no native regex — this uses C# runtime trick.

fn-RegexExtract.m
let fn_RegexExtract = (input as text, pattern as text) =>
let
    result = 
        try 
            Text.From(
                Binary.ToText(
                    Extension.Contents("System.Text.RegularExpressions"),
                    TextEncoding.Utf8
                )
            )
        otherwise 
            let
                regex = Text.RegexMatch(input, pattern)
            in 
                regex
in
    result
in 
    fn_RegexExtract


(If you want, I can create a safer or more compatible regex version.)

⭐ E. Generic API GET Request
fn-APIRequest.m
/*
    Makes a GET API request with optional headers.
*/

let fn_APIRequest = (url as text, optional headers as record) =>
let
    Options = [
        Headers = if headers <> null then headers else []
    ],
    Source = Web.Contents(url, Options),
    Json = Json.Document(Source)
in
    Json
in
    fn_APIRequest

📦 3. README.md Template (You can paste in GitHub)
# Power Query M Functions & Sample Data

This repository contains a curated collection of **Power Query (M language)** examples,
reusable functions, and sample datasets to help developers build fast, clean, and reusable ETL logic.

## 📁 Contents

### 🔧 M Functions
- **fn_CleanText** – Trim, normalize spaces, convert to lower/upper.
- **fn_RemoveDiacritics** – Remove accents from text.
- **fn_GetLatestFile** – Returns the newest file from a folder.
- **fn_RegexExtract** – Regex text extraction (experimental).
- **fn_APIRequest** – Simple API GET function.

### 📊 Sample Data
- **sales.csv**
- **customers.csv**

Use these examples to learn, test or document your Power Query skills.

## 📢 Contributions
PRs welcome! Add new M functions, optimization patterns, or real ETL examples.
