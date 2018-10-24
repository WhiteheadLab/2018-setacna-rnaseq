Using the command line
===

## Goals
* Explain how shell relates to the operating system, and users’ programs.

* Explain when and why command-line interfaces should be used instead of graphical interfaces.

## Background
A shell is a program like any other. What’s special about it is that its job is to run other programs rather than to do calculations itself. The most popular Unix shell is Bash, the Bourne Again SHell (so-called because it’s derived from a shell written by Stephen Bourne). Bash is the default shell on most modern implementations of Unix and in most packages that provide Unix-like tools for Windows.

## What does it look like?

The shell __prompt__ allows you to interact with you computer and the files and folders present in it.

```{bash}
campus-009-192:~ eoziolor$
```

You can enter commands in shell that will allow you to execute actions.

```{bash}
campus-009-192:~ eoziolor$ ls
2018-setacna-rnaseq		Library				awscli-bundle			popgen_class
Applications			Movies				fgfh_post			power
Desktop				Music				jobs				training
Documents			Pictures			learn_python
Downloads			Public				phgenome_post
GIGAIII_bioinformatics_workshop	angus_private_key
```

You can modify the actions of commands (like ls) by using  __flags__ aka __options__

```{bash}
campus-009-192:~ eoziolor$ ls -lh /
total 13
drwxrwxr-x+ 67 root      admin   2.1K Oct 16 14:59 Applications
drwxr-xr-x+ 64 root      wheel   2.0K Oct  1 16:21 Library
drwxr-xr-x   2 root      wheel    64B Oct  1 16:17 Network
drwxr-xr-x@  5 root      wheel   160B Sep 20 21:05 System
drwxr-xr-x   6 root      admin   192B Oct  1 16:17 Users
drwxr-xr-x+  3 root      wheel    96B Oct 18 14:12 Volumes
drwxr-xr-x  22 eoziolor  staff   704B Aug 31 11:39 anaconda3
drwxr-xr-x@ 37 root      wheel   1.2K Sep 20 21:17 bin
drwxrwxr-t   2 root      admin    64B Oct  1 16:17 cores
dr-xr-xr-x   3 root      wheel   4.2K Oct 15 17:58 dev
lrwxr-xr-x@  1 root      wheel    11B Oct  1 16:16 etc -> private/etc
dr-xr-xr-x   2 root      wheel     1B Oct 18 12:52 home
-rw-r--r--   1 root      wheel   313B Aug 17 17:55 installer.failurerequests
dr-xr-xr-x   2 root      wheel     1B Oct 18 12:52 net
drwxr-xr-x   6 root      wheel   192B Oct  1 16:17 private
drwxr-xr-x@ 63 root      wheel   2.0K Oct  1 16:16 sbin
lrwxr-xr-x@  1 root      wheel    11B Oct  1 16:16 tmp -> private/tmp
drwxr-xr-x@  9 root      wheel   288B Sep 20 21:01 usr
lrwxr-xr-x@  1 root      wheel    11B Oct  1 16:16 var -> private/var
drwxr-xr-x   3 root      wheel    96B Oct  1 16:19 vm
```

In this case I am using the modifier -lh, to make ls (list) spit out a (l)ist of files/folders that is (h)uman readable in the directory /.

In order to find out how to use a command you can always use the command __man__ in front of whichever command you'd like to use. That will give you the (man)ual for that command.

```{bash}
LS(1)                     BSD General Commands Manual                    LS(1)

NAME
     ls -- list directory contents

SYNOPSIS
     ls [-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1] [file ...]

DESCRIPTION
     For each operand that names a file of a type other than directory, ls displays its name as well as any requested, associated information.
     For each operand that names a file of type directory, ls displays the names of files contained within that directory, as well as any
     requested, associated information.

     If no operands are given, the contents of the current directory are displayed.  If more than one operand is given, non-directory operands
     are displayed first; directory and non-directory operands are sorted separately and in lexicographical order.

     The following options are available:

     -@      Display extended attribute keys and sizes in long (-l) output.

     -1      (The numeric digit ``one''.)  Force output to be one entry per line.  This is the default when output is not to a terminal.

     -A      List all entries except for . and ...  Always set for the super-user.

     -a      Include directory entries whose names begin with a dot (.).

     -B      Force printing of non-printable characters (as defined by ctype(3) and current locale settings) in file names as \xxx, where xxx
             is the numeric value of the character in octal.

     -b      As -B, but use C escape codes whenever possible.

     -C      Force multi-column output; this is the default when output is to a terminal.

     -c      Use time when file status was last changed for sorting (-t) or long printing (-l).

     -d      Directories are listed as plain files (not searched recursively).

     -e      Print the Access Control List (ACL) associated with the file, if present, in long (-l) output.

     -F      Display a slash (`/') immediately after each pathname that is a directory, an asterisk (`*') after each that is executable, an at
             sign (`@') after each symbolic link, an equals sign (`=') after each socket, a percent sign (`%') after each whiteout, and a ver-
             tical bar (`|') after each that is a FIFO.

     -f      Output is not sorted.  This option turns on the -a option.

     -G      Enable colorized output.  This option is equivalent to defining CLICOLOR in the environment.  (See below.)
```

That way you can experiment with commands and use various options of them.

P.S.: To quit the man prompt you can just type "q"

## Usage

Command prompts are simple and are a language of their own. That means that unless you are percise and accurate about what you want to do - it will get done __poorly__!

Let's say that you forget a space between your command and the modifier you are using.

```{bash}
campus-009-192:~ eoziolor$ ls-F
-bash: ls-F: command not found
```

Bash has no idea what you mean and is likely not going to try to guess what it is. This is why you should always double, triple, quadruple check scripts that you write. One letter off can throw out a whole pipeline and sometimes you might not even know that it's happening.

### Advantages

Biggest advantage of using command line is that you know exactly what you are doing. This is not a black box in which a program is going to magically analyze your data and spit out results. If you're here, usually you know what your data looks like and format it needs to take to be received by the various programs of unix command-line.

Another big advantage of unix is that it uses __pipes__ (|). Basically that is the way for unix to rush the output from one command into another, so that you don't have to save any intermediates. We will chat more about this later, but overall that simplifies analyses __immensely__.

## Navigating Files and Directories

When you open the terminal you are immediately placed in a location on your computer. You can find where you are by running the command pwd (print working directory).

```{bash}
campus-009-192:~ eoziolor$ pwd
/Users/eoziolor
```

The home directory path will look different on different operating systems. On Linux it may look like /home/nelle, and on Windows it will be similar to C:\Documents and Settings\nelle or C:\Users\nelle.

You can navigate between the folders on your system using the command __cd__ (change directory).

Try using it to go into the Desktop folder.

```{bash}
campus-009-192:~ eoziolor$ cd Desktop/
campus-009-192:Desktop eoziolor$ pwd
/Users/eoziolor/Desktop
campus-009-192:Desktop eoziolor$ 
```

Now in order to get out of a directory you can type __cd__ followed by two periods. That will put you back in the directory behind yours.

```{bash}
campus-009-192:Desktop eoziolor$ cd ..
campus-009-192:~ eoziolor$ pwd
/Users/eoziolor
campus-009-192:~ eoziolor$ 
```

Let's say you wanted to move between further away directories, you could just append the same commands following each other.

```{bash}
campus-009-192:~ eoziolor$ cd Documents/UCD/
campus-009-192:UCD eoziolor$ pwd
/Users/eoziolor/Documents/UCD
campus-009-192:UCD eoziolor$ cd ../../
campus-009-192:~ eoziolor$ pwd
/Users/eoziolor
campus-009-192:~ eoziolor$ 
```

### Viewing hidden files

There are also files on your system that do not appear on normal list searches. They are called __hidden__ files and you can reveal them by appending -a at the end of ls.

```{bash}
campus-009-192:~ eoziolor$ ls -a
.				.gitconfig			.zoomus				angus_private_key
..				.ipynb_checkpoints		2018-setacna-rnaseq		awscli-bundle
.CFUserTextEncoding		.ipython			Applications			fgfh_post
.DS_Store			.jupyter			Desktop				jobs
.Rhistory			.matplotlib			Documents			learn_python
.Trash				.oracle_jre_usage		Downloads			phgenome_post
.atom				.python_history			GIGAIII_bioinformatics_workshop	phpopg
.bash_history			.rstudio-desktop		Library				popgen_class
.bash_profile			.ssh				Movies				power
.bash_sessions			.subversion			Music				training
.conda				.swp				Pictures
.cups				.viminfo	
```

### Absolute vs. Relative paths

There are two ways at getting to a folder. One way is to specify the absolute location of the folder:

```{bash}
campus-009-192:~ eoziolor$ cd /Users/eoziolor/Desktop/
campus-009-192:Desktop eoziolor$ pwd
/Users/eoziolor/Desktop
```

This will get you to the folder location no matter your starting location.

On the other hand, if you don't want to have to type the absolute path for very far files/folders, you can specify them from the location you are by just typing their relative location to you.

```{bash}
campus-009-192:~ eoziolor$ pwd
/Users/eoziolor

campus-009-192:~ eoziolor$ ls
2018-setacna-rnaseq		Library				awscli-bundle			popgen_class
Applications			Movies				fgfh_post			power
Desktop				Music				jobs				training
Documents			Pictures			learn_python
Downloads			Public				phgenome_post
GIGAIII_bioinformatics_workshop	angus_private_key		phpopg

campus-009-192:~ eoziolor$ cd Desktop/
campus-009-192:Desktop eoziolor$ pwd
/Users/eoziolor/Desktop
```

## Creating files and folders

### New folders

You can use the command mkdir (make directory) to make a folder in the directory you are.

```{bash}
campus-009-192:~ eoziolor$ mkdir blabla
campus-009-192:~ eoziolor$ ls
2018-setacna-rnaseq		Library				awscli-bundle			phpopg
Applications			Movies				blabla				popgen_class
Desktop				Music				fgfh_post			power
Documents			Pictures			jobs				training
Downloads			Public				learn_python
GIGAIII_bioinformatics_workshop	angus_private_key		phgenome_post
```

If you would like to delete a folder, you can use the command __rmdir__ (remove directory), but that only works if there is nothing in the folder.

To delete a folder with contents type rm -rf followed by the directory. __BE VERY CAREFUL__ with this! You cannot undo this! It does not go into trash, it just disappears __FOREVER__!

### Things to note about folder names

Here are some rules of thumb for creating new folders:
* don't have spaces in the name - it confuses bash
* name them something simple that you can remember
* avoid capital letters - bash is sensitive to them
* create structure in your directories


### New files and editing
In a similar manner you can create a new file with the command __touch__

```{bash}
campus-030-034:~ eoziolor$ touch document.txt
campus-030-034:~ eoziolor$ ls
2018-setacna-rnaseq		Library				awscli-bundle			phgenome_post
Applications			Movies				blabla				phpopg
Desktop				Music				document.txt			popgen_class
Documents			Pictures			fgfh_post			power
Downloads			Public				jobs				setac_private_key
GIGAIII_bioinformatics_workshop	angus_private_key		learn_python			training
campus-030-034:~ eoziolor$ 
```

#### Choose a text editor
__Do not open vim unless you're ready__

Let's start by opening the document we created with the test editor __nano__

```
nano document.txt
```

Now you can type anything that you want within that document.

```
hello
everyone
what
is
going
on
```

I will close the document by pressing __Cmd+x__ and follow the prompt to save the changes.

Now we have a couple of option in which we can look at this document. We can use the command __less__, often employed for bigger documents.

Advantages of __less__:
* only opens a chunk of the document to fill a page
* you can scroll up and down the document

Disadvantages of __less__:
* can't do much more than that in terms of interacting with the text

The other option we have is __cat__.
Advantages of __cat__:
* passes the text through many other programs
* can process zipped text with its sister program __zcat__
* just cool AF

Disadvantages of __cat__:
* your terminal will go crazy if you try to open a large document - type __Cmd+c__ to stop it if it goes unwieldy
* you don't get GUI-like interaction with the text in your document

Let's start by using __cat__ here:

```
cat document.txt 

hello
everyone
what
is
going
on
```

## Piping

Piping is one of the most useful thing in bash script. The unix shell was made to do something to a file and be able to unitize these commands to pass a certain __pipeline__.

The pipe basically means that I will do something to a file and the output of that I will pass to another program...to do whatever the other program is doing with it. The pipe symbol is __|__ and can work as following:

```{bash}
cat document.txt | head -n 3
hello
everyone
what
```

In this case what I'm doing is printing our file and piping the output into the __head__ commnad, which allows me to print only the first 3 lines.

## Grep, tr, wc, mv and many others

I can use an abundance of commands to now manipulate this text. Let's look at a slightly more complicated document and see what we can do.

Let's quickly download a small fasta file containing the _Fundulus heteroclitus_ transcriptome and play with it:

```{bash}
curl -O ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/826/765/GCF_000826765.1_Fundulus_heteroclitus-3.0.2/GCF_000826765.1_Fundulus_heteroclitus-3.0.2_rna.fna.gz
```
If you want to rename a file you can use mv (move)

```{bash}
mv GCF_000826765.1_Fundulus_heteroclitus-3.0.2_rna.fna.gz fhet.tr.fna.gz
```

Now let's look at the top of the file

```{bash}
zcat < fhet.tr.fna.gz | head
>NM_001309911.1 Fundulus heteroclitus low choriolytic enzyme-like (LOC105918466), mRNA
GGAAGGAAAAAATGGATCTCCAAGCACGAGCCTTGCTTCTGCTCCTGCTGCTTTCAGCCGTCTGTAATGCTTACCCCACA
GATAATTACAAAGCAGATGACGAAAACTCAGAGAAGGAGGACATCACAACCACTATCCTCAGAATGAACAATGGATCTGC
CGATATGCTGTTTGAAGGAGACGTTTTTGTTCCAAGATCCCGGACTGCCAAGAAGTGCCTTGATCCACGTTACAGCTGTT
TCTGGCCAAAGTCTTCAAATGGGAATGTGGAAATCCCTTTTGTTTTAAGTGACGAATATGATCACAACGAGAAGAATCAG
ATTCTCAAAGCCATGAAGGGCTTTGAGGGTAGAACCTGCATCCGCTTTGTTCGTCATAGAGGAGAGAGGGCGTACCTGAG
CATTGAGTCCAAATTTGGCTGTTTCTCTTTGATGGGTCGTTCTGGAGAAAGGCAGCTTGTGTCTCTGCAGAGACCCGGTT
GTTTAAATAATGGCATCATCCAGCATGAGCTGCTCCACGCTATGGGTTTCTACCACGAACACACTCGCAGCGACCGTGAC
AAATATGTCAAAATCAACTGGGATAACATACAAGAATATTATTATAAAAACTTCAAAAAAATGGACACAGACAATCTCAC
CCCATATGACTACTCCTCTGTGATGCAATATGGAAAAACTGCCTTTGGAAAGAACAGGGCAGAATCCATCACTCCTATCC
```

### Grep

Here's your normal fasta output. Now let's try to get some quick stats out of this.

```{bash}
zcat < fhet.tr.fna.gz | grep -c "^>"
41170
```

What I told the terminal is to grep, which captures a certain pattern in the document, to count (-c) how often this pattern occurs.

In this case I have counted the number of new lines that begin with __>__, which is every other line in a fasta document. What that tells me is that there are 41170 transcripts represented in this file.

### Tr

Notice that every > line starts with __NM\___. What if we want to get rid of all of these underscores and replace them with a dot? We can do this with __tr__.

```{bash}
zcat < fhet.tr.fna.gz | tr "_" "." | head
>NM.001309911.1 Fundulus heteroclitus low choriolytic enzyme-like (LOC105918466), mRNA
GGAAGGAAAAAATGGATCTCCAAGCACGAGCCTTGCTTCTGCTCCTGCTGCTTTCAGCCGTCTGTAATGCTTACCCCACA
GATAATTACAAAGCAGATGACGAAAACTCAGAGAAGGAGGACATCACAACCACTATCCTCAGAATGAACAATGGATCTGC
CGATATGCTGTTTGAAGGAGACGTTTTTGTTCCAAGATCCCGGACTGCCAAGAAGTGCCTTGATCCACGTTACAGCTGTT
TCTGGCCAAAGTCTTCAAATGGGAATGTGGAAATCCCTTTTGTTTTAAGTGACGAATATGATCACAACGAGAAGAATCAG
ATTCTCAAAGCCATGAAGGGCTTTGAGGGTAGAACCTGCATCCGCTTTGTTCGTCATAGAGGAGAGAGGGCGTACCTGAG
CATTGAGTCCAAATTTGGCTGTTTCTCTTTGATGGGTCGTTCTGGAGAAAGGCAGCTTGTGTCTCTGCAGAGACCCGGTT
GTTTAAATAATGGCATCATCCAGCATGAGCTGCTCCACGCTATGGGTTTCTACCACGAACACACTCGCAGCGACCGTGAC
AAATATGTCAAAATCAACTGGGATAACATACAAGAATATTATTATAAAAACTTCAAAAAAATGGACACAGACAATCTCAC
CCCATATGACTACTCCTCTGTGATGCAATATGGAAAAACTGCCTTTGGAAAGAACAGGGCAGAATCCATCACTCCTATCC
```

This becomes very useful for example if you have a comma separated value document and want to turn it into a tab separated value document. In that case you can use the command: 

```{bash}
tr "," "\t"
```
and now you have a tab separated value document.

### wc

Word count is a useful feature and we can use it in variety of ways.

```{bash}
zcat < fhet.tr.fna.gz | grep -v "^>" | wc -m
 131885065
```

What I've done above is I've used grep to remove any line that begins with __^>__ thus effectively only leaving the lines that have genetic code in them and not the names of each entry. Then I've told wc to give me a count of all the characters (-m) present in those lines, effectively giving me the size of the transcriptome (~131Mb).

I suggest using __man__ or __- -help__ to explore these commands further!
