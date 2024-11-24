# Essential Guide to Windows Command Line Basics

The Command Prompt (CMD) is a command-line interpreter application available in most Windows operating systems. It lets you interact with your computer using text-based commands.

Benefits:

- Faster navigation and file management.
- Access to advanced system tools.

## Getting Started

Open the Command Prompt:

- Press Windows + R, type cmd, and press Enter.
- Search for "Command Prompt" in the Start Menu.

## Basic Commands

- Lists the contents of the current directory.

      dir 
      dir /w  #Wide listing format
      dir /a  #Include hidden files

- Change the current directory

      cd ~/Documents

_Note:_ Use `cd ..` to move up one directory level.

- Clears the screen.

      cls

## Working with Files and Directories

- Creates a new directory.

      mkdir MyNewFolder

- Delete a  directory

      rmdir MyNewFolder

- Copy one or more files to another location.

      copy *.txt C:\Backup

- Move or rename files and directories.

      move oldname.txt newname.txt

- Delete one or more files

      del file.txt

- Rename files or directories

      ren oldname.txt newname.txt
      

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
