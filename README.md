# Clear-HostLine
Clear Lines in the Host Console.  Used to REPLACE lines instead of refreshing/rerunning a function.  Gives more fluid look to your scripts.  Originally functioned in a Menu System (see my RunMahStuff repository) when switching the SuperReadKey from Single to Multiple character input.  I'd love to hear about your different uses and possibilities!

# Structure
Takes in Parameter "Count" at position 1, Finds the current cursor Y position, sets position to $currentLine -$Count and writes blank spaces for the lines being overwritten

# How to use Clear-HostLine:
This will NOT work in the ISE.  It only work in the Console.  
It would be best to call this from inside of a function that ONLY gets called when you are in CONSOLE, or call this only if you are in CONSOLE.  
EXAMPLE: IF ($host.name -eq 'ConsoleHost') {Put the process to be called here}

Be sure to Load up the module with:  
Import-Module $PathToModule\Clear-Hostline.psm1 

Call function when lines need to be cleared:  
Clear-HostLine -Count 3

## If you want Clear-HostLine to automatically know the number of lines to clear... Use Measure-Object:

Import-Module $PathToModule\Clear-HostLine.psm1

Write-Host "System Message"
$Message = {Write-Host "This will be erased - Test Message"}
&$Message
$MessageLines = ($Message | Measure-Object -Line).Lines
start-sleep 2
Clear-HostLine -Count $MessageLines
pause
