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
      

## Advanced Navigation

- Move to the parent directory:

      cd ..

- Using absolute paths:

      cd C:\Users\YourName\Documents

- Relative paths:

      cd ..\AnotherFolder
            

## Customizing the Command Prompt

- Change text color:

      color 0A

    0A sets the background to black and text to green.

## Environment Variables

Display, set, or remove environment variables.

      set  # Display all variables
      set PATH  # Display PATH variable
      set MY_VAR=HelloWorld   # Set MY_VAR variable

## Piping and Redirection

- Redirect Output to a File

      dir > filelist.txt

- Append Content to a File

      echo "New Line" >> filelist.txt

Use a file as input for a command.

      program.exe < input.txt

## Tips and Tricks

- Tab Completion: Start typing a file or folder name and press Tab to auto-complete
- Command History: Use the ↑ and ↓ arrow keys to cycle through previous commands

## Common Errors and Solutions

- **Access Denied:** Run Command Prompt as Administrator (Right-click > Run as Administrator).
- **Unknown Command:** Ensure the command is typed correctly and the required program or script is installed.
- **File Not Found:** Double-check the file path and file name for accuracy.