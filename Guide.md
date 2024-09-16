# Using `Minimize to Tray`[^1]

To start an app automatically, (in this case for demonstration purposes) Aria2 (download utility), one can add a basic task in task scheduler, give it a name like _aria2 autostart_, set the trigger to "when I log on", set the action to "start a program", set the program to cmd.exe (full path `C:\Windows\System32\cmd.exe`), and the arguments is where the magic happens:

```sh
/c cd /d "C:\path\to\aria2" && start "Aria2 dld" /min aria2c.exe --conf-path=aria2.conf && start /b /wait timeout 5 /nobreak > nul && start /b minimize-to-tray /nonotify "Aria2 dld"
```

### Overview (breakdown) of the parameters (part I)

#### `/c` 

closes the command console (`cmd`) window upon completion of the commands.  

#### `cd /d "C:\path\to\aria2"` 

**`cd`**changes directory to "`c:\path\to\aria2`", and  **`/d`**  changes the drive as well if needed.  

#### `&&` 

chains together additional commands.  

#### `start "Aria2 dld"`

starts aria2c.exe with the title _Aria2 dld_, 

#### `/min aria2c.exe` 

minimizes it, 

#### `--conf-path=aria2.conf` 

are command parameters specific to aria2c.exe (not related to minimize-to-tray)
<br>

>So far a command prompt window has opened, changing the directory from which the app is launched, and the app is launched minimized.

### Breakdown of the above command (part II)

Here's the part that was a bit of a struggle to figure out:

```sh
&& start /b /wait timeout 5 /nobreak > nul && start /b minimize-to-tray /nonotify "Aria2 dld"
```

#### `start /b` 

launches a program without opening a window. 

#### `/wait`

The most important part is the `/wait` parameter which makes the program wait until the following command has finished. 

#### `timeout 5 /nobreak`

minimize-to-tray waits 5 seconds and the `/nobreak` parameter makes the pause un-skippable. 

#### `> nul` 

prevents printing the timeout command msg, but is probably unnecessary because the command is launched windowless anyway.

####  `/nonotify`

without notifying windows that it has minimized the window

##### `&& start /b minimize-to-tray /nonotify "Aria2 dld"` 

starts minimize-to-tray windowless (`/b` parameter) and without notifying windows that it has minimized the window (`/nonotify`) labelled _Aria2 dld_, which was named earlier with the `start "Aria2 dld"` command.

Obviously for this to work as it's written here the path to minimize-to-tray.exe has already been added to PATH, which makes cmd.exe able to run it from any directory. Alternatively one could link the direct path to it like "`C:\path\to\minimize-to-tray\minimize-to-tray.exe`", or add `minimize-to-tray.exe` to the same folder containing  the app you want to run.

Here's a video illustrating the process, including setting up an environment variable (adding minimize-to-tray to the `$path` variable in windows 10 (or 11):

 ![Video Tutorial](./video_guide.mp4)

 [^1]: Thanks to [@RedSnt](https://github.com/RedSnt) for providing this tutorial in an issue submitted to the original (parent) repository at [Issue #1](https://github.com/danielgjackson/minimize-to-tray/issues/1)
