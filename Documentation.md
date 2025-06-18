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

1. For the purpose of learning, I want to switch to other option from balanced to recommended.
2. When I press the power button: On battery - shut down, Plugged in - shutdown
3. When I close the lid: On battery - Do nothing, Plugged in - Do nothing
4. Turn off display: On battery - 30 minutes, plugged in - 1 hour
5. Sleep after: On battery - Never, Plugged in - Never <br> 

>[!NOTE]
>Microsoft has a whole documentation for this, and asking AI might complicate the syntax, <br>
>so I would suggest going over the command line `powercfg /help` first and it will print out all the possible command line options. <br>
>then go here [Powercfg command-line options | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/powercfg-command-line-options) for more information.

# Starting with the Power Schemes

Typically, some laptops only have one option but some desktops have 2. For now we'll do two options so I can show you the changes and what command option to use.

