```markdown
---
title: 'BibLaTeX Template for Automating Bibliography Data for the APA Referencing Style'
tags:
  - LaTex  
  - BibLaTex
  - BibTeX
  - APA
  - expl3
authors:
  - name: Amir Mohammad Tahsiri
    orcid: 0009-0007-3230-4147
    equal-contrib: true
    affiliation: "1
affiliations:
 - name: Amir Mohammad Tahsiri
   index: 1
date: 26 Febuary 2025
bibliography: BibLaTeX Template for Automating Bibliography Data for the APA Referencing Style.bib

# Summary
This BibLaTeX-based LaTeX template offers a robust solution for APA (7th edition) referencing with numeric in-text citations, a format required by some academic journals. It ensures strict compliance with APA standards while minimising manual effort through intelligent automation of unorganised bibliography entries. The template is compatible with MikTeX for Windows users and other LaTeX distributions.
What distinguishes this template from existing LaTeX solutions for APA referencing is its capacity to manage unorganised entries. It automatically extracts essential information from bibliography files and formats it according to APA guidelines. Furthermore, it distinguishes between variations of entry types—such as proceedings papers (presented, published in journals, or books)—all identified by `@inproceedings`, using targeted checks. These features are especially valuable for users managing large reference lists who need precise formatting.

# Statement of need
LaTeX users typically integrate references into their documents by downloading BibTeX citations and adding them to a `.bib` file. However, these entries are often incomplete or unorganised: a single field may contain data for multiple fields, while others may omit critical information. For instance, a proceedings paper (`@inproceedings`) from DBLP—a reputable and widely used source—provides comprehensive bibliography data, often more complete than official publisher sites. Yet, its booktitle field typically bundles information for other fields, such as the conference `location` and `date`.
To the developer’s knowledge, even the most comprehensive existing APA referencing packages, such as the standard BibLaTeX APA package (@latexstandard) or Springer Nature’s BibTeX template (@springernature), lack automated identification and extraction of bundled fields to reorder them correctly. Instead, they print the entire contents of each field consecutively. The developer’s tests reveal that these packages also output lengthy fields, such as `abstract` or `note` field, verbatim. Moreover, even when fields like location or date are separate, existing tools fail to recognize them. For example, if a proceedings paper published in an edited book lacks a critical field like `editor`, current packages do not flag the issue or prompt users for corrections, simply reproducing the entry unchanged.

Another critical need this package addresses is its ability to differentiate subtypes of a specific entry type. For instance, it can distinguish whether a proceedings paper is a presentation, published in a journal, or included in a book, despite all sharing the `@inproceedings` identifier. Without this package, users would need to manually identify these distinctions. Unlike other tools, this package automates this process. Additionally, when missing fields render subtype recognition impossible, requiring manual retrieval from online sources, the package ingeniously relies on easily accessible fields—like the URL—to distinguish between entry subtypes, simplifying the process for users.

# Implementation
The main algorithms that make this package unique and require explaination are the document command `\ExtractFieldInfo` and the bibliography driver for `inproceedings` explaining the extraction of bundled fields and differntiating between subtypes of an entry repsectively.
The `\ExtractFieldInfo` macro (line 246) takes an input field and splits it into a sequence of items based on commas (lines 248-249). The main processing loop (\l_my_continue_bool) then iterates through this sequence from right to left (lines 254–341). This direction for iteration is a delicate matter: since the conference title may contain commas as punctuation (not separators). Starting from the right ensures that the title is identified correctly after other fields (e.g., date and location, etc) are extracted. By prioritizing the right-most items, the algorithm avoids misinterpreting the commas that are parts of the title as separate fields.
At the start of the loop (line 258), a regex checks the last item in the sequence for a specific token string (pattern): a four-digit string (e.g., a year) preceded by at least one non-digit character {.\d{4}\Z}. If this token string is found, it indicates that the year is embedded within the title (e.g., “title 2025”). The algorithm wraps this year (and its acronym) in parentheses and sets the remaining items as the conference title (lines 260–267). Such a token string is considered unique amongst other possible list items (i.e., stand-alone year) and by taking advantage of this item’s unique token string, the algorithm can avoid unnecessary searches through the entire list, efficiently isolating the title and saving computational effort.
If the last item does not match this pattern, the algorithm checks if the item in the list matches the string “proceedings” (lines 270–275). If so, it discards (pops) this item, as it is not relevant to the title, date, or location. Next, the algorithm uses another regex to check for a stand-alone year {^\d{4}} (with no preceding characters) in the current item (line 277). If found, the year is discarded (line 280), and the next item is checked for a month (e.g., “January”) to store in the date variable (lines 281–283).
However, if a stand-alone year is not found, the algorithm does not check for other items and returns to the loop start as the title is the only remaining item.
If a date is identified, the next item(s) are assumed to be the location. However, location fields can vary: they typically include two items (city and country), but if the country is ‘USA’, a third item (state) is present. To handle this, the algorithm checks if the first location item is ‘USA’ (line 287). If so, it uses a nested while loop to extract the next two items (city and state); otherwise, it extracts only one item (city) (lines 287–305). These items are stored in a temporary sequence \l_my_location_list_seq from left to right to preserve (first out, first in) their original order (lines 284–286). The nested loop ensures all location items are accounted for before exiting (lines 306–314).
After processing the location, the algorithm returns to the main loop and rechecks for the embedded year pattern (line 258). If no special conditions remain, it builds the title by adding items to \g_my_title_tl variable from left to right, separating them with commas and spaces (lines 325–329). This ensures the title is formatted consistently, even if it contains internal commas. Finally, the algorithm checks if the input sequence is empty to exit the loop or continue processing (lines 330–340).
Now regarding the bibliography driver for `inproceedings`. The driver examines whether the editor field is undefined. If so, it searches for the biburl field. If that is undefined, it looks for an isbn field. In the rare case that none of these fields are defined, it prints a message prompting the user to retrieve the biburl from the URL (address bar) of its online source (lines 494–499).
If the entry includes an isbn field, we can confirm the proceedings paper is part of an edited book. In this case, the driver proceeds to execute the EditorMode function (line 500). The EditorMode function formats the output for references within an edited book. A potential question arises: if the editor field is undefined at this stage, what does it mean to execute a function designed for printing the edtior field? The EditorMode function addresses this by outputting “Find editors from publisher’s source!” if the editor field is absent.
Next, the driver handles cases where the biburl field is defined. It employs a developer-defined
function, ExtractURLInfo, which uses a regex command to scan for terms like article or journals. If a match occurs, we can confidently classify the paper as a journal article (lines 504–512). If no match is found—indicating terms like conference, proceedings, or chapter—and the editor field is absent (with incomplete data already accounted for), the paper is definitively a paper presentation (lines 513–515).
The driver then addresses scenarios where the editor field is defined. It checks if both biburl and isbn are undefined. If so, it prints “Find URL from online source!” However, if isbn is defined, we can confirm the paper belongs to an edited book, and the driver prints using EditorMode (lines 512–516). When biburl is defined, the process mirrors the earlier logic (lines 528–532). Although, this time if the paper is not a journal article, it is part of an edited book, and EditorMode is applied (line 533).
## Summary of Key Innovations
Regarding the `ExtractFieldInfo` algorithm the key innovations are:
- Splitting the input into a sequence variable (\l_my_input_seq) using commas as separators. 
- Iterating from right to left to prevent misinterpreting commas within the title.
- Utilising a unique regex identifier for an embedded year (e.g., “title 2025”) to potentially eliminate additional conditional checks, enhancing algorithm efficiency.
- Popping the sequence’s first item to check for a possible proceedings element.
- Skipping checks for date and location if no standalone year is present, returning to the loop’s
start since the remaining items belong to the title.
- Rebuilding the title in \g_my_title_tl from scratch (left to right) if no embedded year is found, due to the first item being ‘popped’ from the sequence which would belong to the title if
proceedings or (stand-alone) year were not found.
Regarding the bibliography driver for `inproceedings` the key innovations are:
- Purposely conducting checks with disparate field (e.g., isbn check when no editor present or journal article check when edtior present) to detect missing critical fields or excessive ones, prompting the user to ensure the omission is addressed.
- Deliberately using the isbn and biburl fields to mitigate issues caused by potentially missing data in identifying subtypes of proceeding papers, as it is improbable that both fields are absent.
- Using the uniqueness of the biburl, as it reliably includes a clear key identifier of the paper type. 
- Should the user have to supply missing fields, the biburl field (from the online address bar) is the easiest to provide, aligning with the package’s automation goal.

# References

```