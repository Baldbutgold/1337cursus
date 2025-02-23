Things to look up:
https://www.geeksforgeeks.org/static-vs-dynamic-libraries/
https://www.geeksforgeeks.org/compiling-a-c-program-behind-the-scenes/

- Sizeof
	- sizeof(arr) will work if the arr has a predefined value, but if declare with `char *arr` it won't work.
	- sizeof only works at compile time so when you define an array like `char arr[10]` the size allocated to the arr is done at compile time, so sizeof returns the number of bytes allocated for arr at compile time. so it would return 10 since each char is 1 byte.
	- but when you declare a pointer like `char *arr` it will return the size of the pointer itself so that is 8 bytes in the 64-bit system, rather than the memory size it points to, even if you later on allocate the memory with malloc, because at compile time it only knows that arr is a pointer not the size of the memory block it points to. You'd need to keep track of the allocated size manually.
	- So think of sizeof mostly for Dynamic Allocation to help you allocate an array or something like that.
	- **Sizeof returns the total memory size on for arrays with fixed sized declared at compile time** for pointers it just returns size of the pointer
- array vs string
	- remember a string is an array of characters ended with a null terminator if not null terminated there is no string there is just an array of characters.
- C doesn't display characters as readable symbols beyond ASCII 127, so 255 appears garbled, meaning just a weird question mark symbol.
- Compile Time vs Runtime
	- Compile time is when the code is being transformed from high level to machine code by the compiler
	- Run time is when the program is actually running and being executed by the cpu. any operation that depends on user input, dynamic allocation, or other run time conditions occurs at run time meaning the compiler doesn't know the details like exact memory size beforehand.
## Data types in C
we have the following data types
- char takes on 1 byte which is 8 bits and the value range is -128 to 127 or it can also be 0 to 255.
- unsigned char also takes on 1 byte and the value range is 0 to 255
- signed char also 1 byte it takes -128 to 127.
- one thing to note is that since 1 byte is 8bits and each bit can be either 0 or 1, in the case that all 8 bits are 1111 1111 : this means that the smallest bit is 1 then 2,4, 8, 16, 32, 64, 128 and if we take the sum of all those numbers meaning 1 + 2 + 4 + 8 + 16 + 32 + 64 + 128, will result in 255, and -128 + 127 is also that range, but it uses another system called 2 compliments it is a system to work with signed values, the msb *most significant bit* is used to represent positive or negative meaning if the MSB is 0 it is either positive or zero, but if the MSB is 1 it is a negative number, so for positive numbers it is just how you can think of meaning for example the number 9 is `0000 1001` which is 9 `8 + 1` but what about negative values?
- so why do we get -128 to 127, it is because as i said the msb is treated as a negative sign meaning that if 1000 0000 meaning that msb is -128, and the rest is positives so like 0111 1111, will result in you guessed it 127 cause 1 + 2 + 4 + 8 + 16 + 32 + 64 = 127, and that is pretty much about it, so in this way we can represent -128 and also -1 by making 1111 1111 meaning -128 + 127 which is -1 so making the msb negative and the rest is just treated as a positive so 2 compliment system is only used with signed char, and for example let us say we want negative 64, well let us first of all represent 64 in the normal way which is `0100 0000` so that is 64 if we can -64 we flip all the bits and add a 1 `1011 1111` so that is the flipping no we add 0000 0001 the one`1100 0000` and we do the opposite for -64 to 64 so invert `0011 1111` and then add 1 `0100 0000` and that is basically it, the two compliment system
- int which is 2 or 4 bytes depending on the system architecture, it is -32,768 to 32,767 or -2,147,483,648 to 2,147,483,647.
- unsigned int 2 to 4 bytes, 0 - 65,535 or 0 to 4,294,967,295
- short is just 2 bytes int, -32,768 to 32,767
- unsigned short is just 0 - 65,535
- long which is 8 bytes it is a quintillion, and it is also refereed to as int64 cause it is 8 bytes long and 64 bit long
- *unsigned long* 8 bytes and is just the positive
- then we have a float 4 byte, and 6 decimal places, double and long double 8 and 10 bytes respectively and 15 and 19 decimal places respectively 