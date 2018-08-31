# Terminal Emulator Shortcuts 

* CTRL+u clears from cursor to beginning of line
* CTRL+k clears from cursor to end of line
* CTRL+d clears one character to the right of the cursor
* Esc+Backspace clears one word to the left of the cursor
* Esc+d clears one word to the right of the cursor
* Alt+left/right jumps to the beginning of the previous/next word

To clear the entire screen add the following alias to your ~/.bashrc file:

```bash
alias cls="echo -ne '\033c'"
```

Now, in a new terminal typing cls will clear everything including the scroll buffer. 
It works much faster than reset since it does not reset anything.

In fact reset is only needed when you want to fix a broken terminal, e.g. after running cat on a binary file.
If you are on OSX, then Command (⌘)+k will clear the terminal (and the chrome devtools console).
