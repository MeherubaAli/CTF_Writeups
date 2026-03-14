# Compiled
Strings can only help you so far.

### Level: Easy
### Room Overview: 
Analyse a compiled binary file to get the flag.

### Tools Used:
- Ghidra

### Important Note: Download the task file and get started. The binary can also be found in the AttackBox inside the /root/Rooms/Compiled/ directory.<br> Note: The binary will not execute if using the AttackBox. However, you can still solve the challenge.

The room starts with a "threat" of sort: Strings can only help you so far.
But still, let us start with that. We go to `cd /root/Rooms/Compiled/` and if we print `ls`, the we can see the `Compiled.Compiled` file, which we will analyse. 
So, let's start with `strings Compiled.Compiled`. We can see that there are quite a few strings listed like `stdout`, `printf`, `sForNoobH`, `Password:`, `DoYouEven%sCTF`,`_init`, `Correct!`, `Try Again!`, and many more.
Some string here is the password.<br>

Then, we need to open Ghidra which is in the machine to analyse the `Compiled.Compiled` file. We open `file > import file > choose the file` then from `Active project`, open `Compiled.Compiled`.
Let's click on `main` file, as if there is a password, it'll be here. Now, let's take a look at the `decompiled main` file.

We can see that `fwrite("Password: "1.10,stdout)` & next line `__isoc99_scanf("DoYouEven%sCTF",local_28)` and we can see more with `local_28` variable which is stack allocated var.
From there, we can match the passwords input requirements. The first string is `DoYouEven`. `%sCTF` can't be in the password as the `%` sign & anything after it, aren't strings. The next possible strings are `_init` & `__dso_handle`. 

If we check the combinations one by one, `DoYouEven_init` fits the requirements perfectly. <br>

DONE!
