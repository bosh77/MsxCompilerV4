# MSX Compiler V4 v_2025

[MSX Compiler V4 download](https://github.com/bosh77/MsxCompilerV4/releases/tag/MSXv4)


2023-11-12: fixed some bugs
2023-11-12: Added support for [music](#use-music-player) and [sound effects](#use-effect-player) 
2023-11-12: Added StartInterrupt and StopInterrupt commands

2022-12-19: fixed PSET bug in Screen 4 or lower
2022-12-19: fixed PSET bug when color is not specified
2022-12-19: fixed IF...THEN bug
2022-12-10: Added support for 256kb code

[Developed with Cerberus X](https://www.cerberus-x.com)


Works on Windows, Mac OS and Linux. Write your code in BASIC (very similar to the Basic MSX but with some differences) and the compiler will create an assembly file that will go directly on .ROM or .DSK, to try on the emulator or real MSX.
First select the emulator path.

On Mac OS it is necessary to set an additional path because the executable file is located inside the emulator's app folder.
Then open a project "*.msxproj" or create one yourself.
When you are ready, press "F5" to compile and RUN!
In the same folder you will find the ".DSK" or ".ROM" file, and ".ASM" source file if you have selected the option in the settings!

Be careful when you activate the "Save .ASM File When Run" option because it saves the project in assembly files that can overwrite the existing ones

## How it's work

The compiler read BASIC code and translate it in ASSEMBLER, then translate it in BINARY code You can save and see ASM file if you want, select relative option in Settings window
If the binary code resulting from the compilation is greater than 16kb, then you will have to divide your project into more than one file, each with a maximum of 16kb of binary code, up to a maximum of 16 files. It is possible to make calls between files with the instructions CALLPAGE (as gosub) and JUMPPAGE (as goto)

example: CALLPAGE 1,100 --> make a calling to page 1 at line 100

Look at example folders to see how it's work

You can select TARGET of your compilation (CARTRIDGE or DISK)

Depending on the TARGET (cartridge or disk), a .ROM file of a maximum of 512kb or a .DSK file containing up to a maximum of 32 binary files of 16kb each will be created!

- Memory containing the binary code (4000h-7fffh) with memory mapper to change page
- Memory containing the variables (8000h-bfffh)
- Reserved memory (c000h-cb2fh)
- Additional memory available from cb30h onwards. (Note that the stack pointer is initialized to DB00h)
- Stack (DB00h) but you can change it in Settings window

## Instructions and syntax

The supported variables are:
- UBYTE (8 bit unsigned, ex: DIM A AS BYTE)    occupies 1 byte in memory
- INT (16 bit, ex: DIM B AS INT )              occupies 2 bytes in memory
- FLOAT (32 bit, ex: DIM C AS FLOAT)           occupies 4 bytes in memory
- STRING (16-256 byte, ex: DIM D AS STRING)    length in memory depends on the #lenstrings command
- ARRAY (ex: DIM E(10) AS INT) make 10 elements, not 11

Every variable must be declared with DIM before use it

![screenshot2](https://user-images.githubusercontent.com/118820016/203424623-6bc876ef-61fd-42c5-8d8e-f555e0363de0.png)

## Supported instructions:

#lenstrings, #Imports, #SetVarAddress, IncludeFile, CallPage, JumpPage, AsmLine, Goto, Gosub, Return, Dim, Cls,GetChar, GetKey,
Screen, Set page, Setpage, DrawXBlock, DrawBlock, DrawNumber, AddNumber, GetAddressLine,
GetDate, GetTime, GetFiles, GetFileName, SetPalette, SetVDP, Color, Color=, Locate, Print, Stick, Strig,
Pset, Circle, Paint, Line, Copy, Vpoke, Poke, Vpeek, Peek, PutSprite, Put Sprite, DataB,
DataF, DataI, DataS, ReadB, ReadF, ReadI, ReadS, Restore, DrawText, if...then...else,
for...next, GetPoint, SetScroll, Sound, SaveData, LoadData, LoadDataToVram, SaveDataFromVram, CopyRamToVram, CopyVramToRam,
ReadSector, WriteSector, GetCollision, SetAttrSprites, SetColorSprite, SetSpriteAddress, DrawAttrSprites, DrawColorSprites, DrawPatternSprites,
SetVarsPage, LoadMusic, PlayMusic, PauseMusic, ResumeMusic, LoadEffect, PlayEffect, StartInterrupt, StopInterrupt

(In if...then use { } with boolean operator. ex: If a=1 And {b=2 or c=3} Then ... )

## Other functions:

TAN, ATN, COS, SIN, SQR, ABS, INT, RND, ASC, VAL, CHR, STR, HEX, INP, VDP, PAD

## Operators for math operations:

| = or  
& = and  
~ = xor  
% = mod  

## New added commands

### Use Music Player

#### #ImportMusicPlayer

Now you can use the following commands (thanks to Augusto Ruiz and WYZ for [WYZPlayer](https://github.com/AugustoRuiz/WYZPlayer)

#### LoadMusic

Load a song created with [WYZTracker](https://github.com/AugustoRuiz/WYZTracker) (thanks to Augusto Ruiz and WYZ)

Example: **LoadMusic 0,"song"**

#### PlayMusic

Play a song loaded with LoadMusic

Example: **PlayMusic 0**

#### PauseMusic

Pause a song

Example: **PauseMusic**

#### ResumeMusic

Restart a song previously paused

Example: **ResumeMusic**


### Use Effect Player

#### #ImportEffectPlayer

Now you can use the following commands

#### LoadEffect

Load a sound effect

Example: **LoadEffect 0,"sample"**

#### PlayEffect

Play a sound effect loaded with LoadEffect

Example: **PlayEffect 0**


#### StartInterrupt

Create an interruption that performs a routine to the page and label or the specified line number

Example: **StartInterrupt 0,myroutine**

Example: **StartInterrupt 1,100**

#### StopInterrupt

Stop the interruption created with **StartInterrupt**

Example: **StopInterrupt**


### New instructions of this BASIC version

#### #lenstrings

Set length of string. His value can be 15,31,63,127 or 255

Example: **#lenstrings=31**

#### #SetVarAddress

Set the initial memory address of the variables

Example: **#SetVarAddress=$9000**

#### #Imports

adds a new page to the project, which can be reached with the CALLPAGE or JUMPPAGE instructions

Example: **#imports 1,"part2.msxproj"**

Now you can use **CALLPAGE** and **JUMPAGE**

#### CallPage

makes a call to the specified page and program line (or label), as a Gosub instruction

Example: **CallPage 1,100**

#### JumpPage

makes a jump to the specified page and program line (or label), as a Goto instruction

Example: **JumpPage 2,mylabel**

#### AsmLine

Insert an assembly instruction

Example: **AsmLine "LD A,10"**

#### GetChar()

Read the Keyboard and return the ascii code corresponding to the key pressed

Example: **a=GetChar()**

<h2><font color="blue">GetKey(KeyCode)</font></h2>
<h3>Read the Keyboard and return 0 (key not pressed) or 255 (key pressed)</h3>
<h3>Example: b=GetKey(KeySpace)</h3>
<h3>See all key codes <a href="#keycodes">here</a></h3>

<h2><font color="blue">GetAddressLine</font></h2>
<h3>Get the memory address of line or label specified and return it to the specified variable</h3>
<h3>Example: GetAddressLine id,1000 (now the variable id contains memory address of line 1000)</h3>

<h2><font color="blue">GetAddressVar</font></h2>
<h3>Get the memory address of variable specified and return it to the another variable</h3>
<h3>Example: GetAddressVar id,variable (now the variable id contains memory address of variable)</h3>

<h2><font color="blue">DrawBlock ram, vram, lines, wram</font></h2>
<h3>Copy a block of memory from RAM to VRAM</h3>
<h3>Example: DrawBlock 0, 0, 10, 32</h3>
<h3>(See some examples in the Example folders)</h3>

<h2><font color="blue">DrawXBlock ram, vram, lines, wram, wvram, addvram</font></h2>
<h3>Copy a block of memory from RAM to VRAM, it is also possible to specify 2 additional parameters</h3>
<h3>Example: DrawXBlock 0, 0, 10, 32, 20, 40</h3>
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

<h2><font color="blue">SaveData drive,filename,iddata,lengthdata</font></h2>
<h3>Save a block of RAM memory on the disk, starting from the iddata memory address for the specified length.</h3>
<h3>Example: SaveData 0,"filename.ext",iddata,100</h3>

<h2><font color="blue">LoadData drive,filename,iddata</font></h2>
<h3>Load a file in the RAM memory, the initial address is specified by the iddata variable.</h3>
<h3>Example: LoadData 0,"filename.ext",iddata</h3>

<h2><font color="blue">SaveDataFromVram drive,filename,iddata,lengthdata</font></h2>
<h3>Save a block of VRAM memory on the disk, starting from the iddata memory address for the specified length.</h3>
<h3>Example: SaveDataFromVram 0,"filename.ext",iddata,16384</h3>

<h2><font color="blue">LoadDataToVram drive,filename,iddata</font></h2>
<h3>Load a file in the VRAM memory, the initial address is specified by the iddata variable.</h3>
<h3>Example: LoadDataToVram 0,"filename.ext",iddata</h3>

<h2><font color="blue">ReadSector drive,idram,sector</font></h2>
<h3>Load a Disk Sector to RAM memory, the initial address is specified by the idram variable.</h3>
<h3>Example: ReadSector 0,32768,1</h3>

<h2><font color="blue">WriteSector drive,idram,sector</font></h2>
<h3>He writes a sector on the disk with the 512 bytes contained in the RAM to the address specified by the idram variable.</h3>
<h3>Be careful to use this command because it could compromise the disk in case of incorrect writing (always make a copy of the disk).</h3>
<h3>Example: WriteSector 0,32768,14</h3>

<h2><font color="blue">CopyRamToVram ramaddress,vramaddress,lengthdata</font></h2>
<h3>Copy a block of memory from RAM to VRAM.</h3>
<h3>Example: CopyRamToVram 32768,0,16384</h3>

<h2><font color="blue">CopyVramToRam vramaddress,ramaddress,lengthdata</font></h2>
<h3>Copy a block of memory from VRAM to RAM.</h3>
<h3>Example: CopyRamToVram 0,32768,16384</h3>

<h2><font color="blue">GetCollision c,x1,y1,w1,h1,x2,y2,w2,h2</font></h2>
<h3>It detects a collision between two rectangular objects (for example two sprites) and returns 255 in the c variable in case of collision.
    The variables X1, Y1, W1, H1, X2, Y2, W2, H2 must all be of the same type (byte or int).</h3>
<h3>Example: GetCollision c,x1,y1,16,16,x2,y2,16,16</h3>

<h2><font color="blue">SetSpriteAddress</font></h2>
<h3>Set the values of the names, colors and patterns of the sprites, which will be used by the instruction SetColorSprite, DrawAttrSprites, DrawColorSprites and DrawPatternSprites.
    It does not require parameters and must be used after the Screen command or after setting the registers of the VDP of the sprites attributes.
    See Scroll Example in Examples folder.</h3>
<h3>Example: SetSpriteAddress</h3>

<h2><font color="blue">SetColorSprite nsprite,iddatacolors</font></h2>
<h3>Set the color of the sprite specified in nsprite (0-31) with the data of the RAM specified at the address iddatacolors (16 bytes). Screen 4 or higher.</h3>
<h3>Example: SetColorSprite 0,iddatacolors</h3>

<h2><font color="blue">SetAttrSprites spritesrc,spritedest,yscreen</font> and <font color="blue">DrawAttrSprites spritedest</font></h2>
<h3>These two commands must be used together.
    The first law the values of the names of the sprites from 4 bytes matrices and copies them in a memory area, the second copy this memory area in the Vram at the address corresponding to the names of the sprites.
    yscreen specifies a vertical offset useful when working with vertical scroll.<br>See Scroll Example in Examples folder to better understand how they work.</h3>
    <h3>Example:<br>
        Dim xspr(32),yspr(32),mspr(32),cspr(32)<br>Dim idsprsource As Int<br>Dim sprdest(128) As Byte<br>Dim idsprdest As Int<br>.<br>.<br>.<br>
        GetAddressVar idsprsource,xspr(0)<br>GetAddressVar idsprdest,sprdest(0)<br>.<br>.<br>.<br>
        SetAttrSprites spritesrc,spritedest,yscreen<br>DrawAttrSprites spritedest<br>
        
<h2><font color="blue">DrawColorSprites spritedest,numsprites,iddatacolors</font></h2>
<h3>Set the color of the sprites specified in spritedest with the data of the RAM specified at the address iddatacolors. Screen 4 or higher.</h3>
<h3>Example: DrawColorSprites 0,2,iddatacolors (Set the colors of sprites 0,1 and 2)</h3>
                
<h2><font color="blue">DrawPatternSprites spritedest,numsprites,iddatapatterns</font></h2>
<h3>Set the pattern of the sprites specified in spritedest with the data of the RAM specified at the address iddatacolors. Screen 4 or higher.</h3>
<h3>Example: DrawPatternSprites 3,4,iddatapatterns (Set the pattern of sprites 3,4,5 and 6)</h3>
        
<h2><font color="blue">SetVarsPage memorypage</font></h2>
<h3>Set the variable memory page (it is nothing more than OUT ($FE), memorypage) since the variables occupy the memory area $8000-$BFFF</h3>
<h3>Example: SetVarsPage 1</h3>



<hr>
<a name="keycodes">
<h2><font color="blue">List of key codes</font></h2>

<h3>Key0, Key1, Key2, Key3, Key4, Key5, Key6, Key7, Key8, Key9, KeyMinus, KeyEqual, KeyBackslash, KeyOpenBracket, KeyCloseBracket, KeyColon, KeyQuote1, KeyQuote2, KeyMinor, KeyMajor, KeySlash, 
KeyA, KeyB, KeyC, KeyD, KeyE, KeyF, KeyG, KeyH, KeyI, KeyJ, KeyK, KeyL, KeyM, KeyN, KeyO, KeyP, KeyQ, KeyR, KeyS, KeyT, KeyU, KeyV, KeyW, KeyX, KeyY, KeyZ, KeyShift, KeyCtrl, KeyGraph, KeyCap, KeyCode, 
KeyF1, KeyF2, KeyF3, KeyF4, KeyF5, KeyEsc, KeyTab, KeyStop, KeyBs, KeySelect, KeyReturn, KeySpace, KeyHome, KeyIns, KeyDel, KeyLeft, KeyUp, KeyDown, KeyRight</h3>


<hr>
<a name="IDE"></a>
<h2><font color="blue">IDE Functions</font></h2>

<h3>The IDE contains all the normal functions of a text editor (for example: CTRL + Z = Undo)<br>
    In addition there are some commands for different functions:</h3>

<h2><font color="blue">CTRL + K = Set Main Project</font></h2>

<h2><font color="blue">F12 = If pressed on a line number or a label after a GOTO or GOSUB instruction, moves the cursor to the original line number or label</font></h2>

<br><br><br><br>
<hr>
<h3>Special thanks to Zeda Thomas for the <a href="https://github.com/Zeda/z80float">float32 routines!</a></h3>
<h3>Special thanks to Grauw for information about assembly language (<a href="http://map.grauw.nl/">http://map.grauw.nl/</a>)</h3>
<h3>Special thanks to Augusto Ruiz for the <a href="https://github.com/AugustoRuiz/WYZPlayer">WYZPlayer</a> and the <a href="https://github.com/AugustoRuiz/WYZTracker">WYZTracker</a> to compose music </h3>
<h3>Special thanks to WYZ (WYZplayer ASM Code and music modules)</h3>

<br><br>

