# Windows Command Line Instructions for Developers

This guide provides a list of essential Windows command line instructions that developers commonly use in their day-to-day development tasks. It's designed for beginners who want to learn and reference these commands for future use. 

## Introduction

The Windows Command Prompt is a powerful tool that allows developers to interact with the operating system through text-based commands. Mastering these commands can enhance your efficiency and enable you to automate repetitive tasks.


## Change Directory

- Run `cd #directory_path#` to change directory
 
- **Usage:** Navigate between directories.
- **Example:**

       `cd C:\Projects`

- **Note:** Use `cd ..` to move up one directory level.

## List Directory Contents

- Run `dir` to navigate between directories

- Usage: Navigate between directories and view files in directories.

       `dir`
       `dir /w  # Wide listing format` 
       `dir /a  # Include hidden files`
      
-  `mkdir`: Create a new directory.
      
       `mkdir` or `md` - `Make Directory`

  - `mkdir NewProject`

- `rmdir`: Delete a  directory.
      
       `rmdir` or `rm` - `Delete Directory`

  - `rmdir NewProject`
        
## Managing Files

 - `copy`: Copy one or more files to another location.
   
       `copy *.txt C:\Backup`

- `move`: Move or rename files and directories.
   
       `move oldname.txt newname.txt`

- `del`: Delete one or more files.
   
       `del unwanted.txt`
       `del *.tmp` 

- `ren`: Rename files or directories.
   
      `ren oldname.txt newname.txt`

## Viewing and Editing Files
- `more`: Display output one screen at a time -Paginate Output
   
      `type longfile.txt | more`

- `notepad`: Open files in Notepad for editing.
   
      `notepad script.py`

## Running Programs and Scripts

- `exe`: Run executable files or scripts.
   
      `program.exe`

- `python`: Execute Python scripts if Python is installed.
      `python script.py`

- `deploy`: Execute batch scripts (.bat or .cmd files).

      `deploy.bat`

## Environment Variables

- `set`: Display, set, or remove environment variables.

      `set  # Display all variables`
      `set PATH  # Display PATH variable`
      `set MY_VAR=HelloWorld`

- `setx`: Set environment variables for future Command Prompt sessions.

- Set Environment Variables Permanently
      
      `setx MY_VAR "HelloWorld`

## Piping and Redirection

- `dir`: Save command output to a file.

- Redirect Output to a File

      `dir > filelist.txt`

- `echo`: Append command output to the end of a file.

- Append Output to a File

      `echo "New Line" >> filelist.txt`

- `exe`:  Use a file as input for a command.

- Redirect Input from a File

      `program.exe < input.txt`

## Piping and Redirection

- `cls`:  Clear the Command Prompt screen.
   
      `cls`

- `help`:  Get help on commands
   
      `help dir`

- `exit`:  Exit the Command Prompt
   
      `exit`

  
  
<!-- [Refer Link](https://www.markdownguide.org/basic-syntax/) - for more information abould  -->





  





  






