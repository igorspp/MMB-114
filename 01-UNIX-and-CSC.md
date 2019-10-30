# Day 1: UNIX and CSC

## Connecting to Taito

### Windows users

* Launch PuTTY
* In “Host Name (or IP address)”, type **puhti.csc.fi** and click “Open”
* In the following dialogue window, choose “Yes”
* Type your CSC username and hit "Enter"
* Type your password and hit "Enter"
* To logout just type **exit** and hit "Enter"

### MacOS users

* Launch Terminal
(e.g. open the Launchpad and type **terminal**)
* Type **ssh user<span>@taito.csc.fi** and hit "Enter" (change "user" for your own CSC username)
* In the following dialogue, type **yes** and hit "Enter"
* Type your password and hit "Enter"
* To logout first type **exit**, hit "Enter", and close the Terminal window

## Playing around with basic UNIX commands

### Important notes

Things inside a box like this...

```bash
mkdir MMB-114
cd MMB-114
```
...represent commands you need to type in the shell. Each line is a command. Commands have to be typed in a single line, one at a time. After each command, hit “Enter” to execute it.

Things starting with a pound sign (or hashtag)...

```bash
# This is a comment and is ignored by the shell
```

...represent comments embedded in the code to give instructions to the user. Anything in a line starting with a "#" is ignored by the shell. You can type it if you want, but nothing will happen (provided you start with a "#").

We will be using different commands with different syntaxes. Different commands expect different types of arguments. Some times the order matters, some times it doesn't. If you are unsure, the best way to check how to run a command is by taking a look at its manual with the command **man**. For example, if you want to look at the manual for the command **mkdir** you can do:

```bash
man mkdir

# You can scroll down by hitting "backspace"
# To quit, hit "q"
```

### Creating and navigating directories

First let's see where we are:

```bash
pwd
```

Are there any files here? Let's list the contents of the folder:

```bash
ls
```

Let's now create a new folder called "MMB-114". In addition to the command (**mkdir**), we are now passing a term (also known as an argument) which, in this case, is the name of the folder we want to create:

```bash
mkdir MMB-114
```

Has anything changed? How to list the contents of the folder again?

<details>
<summary>
HINT (CLICK TO EXPAND)
</summary>

> ls

</details>  

---

And now let's enter the "MMB-114" folder:

```bash
cd MMB-114
```

Did it work? Where are we now?

<details>
<summary>
HINT
</summary>

> pwd

</details>  

### Creating a new file

Let's create a new file called "myfile.txt" by launching the text editor **nano**:

```bash
nano myfile.txt
```

Now inside the nano screen:

1. Write some text

2. Exit with ctrl+x

3. To save the file, type **y** and hit "Enter"

4. Confirm the name of the file and hit "Enter"

List the contents of the folder. Can you see the file we have just created?


### Copying, renaming, moving and deleting files

First let's create a new folder called "myfolder". Do you remember how to do this?

<details>
<summary>
HINT
</summary>

> mkdir myfolder

</details>  

---

And now let's make a copy of "myfile.txt". Here, the command **cp** expects two arguments. The first is the name of the file we want to copy, and the second is the name of the new file:

```bash
cp myfile.txt newfile.txt
```

List the contents of the folder. Do you see the new file there?  

Now let's say we want to copy a file and put it inside a folder. In this case, we give the name of the folder as the second argument to **cp**:

```bash
cp myfile.txt myfolder
```

List the contents of "myfolder". Is "myfile.txt" there?

```bash
ls myfolder
```

We can also copy the file to another folder and give it a different name, like this:

```bash
cp myfile.txt myfolder/copy_of_myfile.txt
```

List the contents of "myfolder" again.  Do you see two files there?

Instead of copying, we can move files around with the command **mv**:

```bash
mv newfile.txt myfolder
```

Let's list the contents of the folders. Where did "myfile.txt" go?

We can also use the command **mv** to rename files:

```bash
mv myfile.txt myfile_renamed.txt
```

List the contents of the folder again. What happened to "myfile.txt"?

Now, let's say we want to move things from inside "myfolder" to the current directory. Can you see what the dot (**.**) is doing in the command below? Let's try:

```bash
mv myfolder/newfile.txt .
```

Let's list the contents of the folders. The file "newfile.txt" was inside "myfolder" before, where is it now?  

The same operation can be done in a different fashion. In the commands below, can you see what the two dots (**..**) are doing? Let's try:

```bash
# First we go inside the folder
cd myfolder

# Then we move the file one level up
mv myfile.txt ..

# And then we go back one level
cd ..
```

Let's list the contents of the folders. The file "myfile.txt" was inside "myfolder" before, where is it now?  


We have so many identical files in our folders. Let's clean things up and delete some files:

```bash
rm newfile.txt

# To confirm, type yes and hit "Enter"
```

Let's list the contents of the folder. What happened to "newfile.txt"?  

To skip the confirmation dialogue and force the deletion, we can modify the command by adding the force flag (**-f**). Flags are used to pass additional options to the commands. Let's try again, but pay attention in what you are doing, if you accidently remove the wrong file, it is gone forever!

```bash
rm -f myfile.txt
```

And now let's delete "myfolder":

```bash
rm -f myfolder
```

It didn't work did it? Because to delete a folder we have to modify the command further by adding the recursive flag (**-r**):

```bash
rm -f -r myfolder

# Any of the formats below also work:
rm -r -f myfolder
rm -fr myfolder
rm -rf myfolder

# The following command also works, but only if the folder is empty:
rmdir myfolder
```

Let's list the contents of the folder. What happened to "newfolder"?  


## Finished and want to learn more?

Try taking some online tutorials:

* https://www.codecademy.com/learn/learn-the-command-line
* http://rik.smith-unna.com/command_line_bootcamp/
