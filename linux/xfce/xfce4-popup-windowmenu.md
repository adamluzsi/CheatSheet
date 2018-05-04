# /usr/bin/xfce4-popup-windowmenu

### Add items to Xfce Applications Menu

To add an application launcher to Xfce Applications Menu is simple; all you have to do is place the *.desktop file that launches the application in the right folder.

**Create the *.desktop file**
Create a text file whose extension is 'desktop', with the following content:

```DesktopEntry
[Desktop Entry]
Version=1.0
Type=Application
Name=ItemName
Exec=Command
Icon=IconFile
Categories=Category;
```

On _ItemName_ write the name that should be displayed on the menu. _Command_ is the command that should be run. _IconFile_ is the path to some *.png file. The _Category_ dictates the sub-menu where the item will be placed. See the table bellow to see what _Category_ value you should use.

| Sub-Menu    | Categories  |
| ----------- | ----------- |
| Accessories | Utility     |
| Development | Development |
| Games       | Game        |
| Graphics    | Graphics    |
| Internet    | Network     |
| Multimedia  | AudioVideo  |
| Office      | Office      |
| System      | System      |


A new item can be placed on a sub-menu by selecting the right 'Categories' value


To learn more about *.desktop files see the reference at the end of this post.

**Add an item for you user only**
Copy the *.desktop file to: $HOME/.local/share/applications

**Add an item for all users**
You'll **need root privileges** to do this.
Copy the *.desktop file to: /usr/share/applications

After you copy the *.desktop file the new item will be automatically added to the Applications Menu.
