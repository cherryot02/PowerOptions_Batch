# Power Options Adjustments 🔋
The main purpose of this simple project was to streamline the process of adjusting the power options in the Control Panel. <br><br>
The company I worked at had me deploy multiple computers and I had to manually adjust the power options everytime. <br>
I am also aware that there's an executable file called NoSleep somewhere in the internet but just for learning purposes, I made a batch file instead and see if it works. (It does).

## Plan
We will be using `powercfg` , basically short for Power Config (confirguration)

Depending on the PC, PC brand and whatever your organization has, you might have 2 or more options for power schemes. <br>
In my case there's two, one for balance and one for recommended.<br>

A. Create a .bat file using NotePad with the command line syntaxt for `powercfg` .<br>
B. Get the keys and GUID alias needed from either the command line or the registry. In this case, we don't need to open the registry.<br>
C. Study the current Power Options of your PC so you can tell what has changed and what needs to be changed. <br>
D. Get the proper syntax for the command and then apply the changes for the following:

>[!IMPORTANT]
>Before adjusting any options, you have to select which power schemes first. <br>
>Else, it will overwrite the adjusted to the default options for that particular scheme. <br>

1. For the purpose of learning, I want to switch to other option from balanced to recommended. [skip](https://github.com/cherryot02/PowerOptions_Batch/blob/main/Documentation.md#switching-power-schemes)
2. When I press the power button: On battery - shut down, Plugged in - shutdown [skip](https://github.com/cherryot02/PowerOptions_Batch/blob/main/Documentation.md#pressing-the-power-buttons)
3. When I close the lid: On battery - Do nothing, Plugged in - Do nothing
4. Turn off display: On battery - 30 minutes, plugged in - 1 hour
5. Sleep after: On battery - Never, Plugged in - Never <br>

>[!NOTE]
>Microsoft has a whole documentation for this, and asking AI might complicate the syntax, <br>
>so I would suggest going over the command line `powercfg /help` first and it will print out all the possible command line options. <br>
>then go here [Powercfg command-line options | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/powercfg-command-line-options) for more information.

# Starting with the Power Schemes

Typically, some computers only have one option but some have 2. For now we'll do two options so I can show you the changes and what command option to use.

Basically these right here and the "power schemes."<br>
![image](https://github.com/user-attachments/assets/f1c05e07-c4b0-4c6f-ab54-c94a5a7867ab)
<br>
On the photo, it already selected on the recommended but for the next example, let's pretend it's in balanced then we're switching to the recommended options.<br>
To do that, we have to open the command prompt first and run `powercfg /help` or `powercfg /?`. Which will show all the possible options for the powercfg command. <br>
Then, use `powercfg /list` or `powercfg /l` to list all the powerscheme options including their "keys". Whichever has the * means it's the one that's active. <br>
Alternatively, we can do `powercfg /getactivescheme` but that will only show the active one and not the others. We need the other keys because we are switching.<br><br>

Open a notepad and paste the long hexadecimal number, also are the HKEYS, on the recommended scheme. We will be using that to set a variable later.

# Batch file
Before doing anything else, save the notepad as a batch file. To do that, just File > Save As > filename.bat > Save. I named mine as "power.bat"<br>
I would save it on the desktop for easy access. After saving the file. Close it out and look for the file you just save. It should have some gears in a window icon. 
![image](https://github.com/user-attachments/assets/0c6f46b4-e189-42b5-90f9-1c4067537cc1)<br><br>

We are not done yet, we just made the file, we still have a lot to do. <br>
To edit the file, do not double -click on it yet because that will run the batch file. It won't really do anything since we only halve the key in there --anyways-- right-click on the file and select Edit in Notepad. <br>
Now we're are back in and time to the actual file.

## Syntax
start the file with the following:
```
@echo off
setlocal

:: comments after two colons
:: we will be putting the commands in this nest.

endlocal
```
To explain a little bit more. `@echo off` is used to tell the computer that this is a batch file and to not repeat or display the the following texts/lines after that.<br> <br>
setlocal and endlocal are also commancds acts more like a nest for the commands that we will be running, in this case `powercfg`.<br>
I am aware that syou don't always have to do this but we were taught in my courses that this is a safe way to do so it creates an local environment in the PC when you run the file, in cases where the file is disabled or what, it can go back to the state when we didn't run it. <br>

## Switching Power Schemes
We will now start with switching the power schemes from balanced to recommended. Remember that you already pasted the key earlier so just replace the text after the "=" below. <br>
or just run the `powercfg /list` again and note the key.

```
@echo off
setlocal

:: Set the active scheme if not set
SET GUID= scheme_guid (the hexnumber or the GUID)
powercfg /s %GUID%


endlocal
```
`SET` here is basically a command for creating variables. Because I did not want to copy/paste a long hex number multiple times down the line, I'll make a variable called GUID and just use `%GUID%` instead of the long number. Don't get me wrong, you can! but I just prefer this way for a cleaner look. This is just like math variables where you give a value to x so x = GUID and to use variables in a batch, you have to enclose them with %. like %x% <br><br>

After that, you can test the file by closing out and double-clicking on it. Look at the Control Panel Power Options window and see if it moved. If so, then let's proceed!
<br>

## Pressing the Power Buttons
Depending on what you want, you can set this to sleep, hibernate or shutdown. I want it to shut down when I press the power button on both on battery and when plugged in.
We will add the following commands to do so;
```
powercfg /setacvalueindex %GUID% SUB_BUTTONS PBUTTONACTION 003
powercfg /setdcvalueindex %GUID% SUB_BUTTONS PBUTTONACTION 003
```
So let's break it down:
1. powercfg the main command
2. /setacvalueindex - basically the "plugged in" part.
3. /setdcvalueindex -  the "on battery"
4. %GUID% - the power scheme as a variable
5. SUB_BUTTONS - GUID_Alias for the the dropdown menu, we could have used the hex but using the alias can also work.
6. PBUTTONACTIONS - GUID_Alias for which power setting, this one is for the power button.
7. 003 - the value for setting it on shutdown.

So where did I find this? Open the commandline and and run `powercfg /q` and it will display all the the options and settings, including the GUID, alias , and what value to use. <br>
I did find the command prompt does not display the action alias for powerbutton actions and lid close action, so I searched for Documentations and found them all here: [Power button action | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/customize/power-settings/power-button-and-lid-settings-power-button-action)









