# Windows Command Line Instructions for Developers

This guide provides a list of essential Windows command line instructions that developers commonly use in their day-to-day development tasks. It's designed for beginners who want to learn and reference these commands for future use.

## Introduction

The Windows Command Prompt is a powerful tool that allows developers to interact with the operating system through text-based commands. Mastering these commands can enhance your efficiency and enable you to automate repetitive tasks.


**1. **`cd` - Change Directory**
 
- **Usage:** Navigate between directories.
- **Example:**

- _ ```cmd
  cd C:\Projects

- **Notes:** Use cd .. to move up one directory level.

**2. **`dir` - List Directory Contents**

- **Usage:** Navigate between directories.
- **Example:**
- _ ```dir
       dir /w  # Wide listing format
       dir /a  # Include hidden files
      
  **2a. **`mkdir` or `md` - Make Directory**

  - **Usage:**  Create a new directory.

  - **Example:**
  - _ ```mkdir NewProject

   **2b. **`rmdir` or `rd` - Remove Directory**

  - **Usage:**  Delete directory.

  - **Example:**
  - _ ```rmdir NewProject
         rmdir /s /q OldProject   **Delete directory and contents quietly**


**3. **Managing Files**


  **3a. **`copy`**
   - **Usage:** Copy one or more files to another location.


   - **Example:**
   - _ ```copy source.txt destination.txt
          copy *.txt C:\Backup

  **3b. **`move`**
   - **Usage:** Move or rename files and directories.
   
   - **Example:**
   - _ ```move file.txt C:\NewFolder
          move oldname.txt newname.txt

  **3c. **`del`or `erase`**
   - **Usage:** Delete one or more files.
   
   - **Example:**
   - _ ```del unwanted.txt
          del *.tmp  # Delete all .tmp files

  **3d. **`ren`or `rename`**
   - **Usage:** Rename files or directories.
   
   - **Example:**
   - _ ```ren oldname.txt newname.txt

  **3e. **`type`**
   - **Usage:** Display the contents of a text file.
   
   - **Example:**
   - _ ```ren oldname.txt newname.txt


**4. **Viewing and Editing Files**

  **4a. **`more`** - 
   - **Usage:** Display output one screen at a time - Paginate Output
   
   - **Example:**
   - _ ```type longfile.txt | more

  **4b. **`notepad`**
   - **Usage:** Open files in Notepad for editing.
   
   - **Example:**
   - _ ```notepad script.py

**5. **Running Programs and Scripts**

  **5a. **`Executing Programs`**
   - **Usage:** Run executable files or scripts.
   
   - **Example:**
   - _ ```program.exe

  **5b. **`Running Python Scripts`**
   - **Usage:** Execute Python scripts if Python is installed.
   
   - **Example:**
   - _ ```python script.py

  **5c. **`Running Batch Files`**
   - **Usage:** Execute batch scripts (.bat or .cmd files).
   
   - **Example:**
   - _ ```deploy.bat


**6. **Environment Variables**

  **6a. **`set`**
   - **Usage:** Display, set, or remove environment variables.
   
   - **Example:**
   - _ ```set  # Display all variables
          set PATH  # Display PATH variable
          set MY_VAR=HelloWorld

  **6b. **`setx`** - Set Environment Variables Permanently
   - **Usage:** Set environment variables for future Command Prompt sessions.
   
   - **Example:**
   - _ ```setx MY_VAR "HelloWorld"

**7. **Piping and Redirection**

  **7a. **`Redirect Output to a File`**
   - **Usage:** Save command output to a file.

     - **Example:**
     - _ ```dir > filelist.txt"

  **7b. **`Append Output to a File`**
   - **Usage:** Append command output to the end of a file.

     - **Example:**
     - _ ```echo "New Line" >> filelist.txt"

  **7c. **`Redirect Input from a File`**
   - **Usage:** Use a file as input for a command..

     - **Example:**
     - _ ```program.exe < input.txt"

  **7d. **`Pipe Output to Another Command`**
   - **Usage:** Use the output of one command as input to another.

     - **Example:**
     - _ ```dir | find "keyword""

**8. **Piping and Redirection**

  **8a. **`cls`**
   - **Usage:** Clear the Command Prompt screen.
   
   - **Example:**
   - _ ```cls

   **8b. **`help`**
   - **Usage:** Get help on commands.
   
   - **Example:**
   - _ ```help dir

  **8c. **`exit`**
   - **Usage:** Exit the Command Prompt.
   
   - **Example:**
   - _ ```exit




  
    

  






  





  






