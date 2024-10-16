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
      
- 1 .`mkdir`: Create a new directory.
      
       `mkdir` or `md` - `Make Directory`

  - `mkdir NewProject`

- 2 .`rmdir`: Delete a  directory.
      
       `rmdir` or `rm` - `Delete Directory`

  - `rmdir NewProject`
        
## Managing Files

 - 1 .`copy`: Copy one or more files to another location.
   
       `copy *.txt C:\Backup`

- 2 .`move`: Move or rename files and directories.
   
       `move oldname.txt newname.txt`

- 3.`del`: Delete one or more files.
   
       `del unwanted.txt`
       `del *.tmp` 

- 4 .`ren`: Rename files or directories.
   
      `ren oldname.txt newname.txt`

## Viewing and Editing Files
- 1 .`more`: Display output one screen at a time -Paginate Output
   
      `type longfile.txt | more`

- 2 .`notepad`: Open files in Notepad for editing.
   
      `notepad script.py`

## Running Programs and Scripts

- 1 .`exe`: Run executable files or scripts.
   
      `program.exe`

- 2 .`python`: Execute Python scripts if Python is installed.
      `python script.py`

- 3 .`deploy`: Execute batch scripts (.bat or .cmd files).

      `deploy.bat`

## Environment Variables

- 1 .`set`: Display, set, or remove environment variables.

      `set  # Display all variables`
      `set PATH  # Display PATH variable`
      `set MY_VAR=HelloWorld`

- 2 .`setx`: Set environment variables for future Command Prompt sessions.

- Set Environment Variables Permanently
      
      `setx MY_VAR "HelloWorld`

## Piping and Redirection

- 1 .`dir`: Save command output to a file.

- Redirect Output to a File

      `dir > filelist.txt`

- 2 .`echo`: Append command output to the end of a file.

- Append Output to a File

      `echo "New Line" >> filelist.txt`

- 3 .`exe`:  Use a file as input for a command.

- Redirect Input from a File

      `program.exe < input.txt`

## Piping and Redirection

- 1 .`cls`:  Clear the Command Prompt screen.
   
      `cls`

- 2 .`help`:  Get help on commands
   
      `help dir`

- 2 .`exit`:  Exit the Command Prompt
   
      `exit`

  
  
[Refer Link](https://www.markdownguide.org/basic-syntax/) - for more information abould 





  





  






