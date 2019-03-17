# Clear-HostLine
Clear Lines in the Host Console.  Used to REPLACE lines instead of refreshing/rerunning a function.  Gives more fluid look to your scripts.  Originally functioned in a Menu System (see my RunMahStuff repository) when switching the SuperReadKey from Single to Multiple character input.  I'd love to hear about your different uses and possibilities!

# Structure
Takes in Parameter "Count" at position 1, Finds the current cursor Y position, sets position to $currentLine -$Count and writes blank spaces for the lines being overwritten

# How to use Clear-HostLine:
This will NOT work in the ISE.  It only work in the Console.  
It would be best to call this from inside of a function that ONLY gets called when you are in CONSOLE, or call this only if you are in CONSOLE.  

**EXAMPLE**:
<pre><code>
IF ($host.name -eq 'ConsoleHost') {Put the process to be called here}
</code></pre>

**Be sure to Load up the module with**:  
<pre><code>
Import-Module $PathToModule\Clear-Hostline.psm1 
</code></pre>

**Call function when lines need to be cleared**:  
<pre><code>
Clear-HostLine -Count 3
</code></pre>

## If you want Clear-HostLine to automatically know the number of lines to clear... Use Measure-Object:
<pre><code>
Import-Module $PathToModule\Clear-HostLine.psm1

Write-Host "System Message"  
$Message = {Write-Host "This will be erased - Test Message"}  
&$Message  
$MessageLines = ($Message | Measure-Object -Line).Lines  
start-sleep 2  
Clear-HostLine -Count $MessageLines  
pause  
</code></pre>

# Quirk #1
**Description**: When using this with ScriptBlocks, be careful as to how you format them.  

In the example above, if $Message was formatted as:
<pre><code>
$Message = {  
Write-Host "This will be erased - Test Message"  
}
</code></pre>

**Problem**: $Messages would be calculated as **2** Lines

**Solution Example**:  
Format the ScriptBlocks like this:

If there is a Single Line in the ScriptBlock, format it like this:
<pre><code>
$Message = {Write-Host "This will be erased - Test Message"}
</code></pre>

If there are Multiple Lines in the ScriptBlock, format it like this:
<pre><code>
$Message = {Write-Host "This will be erased - Test Message"  
            Write-Host "This will be erased - Test Message #2"}  
</code></pre>

# Quirk #2
**Description**: You cannot use this when your cursor will end up at Y position 0  

**Problem**: Program will give an error because there will be no buffer to use and calculating anything on 0 is problematic.

**Solution Example**: When using this at the TOP of the screen, put a message at the top of the screen BEFORE your real message... and then delete that message line(s).  If you look at my example above, there is a line that writes " System Message" to the Console.  That line is there to prevent this error.  

When I first created this, my use case was to "refesh" a prompt at the BOTTOM of the screen in my "RunMahStuff" menu system... so I never had to worry about being at the TOP of the screen.
