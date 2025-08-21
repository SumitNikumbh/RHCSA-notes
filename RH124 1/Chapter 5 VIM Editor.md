

# 1. Introduction to VIM

**VIM (Vi IMproved)** is a powerful text editor that is an improved version of the vi editor distributed with most UNIX systems. It supports multiple modes and is often used for system administration tasks.  
  
**To install vim**:  
$ sudo yum install vim -y  
  
**To open a file**:  
$ vim filename
![pitucre 1 ](./Attachments/VIMpicture1.png)

# 2. Modes in VIM

VIM operates in three primary modes:  
  
1. **Normal Mode**: Default mode. Used for navigation and operations.  
2. **Insert Mode** : Press '**i**' to enter. Used for typing text.  
3. **Command Mode** : Press '**:**' to enter. Used for commands like saving and quitting.  
  

Press `**Esc**` to switch back to Normal mode from Insert or Command Mode.

# 3. Insert Mode Commands

·       **i** → Insert before the cursor

·       **I** → Insert at the beginning of the line

·       **a** → Insert after the cursor

·       **A** → Insert at the end of the line

·       **o** → Open a new line below

·       **O** → Open a new line above

# 4. Normal Mode Navigation & Editing

·       **h** → Move left

·       **l** → Move right

·       **j** → Move down

·       **k** → Move up

·       **0** → Beginning of line

·       **$** → End of line

·       **w** → Next word

·       **b** → Previous word

·       **G** → Go to end of file

·       **gg** → Go to start of file

·       **10G** → Go to line 10

# 5. Command Mode Commands

·       **:w** → Save the file

·       **:q** → Quit

·       **:wq** → Save and quit

·       **:q!** → Force quit without saving

·       **:x** → Save and quit

·       **:set nu** → Show line numbers

·       **:set nonu** → Hide line numbers

# 6. Copy, Paste, Undo, Redo

·       **yy** → Copy current line

·       **p** → Paste below

·       **P** → Paste above

·       **u** → Undo last change

·       **Ctrl** **+ r** → Redo

# 7. Delete Commands

·       **x** → Delete character under cursor

·       **X** → Delete character before cursor

·       **dw** → Delete word

·       **dd** → Delete current line

·       **d$** → Delete till end of line

·       **d^** → Delete till beginning of line

·       **dG** → Delete till end of file

·       **:10**,**20d** → Delete lines 10 to 20

# 8. Searching and Highlighting

·       **/word** → Search for 'word' forward

·       **?word** → Search for 'word' backward

·       **n** → Next match

·       **N** → Previous match

# 9. Customization with .vimrc

To make VIM settings permanent, edit the **~/.vimrc** file and add settings like:

·       set nu

·       syntax on

·       set tabstop=4

·       set autoindent

·       set smartindent

  
