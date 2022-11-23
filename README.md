# MsxCompilerV4
<h3>Works on Windows and Mac OS. Write your code in BASIC (very similar to the Basic MSX but with some differences) and the compiler will create an assembly file that will go directly on .ROM or .DSK, to try on the emulator or real MSX.
First select the emulator path.

Then open a project "*.msxproj" or create one yourself.
When you are ready, press "F5" to compile and RUN!
In the same folder you will find the ".DSK" or ".ROM" file, and ".ASM" source file if you have selected the option in the settings!
</h3>



![screenshot2](https://user-images.githubusercontent.com/118820016/203424623-6bc876ef-61fd-42c5-8d8e-f555e0363de0.png)



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




