# Systems Class notes
## This file was created so that I can take notes and refer to important transactions when using the VM.

**Common commands** 

- ls - to view  files
- clear - to clear the screen
- exit to close window

The italics commands are the ones I am using often:
*1. list files and directories.................. ls*
2. print name of current/working directory..... pwd
3. create a new directory...................... mkdir
4. remove or delete an empty directory......... rmdir
*5. change directory............................ cd*
6. create an empty file........................ touch
7. print characters to output.................. echo
*8. display contents of a text file............. cat*
*9. copy a file or directory.................... cp*
10. move or rename a file or directory.......... mv
11. remove or delete a file or directory........ rm


**For adding and removing software**
sudo apt update -- searches for updates (include software name)
  This command will bring up any updates available
sudo apt upgrade -- upgrades files
  This command will update the files
apt search <name> - finds software
apt show
sudo apt install <name>   installs sofware
sudo apt --purge remove
sudo apt autoremove
sudo apt clean

** I use edit for my text editor** 
when editing file, I typically use sudo edit filename.extention

**USING GREP**
grep to search 
grep -i "searchterm" file - to look for both upper and lower case specific words
grep -Ei "(bsd|atari)" operating-systems.csv  --- boolean search OR


yaz-client - to search on libraries website, must connect via link
Z> open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY  --links to UK Library
