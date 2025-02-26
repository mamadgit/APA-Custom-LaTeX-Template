# Instructions
For sections of code that require manual setup, this document will offer precise instructions on what to do.

**Instructions**:
1. **Replace the original `biblatex.def`**: Locate the original `biblatex.def` in your installation path and replace it with the modified `biblatex.def` presented on this repository.
2. **For MikTeX Users**:
   - Navigate to your user-specific MikTeX installation directory:
     - On Windows, this is typically found in:
       ```
       C:\Users\[Your Username]\AppData\Local\Programs\MiKTeX\tex\latex\biblatex\biblatex.def
       ```
   - Here, `[Your Username]` represents your actual Windows username.
   - Copy the original `biblatex.def` from your source or backup location to this path.
   - Ensure you have administrative privileges to write to this directory or run your text editor as an administrator.
     
Or if you'd prefer another method:
- Copy the contents of `APA-Custom-Template.def` **from line 5 to line 16**. Locate the original `biblatex.def` in your installation path and add the copied content to the macro `DeclareDriverSourccemap` on **line 1339**.

Remember, modifying system files like `biblatex.def` can affect other LaTeX documents you work on, so backup your current `biblatex.def` before making changes.
