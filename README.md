# APA Numeric Citation Template (APA 7th Edition)
A LaTeX template for APA-style referencing with numeric in-text citations, fully compliant with APA 7th edition standards and featuring advanced automation for citation data processing.

## Note
For information regarding the details and purpose of the branches of this repository, please read the `Instructions-Descriptions.md` markdown file. Also, to see the output and capabilities of this package please look at the `APA-Custom_LaTeX-Template.pdf` as an example.

## Introduction
This LaTeX template, written using BibLaTeX, provides a reliable solution for APA-style referencing (7th edition) with numeric in-text citations, a format required by some academic journals. Designed for researchers, students, and academics, it ensures strict adherence to APA standards while significantly reducing manual effort through intelligent automation regarding unorganised bibliography data. It is compatible with MikTeX installations for Windows users, as well as other LaTeX distributions.

## Key Features
- **Full APA 7th Edition Compliance**: Accurately implements all guidelines of the APA 7th edition for various entry types such as articles, books, conferences (session and proceedings), and theses (PhD and masters).
  
- **Advanced Automation**: Includes an algorithm that parses and extracts citation data from `.bib` files, automating the handling of incomplete or misstructured entries:
  - Extracts and formats conference details (e.g., title, location, date) embedded in fields like `booktitle`.
  - Correctly places extracted data in the appropriate fields, minimizing manual editing.
- **Designed for Academic Use**: Tailored for journals requiring numeric APA citations, offering a time-saving solution for academic writing.
  - **Note:** Users wishing to use the standard APA style can do so with a few simple modifications, as explained in the `.tex` file.
- **Seamless Integration**: Works effortlessly with LaTeX projects using BibLaTeX.

## Why This Template?
- **Standout Automation Feature**: Intelligently processes citation data from `.bib` files, automating the handling of incomplete or poorly structured entries, significantly reducing the need for manual field adjustments.<br>Other widely used APA referencing packages, such as BibLaTeX's default APA style and an APA package provided for Springer Nature publications (written in BibTeX with no support for numeric in-text citations), lack this level of automation. For example:
  - They may overdeliver by printing unnecessary fields, such as a paragraph-long note field in an article entry.
  - They are unable to identify and redistribute misstructured data, such as a `booktitle` field in a conference entry that may contain multiple elements (e.g., conference title, location, date), leaving users to manually correct these issues.
  - The cannot distinguish between subtypes of a particular entry such as `@inproceedings` as a paper may be a presented, published in a journal or a chapter in an edited book and yet they all have the same key identifier in their bibliography entry.
    
- **Strict APA 7th Edition Compliance**: Adheres to the strict guidelines of the APA 7th edition, including correct order and punctuation. To the observations and tests of this developer, other packages have notable flaws in this regard, such as:
  - Missing "&" for two or more authors in the Springer Nature's APA package.
  - Printing duplicate "http" prefixes in URLs or DOIs in BibLaTeX's default APA style.
  - Including unnecessary fields by APA standards such as paragraph-long notes in both APA packages (BibLaTeX's default and Springer Nature), leading to non-compliant output.
- **Superior Solution for APA Referencing**: By addressing these automation and compliance issues, this template provides a superior solution for accurate APA referencing, reducing manual effort and ensuring consistency across academic writing.

## Installation and Setup
1. Clone this repository: https://github.com/mamadgit/APA-Custom-LaTeX-Template.git
2. Include the template in your LaTeX project:
- Add the provided `.bbx` and `.dbx` files to your project directory.
- Configure BibLaTeX to use this style in your `.tex` file (according to the `.tex` provided).
3. Compile your document using a LaTeX editor (e.g., Overleaf, TeXShop) with BibLaTeX enabled.

## Contributing and Feedback
This project is open to contributions! If you have suggestions, bug reports, or feature requests, please open an issue or submit a pull request. Feel free to star this repository if you find it useful!
