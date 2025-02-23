**Total Functions 11/x**
# to fix
strlcat errors
~~strnstr errors~~
substr errors
~~strtrim errors~~
~~calloc redo~~

# Make file
makefiles is used to help decide which parts of a large program need to be recompiled. In the vast majority of cases, C or C++ files are complied. make can also be used beyond compliation too, when you need a series of instructions to run depending on what files have being changed

## the syntax
```
targets: prerequisites
	command
	command
```
- a target is a file name seperated by spaces
- commands are series of setsp typically used to make the target these need to start with a tab character not spaces
- the prerequisities are also file names, seperated by spaces. these files need to exist before the commands for the target are run these are also called dependencies

One of the critical things in Makefile
is that when you have a target
```
blah: blah.c
	cc blah.c -o blah
```
if any modification happen to blah.c make file will notice that blah.c is newer than blah, therefore something changed and needs to be recompiled and this the essence of make and it is a very important point.
# random info
char is essentially a small integer type in c represented as an ASCII value;
y Morpheus and Punisher? I’m i# ft_is*.c
ft_is* always checks if a character passed in a range of characters or not.
The code goes like this
```c
int ft_isalpha(int c)
{
	if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z'))
		return (1);
	return (0);
}
```
Why does the function prototype takes in int, instead of char?
Simply put They have to accept EOF (End of line) in addition to normal character values, they must be also able to handle things like unsigned char.
They also predate the invention of function prototypes. At that time, there was no way to pass a char to a function -- it was always promoted to int first.

Here is more about it :
When C was first invented, there was no compile-time checking of function arguments. If one called `foo(bar,boz)`, and `bar` and `boz` were of type `int`, the compiler would push two `int` values on the stack, call `foo`, and hope it was expecting to get two `int` values. Since integer types smaller than `int` are promoted to `int` when evaluating expressions, C functions which were written prior to the invention of prototypes could not pass any smaller integer type.

This is why they are different. Evolution...

### ft_is* check
- name check
- prototype check
- return 1 check
names are : ft_isalnum.c - ft_isalpha.c - ft_isdigit.c - ft_isascii.c - ft_isprint.c

# ft_tolower ft_toupper
ft_tolower.c | ft_toupper.c

2 other functions done, checked name, prototype value returned

both of those functions check if the character is in the opposite character for example tolower checks if the function is in upper form and transform it to lower letter by manipulating the ascii value of that character, and returning the manipulated value, but if no change was done it just returns the original value

the prototype is similar to ft_is* because as we mentioned back then the functions didn't really have a prototype so the safest way to do it is to just have an int, if the user passed a character it will be interpreted as the int value of that character which is in the other turn is the ascii value of that character.

# fd functions
This is a test for the fd functions
```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>

// Declare function prototypes
void ft_putchar_fd(char c, int fd);
void ft_putstr_fd(char *s, int fd);
void ft_putendl_fd(char *s, int fd);
void ft_putnbr_fd(int n, int fd);

int main() {
    // Open a file for writing output
    int fd = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    if (fd < 0) {
        perror("Error opening file");
        return 1;
    }

    // Test ft_putchar_fd
    printf("Expected: A\n");
    ft_putchar_fd('A', fd);
    ft_putchar_fd('\n', fd);

    // Test ft_putstr_fd
    printf("Expected: Testing ft_putstr_fd\n");
    ft_putstr_fd("Testing ft_putstr_fd\n", fd);

    // Test ft_putendl_fd
    printf("Expected: Testing ft_putendl_fd\n");
    ft_putendl_fd("Testing ft_putendl_fd", fd);

    // Test ft_putnbr_fd
    printf("Expected: 12345\n-67890\n0\n");
    ft_putnbr_fd(12345, fd);
    ft_putchar_fd('\n', fd);
    ft_putnbr_fd(-67890, fd);
    ft_putchar_fd('\n', fd);
    ft_putnbr_fd(0, fd);
    ft_putchar_fd('\n', fd);

    // Close the file
    close(fd);

    printf("Tests completed. Check 'output.txt' for results.\n");
    return 0;
}
```
those functions are pretty basic, but to make sure that we have a good grasp of what they do we need to understand file descriptors,

## what are file descriptors?

A file descriptor is a handle (int value) used to access a file or other Input/Output resources, such as pipe or network socket or a open file. Normally a file descriptors index into a per-process file descriptor table maintained by the kernel in Linux/Unix OS, that in turn indexes into a system wide table of files opened by all processes, called the file table. Thsi table records the 'mode' with  which the file or the other resource has been opened for the following operations : "reading, writing, appending" and possibly other modes.

Simply put they are mappings to a file, pointers to a file that the process is using. 
## ft_putchar_fd
while reading the manual of write you can see that it accepts 3 parameters
write (file descriptor, character address, and the count on how many to write for)
- file descriptor is for file operations, it is usually either the common values which is 0 for standard input 1 for standard output and 2 standard error
- the rest integer values is allocated once a file operation is done such as open and socket
- the second value is a pointer to that data to be written that is why we need to pass in the address of a character but if we have a string declared inside of it it is able to just go through it, but if we ahve a string declared we need an index or a way to iterate one by one and also make sure to pass in the right size of that string but that is something for ft_putstr_fd
## ft_putstr_fd
the function putstrfd is a pretty simple function since we can tell our program to stop when hitting the null we can do a simple while loop which will stop when it hits the null of the string and pass in the character directly to ft_putchar which will take that character and pass in the address to the write
note that if you don't call in ft_putchar and call in write directly you need to pass in 
`&s[i]` instead of `s[i]` because the first represents the address the second represents the character stored in there the second is more like saying `*( s + i)`
## ft_putendl_fd
	void ft_putendl_fd(char *s, int fd)
This function is pretty simple just the basic putstr but it always write a new line at the end of the string that is it
## ft_putnbr_fd
putnbr is a function that write the number to the specified file descriptor

the way this function work is that it takes an int number and needs to write it kind of similar to atoi, but the premises here is to write that number to the specified file descriptor

the first thought process is that a number in the int form is not a number in the ascii form so we need to do a small operation which is to transform that number to the ascii value

if you took a look at the ascii table `man ascii` you will find out that the numbers 0 - 9 in there decimal value are the number it self plus + 48 meaning that `0 + 48` is zero in the ascii form if that makes sense.

So know we know how to transform that number to ascii,
ةخىنث
let us imagine the following number `1` in this case all we have to do is just call ft_putchar with our n + 48 and that is right

and surprisingly you would be correct!

but what if the number is `12` in this case it will would be 12 + 48 which is not 12 is it? it is something else in the ascii table you can check out the ascii table to find out that value, but it is really not 12 in the ascii form 

so let us break it down,

now we need 1 + 48 and 2 + 48 in there ascii form so what is the trick?
**RECURSION**
what is recursion simply put recursion if that act of a function calling it self again and again until a set condition is met.

now think about it, we are sure that we can treat any value from 0-9 so our breaking condition if the biggest number which is 9, so if the number is bigger than 9 perse 10,11,12 and so on we can't treat we need to treat every single digit standalone,

the trick is calling the function with the `n / 10` 

so if we get `12` we call in our function with n / 10 which is the number 1, and then we simply and then we print the following 1 + 48 right?

now you would be wrong, well we are missing a simple piece remember now we have the value 1 but what about the 2?

here is a diagram that will help you![[Pasted image 20241116111354.png]]

so here you can see that the missing link is the module

what is the module you might ask pretty simple it is basically the remainder of the division 

so the module of 10 / 10 is zero cause there is nothing left
but the module of 12 / 10 that is how we get the value 2
what about the module of 1 / 10 it is one so we will always get that first number

so now we got 2 and we got 1 now the order is what matters

so what we do is the following
```
if (n > 9)
	putnbr(n / 10)
putchar(n % 10 + 48)
```
as you can see this is the code
if n is bigger than 9
	we call in our function again
but if that if is done we print our character

now let us get the example of a bigger number to be able to understand more in depth with the following diagram

![[Pasted image 20241116112238.png]]
I hope this kinda of clear things up

but now we have to handle the negative value for that 

we simply put the negative value with putchar and multiply the negative number with (-1) to transform to positive, and of course all of this is in an if loop, if the number is negative

and the final piece is the INT_MIN

now you might wonder why do we have to handle the int_min (I didn't know this while writing this article)

### int_min
let us ask the question that might lead us to the answer,
What is the maximum int value in signed int?
it is INT_MAX which is 2,147,483,647 right?
what is the INT_MIN
it is (-2,147,483,648) so far so good

do you remember what we do to handle the negative number? right we multiply it with -1
so what is `-1 * -2,147,483,648` it is 2,147,483,648 which is bigger than 2,147,483,647 by one digit meaning we can't represent it and we have to handle it by our own that is basically it.

so we need to check if the integer passed in by the user is equal to the int_min and just handle it accordingly by calling putstr of with the int min

and if you didn't handle it you will basically get this weird value

![[Pasted image 20241116113048.png]]

well, I don't know :) if you do let me know would love to know but time doesn't allow

 Aslamu 3likum.

# atoi && itoa
The concept of both those functions are pretty similar but one is harder than the other in it's implementation 
the goal of atoi is to convert an ascii to an integer
in the other hand itoa is to convert an integer to an ascii.

let us get started on breaking down the the *atoi*

### ft_atoi
let us start by reading the man rtfm

the prototype is as follows `int atoi(const char *nptr)`

convert a string to an integer

it converts the initial portion of the string pointed by nptr to int. The behavoir is the same 
strtol(nptr, NULL, 10)

except that atoi() doesn't detected errors.

Returns **the converted value or 0 on error**
- reading this manual prompts us to check the following function strtol;
#### strtol
it converts the first part of the string to a long integer value according to the given base we won't go in much detail but the base we work with in atoi is just the decimal _10_ base.

The string may start with any amount of white space determined by isspace function, followed by a single optional '+' or '-' the remainder of the string is converted to an integer stopping at the first invalid character and returning from there

- after reading the following we should understand that to rebuild atoi we need to achieve the following :
	- recreate isspace
	- detect the first sign if it is present + or -
	- treat the rest of the string and start transforming it to an integer
note that there is some undefined behavior so it doesn't matter.

and that is about it make sure to first implement the isspace and then check for the next index if it is a sign then multiple accordingly and go the the next index and as long as you are in the a digit start saving the value of that digit in a result using the same concept for putnbr.
## ft_itoa
it is the opposite of atoi, we take in an integer and transform it into an ascii value
# mem functions | 2/8 functions
### why use unsigned char
In C, unsigned char is often used in memory functions due to its unique properties:

- Guaranteed 1-byte size: unsigned char is always a single byte (8 bits), which makes it an ideal choice for representing small, fixed-size data types or flags.
- No sign bit: Unlike signed char, unsigned char does not have a sign bit, which means it can only represent non-negative values (0 to 255). This property ensures that bitwise operations and arithmetic operations behave predictably.
- Implementation-defined behavior: The C standard does not specify whether char is signed or unsigned by default. Using unsigned char explicitly ensures a consistent behavior across different compilers and platforms.

In memory functions, unsigned char is commonly used: Bitwise operations: When working with bit flags, masks, or bit-level manipulation, unsigned char provides a convenient and efficient way to represent and operate on single bytes. Memory-mapped I/O: In systems programming, unsigned char is often used to access memory-mapped I/O registers, where each byte represents a specific control or status bit. Buffer manipulation: When working with buffers or arrays of bytes, unsigned char can be used to iterate over the buffer elements or to perform byte-level operations. Compatibility: In legacy code or when interacting with systems that use 8-bit bytes, unsigned char ensures compatibility and avoids potential issues with signed char types. In summary, using unsigned char in memory functions in C provides: Predictable behavior due to its non-negative value range Efficient use of memory, as it always occupies a single byte Compatibility with systems that use 8-bit bytes Convenience for bitwise operations and bit-level manipulation When working with memory functions in C, consider using unsigned char whenever you need a single byte of data, bit-level manipulation, or compatibility with systems that use 8-bit bytes.
## understand type casting
simply put convert one datatype into another 
there is two kinds :
- implicit type cast which is used to convert the data type of any variable without using the actual value that the variable holds it performs the conversions without altering any of the values which are stored in the data variable. Conversion of lower data type to higher data type will occur automatically
- ![[Pasted image 20241116173515.png]]
as the image above explains implicit type casting just happens by the program automatically promoting a variable to another data type, just the compiler doing it is thing converting one data type to another, and usually happens when a small data type is used in a expression with a larger data type 
```c
int a = 10;
float b = a; // int will be converted to float

char x = 'a';
x = x + 3;
```
here in the second char example 3 is an int but x is smaller value so it is implicitly converted to an int.
- Explicit Type casting
This is simply put when you tell the program to convert and data type to another, this is mostly done in cases where if the datatype remains unchanged it can give incorrect output. in such cases typecasting can help to get the correct output and reduce the time of compilation. This type of casting is defined within the program, and we use what is called a ``
```c
//cast operator example
float a = 5.5;
int b = (int) a;
```
![[Pasted image 20241116174153.png]]
implicit : like purint water from a smaller glass into a bigger glass it happens naturally, without any effort
explicit : like forcing a larger object into a smaller container -- you have to do it manually and some parts might get lost.

and just for fun the literally meaning of explicit is directly and clearly without any room for confusion
implicit is suggested not directly expressed
good functions that kinda of execute type casting are atoi and itoa.
## ft_memset
This function reads the following : 
	memset - fill memory with a constant byte
	it fills the first n bytes of the memory area pointed to by s with the constant byte c,
	it returns a pointer to the memory area s

memset sets a range of memory that is sent by the user, all that the user sends is the pointer and the character to fill that memory range with, and the range is defined by the size passed by user

prototype `void *memset(void *s, int c, size_t n);`

The logic is pretty basic just go over the whole range of memory either from zero up to n or go from n down to zero, just make sure not to interact with n, cause again and again n is size but we are working with indexes meaning that the world "Hello" is of size 5, but the last index is of value 4 if that makes sense and the size including the null is 6 but the index is of the null is 5, always think like this.

And also don't forget to type cast, since the function prototype accepts any value, meaning void pointer so that the user can pass in any type of pointer, but since memset affects byte by byte we need to type cast to a char pointer to be able to preform actions on the byte by byte sequence.
## ft_bzero

This functions doesn't really fall under the naming mem but it is just memset but the character passed to memset is 0
the manual reads, bzero, explicit_bzero - zero a byte string

it erases the data in the n bytes of the memory starting at the location pointed to by s, by writing zeros (bytes containing '\0') to that area

it returns nothing

the prototype is : void bzero(void *s, size_t n);

and for the implementation we just call on the memset with the value 0 passed on there that is about it.
## memcpy
copy memory area

the memcpy function copies n bytes from memory area src to memory area dst, from source to destination that is why it is called the names they are called I need to stop confusing between them.

the memory area must not overlap, in the case they do we use memove

and simply memcpy returns a pointer to dest

so to recreate this function simply we do something called a typecast

## memchr
simply scan a memory area for a character so we go until we find a character we return the pointer to it if not found we simply return 0
keep in mind that it does for the first instance of the character.
## memmove
we move a block of memory just like memcpy and but we handle overflow in the case that dest >= src we copy from the end of the block memory to not lose the data.

## memcmp
compare memory areas

it compares the first n bytes (each interpreted as unsinged char) so the 

# functions that pass in a function 

those functions are pretty basic the first one is called striteri
and the prototype can tell us a lot 

```c
void ft_striteri(char *s, void (*f)(unsigned int, char*))
```
the function have no return value and the description reads that striteri would apply the function that is passed in for every character in the string,

and this is also called fallback function : which is a function that is call through a function pointer

so when you calling this function you would just pass in the function name and that is it 

`ft_striteri("Hello", ft_toupper)` as you can see here and if you printed the address of this function it will show you some time of pointer 

so in the prototype void is the return type since iteri manipulates the address of the character it doesn't have to return anything for us, we have our pointer function and then the prototype of that function so that nothing else that is wrong is passed on to our function

and that is iteri and mapi in a nutshell

again mapi manipulates the character that is given to it and returns it 
iteri manipulates the value that is stored in the address that is passed to it and doesn't return anything

this is the prototype on how do call back`return_type (*pointer_name)(parameter_types);`

# string search functions
those functions will probably search from something inside of a string and return a value wither that is a pointer or another string it really depends

## ft_strchr ft_strrchr

reads 'locate character in a string'

it returns a pointer to the first occurrence of the caracter c in the string s.

and strrchr does the opposite of it.

return a pointer to the matched character or NULL if the character is not found. the terminating null byte is considered part of the string, so that if c is specified as \0 these functions return a pointer to the terminator aka the end of the string.

## ft_split
the main idea of this function is to take a string `*s` and split it into an array of words based on a delimiter

the example is as follows "Hello there world" and the delimiter is ' ' space what will happen here is that we need to  create a null terminated array`[["Hello"], ["there"], ["world"], ['\0']]` 

the array above is what we need to output and we need to malloc and free,

so the idea is as follows

we found out the length of this string using a function called word count, this function will count the amount of words needed to be allocated + 1 for the null terminated array,

here is one implementation of word count

### word count
first of all we use a flag and that flag value is set to 1, after that we have our count variable
we start our while loop and the loop is iterating using an index and stopping when the null terminator is hit,

so if the flag is 1, and the character is not the delimiter the count increases, and also the flag is reset to 0, but if we are in a delimiter we set the flag to 1 and the loop keeps on going on that is it, pretty simply

### the rest of split
after knowing the word count you malloc the array size, then there is a bit of logic 
first of all you put yourself in a big loop where you loop through the whole string
inside of that loop you basically make sure that the first characters are not the delimiter the momement you hit a non delimiter you stop and now you start counting the size of your string and that can be done in another loop and that loop also makes sure that you didn't hit the end of the string, after that same loop you malloc the index of the array for the current string you are in, right after that you use that same loop to copy each character from the string to the array index and you null terminator and that is it

## ft_strjoin
this is also a pretty basic function you malloc the length of both strings + 1 for the null terminator 

and then you start copying the first string from 0 up to the len start copying the next string up to len but start from the length of the first string to len of the the next string and right after it you null terminat

## strnstr
this function is used to locate a string inside of another needle in a haystack,

so first of all think about implementing just strstr then implement the n part

the manual reads locates the first occurence of the null-termianted string little in the string big, where not more than len characters are searched. Characters that appear after \0 character are not searched. Since the strnstr function is a freebsd specific api it should on be used when portability is not a concern

if little is an empty string, big is returned, if little occurs nowhere in big null is returned; otherwise a pointer to the first character of the first occurrence of little is returned.

so yea pretty simple implementation just make sure that in your own logic, you implement in the while loop condition or an if loop, that your index is not out of range of the size so like i + j < size and that is about it really.
## strtrim
the idea behind this function is also pretty simple

so if we `abcabchelloabc` and our set is `abc` we need to trim out abc from there until we meet a character that is not in that set so for example `hello` should be the output and that is pretty much it,

but how do you implement well first of all we need to know the positions of where we start and where we end 

and that is simple as long as we are in the string and we can use strchr as long as it returns true meaning that for the set= `abc` and the str[i] is in that set we increase start until we meet with a character that is not inside of the set aka h

with the end we do the opposite we set end to the len -1 of our string we also call in set and our break in condition is that end doens't get to 0 and that is it
we malloc end - start + 2
we set the null in end - start + 1 index, 
we use memcpy to start copying from the str to trimmed str, and we stop at end - start + 1 because memcpy requires the size not the index and always remember size is always 1 time bigger from the index
and that is about it,
now if the start is bigger than the end we then just allocate for the null terminator and return the null terminator that is it
## substr
is another simple function you give it a start and an end and you malloc a new string where it take the start and stop at then end when making the new string, that is really it and some case handling if like start > end and so on 
# mem alloc function

## ft_strdup

is a pretty basic function you malloc the size of the string and make sure you also include the null terminator

you call in memcpy, cpy the whole string and then make sure you put the null terminator in its spot
# other functions

## ft_strlen
is a pretty basic just keep on counting the string length using an int i until you hit the end of the string which the null terminator always remember that a string is an array of characters and the difference between a string and a buffer is that a string is nul terminated and the buffer is just a place in the memory

## ft_strncmp
is a also a simple yet good function
it finds the difference between two strings think of it just comparing both values of the string positions meaning if you have "hello" and "kello" with a size of 5, it will compare character by character until we meet a difference and return a difference value so in this case we see that h > k so we can return either 1 or the difference between there ascii values which is the easiest thing to do 

if n is 0 we return 0 or if we have reached n and found no difference we also return 0, so to handle both cases we just say if i == n we return 0 that is about it.