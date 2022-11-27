# MsxCompilerV4
<h3>Works on Windows and Mac OS. Write your code in BASIC (very similar to the Basic MSX but with some differences) and the compiler will create an assembly file that will go directly on .ROM or .DSK, to try on the emulator or real MSX.
First select the emulator path.


![screenshot3](https://user-images.githubusercontent.com/118820016/203636763-f5b9a3da-550d-4ac0-a9c8-87012059e98a.png)


Then open a project "*.msxproj" or create one yourself.
When you are ready, press "F5" to compile and RUN!
In the same folder you will find the ".DSK" or ".ROM" file, and ".ASM" source file if you have selected the option in the settings!
</h3>

<H1>How it's work</H1>
<h3>The compiler read BASIC code and translate it in ASSEMBLER, then translate it in BINARY code
    <br>
You can save and see ASM file if you want, select relative option in Settings window</h3>
<h3>if the binary code resulting from the compilation is greater than 16kb, then you will have to divide your project into more than one file, each with a maximum of 16kb of binary code, up to a maximum of 16 files.
    It is possible to make calls between files with the instructions CALLPAGE (as gosub) and JUMPPAGE (as goto) </h3>
<h3>example: CALLPAGE 1,100 --> make a calling to page 1 at line 100</h3>
<h3>Look at example folders to see how it's work</h3>
<h3>You can select TARGET of your compilation (CARTRIDGE or DISK)</h3>
<h3>Depending on the TARGET (cartridge or disk), a .ROM file of a maximum of 256kb or a .DSK
    file containing up to a maximum of 16 binary files of 16kb each will be created!</h3>

<h4>
    <ul>
<li>Memory containing the binary code (4000h-7fffh)  with memory mapper to change page</li>

<li>Memory containing the variables   (8000h-bfffh)</li>
<li>Reserved memory                   (c000h-cb2fh)</li>


<li>Additional memory available from cb30h onwards.
(Note that the stack pointer is initialized to d800h)</li>

<li>Stack                             (d800h) but you can change it in Settings window</li>
</ul>
</h4>

<hr>

<H1>Instructions and syntax</H1>

<h3>
The supported variables are:</h3>
<h3>
<ul>
<li>UBYTE (8 bit unsigned, ex: DIM A AS BYTE)    occupies 1 byte in memory</li>
<li>INT (16 bit, ex: DIM B AS INT )              occupies 2 bytes in memory</li>
<li>FLOAT (32 bit, ex: DIM C AS FLOAT)           occupies 4 bytes in memory</li>
<li>STRING (16-256 byte, ex: DIM D AS STRING)    length in memory depends on the #lenstrings command</li>
<li>ARRAY (ex: DIM E(10) AS INT) make 10 elements, not 11</li>
</ul></h3>
<h3>Every variable must be declared with DIM before use it</h3>



![screenshot2](https://user-images.githubusercontent.com/118820016/203424623-6bc876ef-61fd-42c5-8d8e-f555e0363de0.png)


<h2>Supported instructions:</h2>

<h3>#lenstrings, #Imports, #SetVarAddress, IncludeFile, CallPage, JumpPage, AsmLine, Goto, Gosub, Return, Dim, Cls,GetChar, GetKey,
Screen, Set page, Setpage, Draw2Block, DrawBlock, DrawNumber, AddNumber, GetAddressLine,
GetDate, GetTime, GetFiles, GetFileName, SetPalette, SetVDP, Color, Color=,Locate,Print,Stick,Strig,
Pset, Circle, Paint, Line, Copy, Vpoke, Poke, Vpeek, Peek, PutSprite, Put Sprite, DataB,
DataF, DataI, DataS, ReadB, ReadF, ReadI, ReadS, Restore, DrawText, if...then...else,
for...next, GetPoint, SetScroll, Sound, SaveData, LoadData, LoadDataToVram, SaveDataFromVram, CopyRamToVram, CopyVramToRam,
ReadSector, WriteSector, GetCollision, SetAttrSprites, SetColorSprite, SetSpriteAddress, DrawAttrSprites, DrawColorSprites, DrawPatternSprites,
SetVarsPage</h3>

<h3>(In if...then use { } with boolean operator. ex: If a=1 And {b=2 or c=3} Then ... )</h3>


<h2>Other functions:</h2>

<h3>TAN, ATN, COS, SIN, SQR, ABS, INT, RND, ASC, VAL, CHR, STR, HEX, INP, VDP, PAD</h3>


<h2>Operators for math operations:</h2>

<h3>
| = or<br>
& = and<br>
~ = xor<br>
% = module<br>
</h3>

<hr>

<H1>New instructions of this BASIC version</H1>

<h2><font color="blue"> #lenstrings</font></h2>

<h3>Set length of string. His value can be 15,31,63,127 or 255.</h3>
<h3>Example: #lenstrings=31</h3>

<h2><font color="blue"> #SetVarAddress</font></h2>

<h3>Set the initial memory address of the variables</h3>
<h3>Example: #SetVarAddress=$9000</h3>

<h2><font color="blue">#Imports</font></h2>
<h3>adds a new page to the project, which can be reached with the CALLPAGE or JUMPPAGE instructions</h3>
<h3>Example: #imports 1,"part2.msxproj"</h3>
<h3>Now you can use CALLPAGE and JUMPAGE</h3>

<h2><font color="blue">CallPage</font></h2>
<h3>makes a call to the specified page and program line (or label), as a Gosub instruction</h3>
<h3>Example: CallPage 1,100</h3>

<h2><font color="blue">JumpPage</font></h2>
<h3>makes a jump to the specified page and program line (or label), as a Goto instruction</h3>
<h3>Example: JumpPage 2,mylabel</h3>

<h2><font color="blue">AsmLine</font></h2>
<h3>Insert an assembly instruction</h3>
<h3>Example: AsmLine "LD A,10"</h3>

<h2><font color="blue">GetChar()</font></h2>
<h3>Read the Keyboard and return the ascii code corresponding to the key pressed</h3>
<h3>Example: a=GetChar()</h3>

<h2><font color="blue">GetKey(KeyCode)</font></h2>
<h3>Read the Keyboard and return 0 (key not pressed) or 255 (key pressed)</h3>
<h3>Example: b=GetKey(KeySpace)</h3>
<h3>See all key codes <a href="#keycodes">here</a></h3>

<h2><font color="blue">GetAddressLine</font></h2>
<h3>Get the memory address of line or label specified and return it to the specified variable</h3>
<h3>Example: GetAddressLine id,1000 (now the variable id contains memory address of line 1000)</h3>

<h2><font color="blue">DrawBlock ram, vram, lines, wram</font></h2>
<h3>Copy a block of memory from RAM to VRAM</h3>
<h3>Example: DrawBlock 0, 0, 10, 32</h3>
<h3>(See some examples in the Example folders)</h3>

<h2><font color="blue">Draw2Block ram, vram, lines, wram, wvram, addvram</font></h2>
<h3>Copy a block of memory from RAM to VRAM, it is also possible to specify 2 additional parameters</h3>
<h3>Example: Draw2Block 0, 0, 10, 32, 20, 40</h3>
<h3>(See some examples in the Example folders)</h3>

<h2><font color="blue">GetDate</font></h2>
<h3>Gets the date of the system</h3>
<h3>Example: GetDate year, month, day, dayofweek</h3>

<h2><font color="blue">GetTime</font></h2>
<h3>Gets the time of the system</h3>
<h3>Example: GetTime hours, minutes, seconds</h3>

<h2><font color="blue">SetPalette</font></h2>
<h3>Set the color palette from 32 bytes long index variable</h3>
<h3>Example:<br>Dim palette(32) As Byte<br>SetPalette palette</h3>

<h2><font color="blue">SetVDP</font></h2>
<h3>Sets a VDP register with the specified value</h3>
<h3>Example: SetVDP 24,10</h3>

<h2><font color="blue">GetFiles</font></h2>
<h3>Reads files from the specified unit and inserts them in a index variable, also returning the number of files found</h3>
<h3>Example:<br>Dim filesdata(1232) As Byte<br>Dim filesnum As Byte<br>GetFiles 0, filesdata, filesnum</h3>

<h2><font color="blue">GetFileName</font></h2>
<h3>Recover the name of a file from a index variable set before via Getfiles (see Getfiles)</h3>
<h3>Example: GetFileName numfile, filesdata, filename</h3>





