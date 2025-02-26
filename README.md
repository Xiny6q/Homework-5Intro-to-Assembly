Download link :https://programming.engineering/product/homework-5intro-to-assembly/


# Homework-5Intro-to-Assembly
Homework 5Intro to Assembly
1.1 Debugging Assembly

For information on debugging or the autograder, jump to section 5. Please note that Docker must be running, but it doesn’t need to be within the image.

1.2 Modulus

Given values x and mod, calculate x % mod and store this at the address ANSWER. The pseudocode can be found in section 3.2.

Relevant labels:

X and MOD where you can load the inputs from.

ANSWER where you will store your result, x mod m.

Say x = 17 and mod = 5. After running your program, mem[ANSWER] should equal 2.

1.3 Build Maximum Array

Given two competing arrays A and B of the same length, fill in a third array located C. For each index, if A[i] ≥ B[length – i – 1], set C[i] = 1. Otherwise, set C[i] = 0. Store these values in the array represented by C. The pseudocode can be found in section 3.3.

Relevant labels:

A which holds the address of A.

B which holds the address of B.

C which holds the address of C, your solution array.

LENGTH which holds the size of the arrays.

Say A is [-2, 2, 1] and B is [1, 0, 3]. After running your program, C should be [0, 1, 1].

1.4 Octal String to Int

You are given an unsigned octal number encoded as a string. Translate this string to its numerical value. The value stored at RESULTADDR represents another different address. Store your numerical value result at this different address. The pseudocode can be found in section 3.4.

Relevant labels:

OCTALSTRING which holds the starting address of the octal string

LENGTH which holds the length of the octal string

RESULTADDR which holds the address of where you will store your numerical value result.

ASCII which holds the value -48

Say the string at OCTALSTRING was “2110”. After running your program, mem[mem[ANSWERADDR]] should equal 1096 (base 10).

1.5 Palindrome

Given a string, calculate if it’s a palindrome. ANSWERADDR should contain another address. At this address, store true if the string is palindrome and false if it is not. Let’s agree to interpret a 1 in memory as true, and a 0 in memory as false. The pseudocode can be found in section 3.5.

Relevant labels:

STRING which holds the address of the string that you will evaluate

ANSWERADDR which holds the address of where you should store the boolean result.

Say the provided string is “aibohphobia”. After running your program, mem[mem[ANSWERADDR]] should equal true.

Recall that we represent the end of a string with a null terminator, which is the numerical value 0.

1.6 Advice from your TAs

When converting pseudocode into assembly, become human compilers and translate one line of pseu-docode into one (or more) lines of assembly. One line at a time. Don’t look at the pseudocode and try to optimize things in your head and write the entire function. Trust us when we say translating one line at a time is the way. So, put on blinders. Ignore previous and future lines of code.

Don’t assume a register still retains a value from a previous LD.

Don’t assume the condition code reflects the register you think it does (NZP).

Checkout the Appendix for the ASCII table and other helpful resources

The local autograder is a shell script which should be run in your local terminal (not the docker terminal).

The above strategy ofcourse produces inefficient code. But, it produces correct assembly code, every time. Not clever code. Not optimized code. Not short code. But correct code. So, if you ever find it difficult to write assembly, just go at it one step at a time :)

psst..also you could come to office hours or post on ed discussion but definitely try that advice first :DD

Overview

2.1 Purpose

So far in this class, you have seen how binary or machine code manipulates our circuits to achieve a goal. However, as you have probably figured out, binary can be hard for us to read and debug, so we need an easier way of telling our computers what to do. This is where assembly comes in. Assembly language is symbolic machine code, meaning that we don’t have to write all of the ones and zeros in a program, but rather symbols that translate to ones and zeros. These symbols are translated with something called the assembler. Each assembler is dependent upon the computer architecture on which it was built, so there are many different assembly languages out there. Assembly was widely used before most higher-level languages and is still used today in some cases for direct hardware manipulation.

2.2 Task

The goal of this assignment is to introduce you to programming in LC-3 assembly code. This will involve writing small programs, translating conditionals and loops into assembly, modifying memory, manipulating strings, and converting high-level programs into assembly code.

You will be required to complete the four functions listed below with more in-depth instructions on the following pages:

modulus.asm

buildMaxArray.asm

octalStringToInt.asm

palindrome.asm

The assignment is due by 11:59 PM on February 28th, 2023. You could also submit the assignment by March 1st, 2023 for a late penalty of 25% of your overall grade.

2.3 Criteria

Your assignment will be graded based on your ability to correctly translate the given pseudocode into LC-3 assembly code. Check the deliverables section for deadlines and other related information. Please use the LC-3 instruction set when writing these programs. More detailed information on each instruction can be found in the Patt/Patel book Appendix A (also on Canvas under “LC-3 Resources”). Please check the rest of this document for some advice on debugging your assembly code, as well some general tips for successfully writing assembly code.

You must obtain the correct values for each function. While we will give partial credit where we can, your code must assemble with no warnings or errors (Complx will tell you if there are any). If your code does not assemble, we will not be able to grade that file and you will not receive any points. Each function is in a separate file, so you will not lose all points if one function does not assemble. Good luck and have fun!

Detailed Instructions

3.1 Notes

Assume all inputs will enforce the assumptions.

The provided pseudocode will naturally account for all assumptions and edge cases.

Make sure you conceptually understand what labels are and what using them really means behind the scenes. All problems will revolve around using labels to load inputs and store outputs.

Be careful changing anything outside the first .orig x3000 and HALT lines in every file. Watch out for the instructions in each file to know what you can and cannot change for testing. Your autograder’s functionality will depend on some of the initial code we provided.

The algorithms presented for each operation are not meant to be the most efficient.

Be wary of the differences between instructions like LD and LEA. When you have an answer, make sure you’re storing to the correct address. Trace through your code on Complx if you’re not sure if you’re using the correct instruction.

Debugging via Complx helps tremendously. Eyeballing assembly code can prove to be very difficult. It helps a lot to be able to trace through your code step-by-step, line-by-line, to see if each assembly instruction does what you expected.

You can check if far-away addresses contain expected values in Complx by going to View >> GoTo Address.

3.2 Part 1: Modulus

Given a values x and mod, calculate x % mod and store this at the address ANSWER.

Assumptions:

x ≥ 0

mod > 0

Relevant labels:

X and MOD where you can load the inputs from.

ANSWER where you will store your result, x mod m.

Say x = 17 and mod = 5. After running your program, mem[ANSWER] should equal 2.

Notice this problem involves a while loop, which means your assembly program will need to know how rewind backwards to the top of the loop. Perhaps there are instructions we can use to directly change the PC and help our program shift around our code.

Suggested Pseudocode:

int x = 17;

int mod = 5;

while (x >= mod) {

x -= mod;

}

mem[ANSWER] = x;

3.3 Part 2: Build Maximum Array

Given two competing arrays A and B of the same length, fill in a third array C. For each index, if A[i]≥ B[length – i – 1], set C[i] = 1. Otherwise, set C[i] = 0.

Assumptions:

All provided arrays will have the same length. We provide this length at label LENGTH.

Array C has already been reserved sufficient memory at labelC.

Relevant labels:

A which holds the address of A.

B which holds the address of B.

C which holds the address of C, your solution array.

LENGTH which holds the size of the arrays.

Say A is [-2, 2, 1] and B is [1, 0, 3]. After running your program, C should equal [0, 1, 1].

C[0] should be 0 because -2 ≱ 3

C[1] should be 1 because 2 ≥ 0

C[2] should be 1 because 1 ≥ 1

You’ll notice that each array’s label stores a value written in hexadecimal. Perhaps these values should be interpreted as memory addresses. Recall how we agreed to represent arrays in memory.

How would you load a specific array value in assembly? If the first value of A is located at address x3200, think about where the second or third value of A could be located at.

Implement your assembly code in buildMaxArray.asm

Suggested Pseudocode:

int A[] = {-2, 2, 1};

int B[] = {1, 0, 3};

int C[3];

int length = 3;

int i = 0;

while (i < length) {

if (A[i] >= B[length – i – 1]) {

C[i] = 1;

}

else {

C[i] = 0;

}

i++;

}

3.4 Part 3: Octal String to Int

Given an octal number encoded as a string, translate it to its numerical value. The value stored at RESULTADDR represents another different address. Store your numerical value result at this different ad-dress.

Assumptions:

‘0’ ≤ octalString[i] ≤ ‘7’

The provided octal string will be nonnegative.

We do not have to worry about overflow.

Relevant labels:

OCTALSTRING which holds the starting address of the octal string.

LENGTH which holds the length of the octal string.

RESULTADDR which holds the address of where you will store your numerical value result.

ASCII which holds the value -48.

Say the string at OCTALSTRING was “2110”. After running your program, mem[mem[RESULTADDR]] should equal 1096 (base 10). If RESULTADDR = x4000, then this means the value all the way at memory address x4000 should be changed to 1096. The value at RESULTADDR should be unchanged.

Recall how we interpret characters using ASCII (ASCII table listed in Appendix). You might find the ASCII label useful.

Implement your assembly code in octalStringToInt.asm

Suggested Pseudocode:

String octalString = “2110”;

int length = 4;

int value = 0;

int i = 0;

while (i < length) {

int leftShifts = 3;

while (leftShifts > 0) {

value += value;

leftShifts–;

}

int digit = octalString[i] – 48;

value += digit;

i++;

}

mem[mem[RESULTADDR]] = value;

3.5 Part 4: Palindrome

Given a string, calculate if it’s a palindrome. ANSWERADDR should contain another address. At this address, store true if the string is palindrome and false if it is not.

Assumptions:

‘a’ ≤ str[i] ≤ ‘z’

We still consider empty strings as palindromes.

We agree that a 1 in memory represents true and a 0 represents false.

Relevant labels:

STRING which holds the address of the string that you will evaluate.

ANSWERADDR which holds the address of where you should store the boolean result.

Say the provided string is “aibohphobia”. After running your program, mem[mem[ANSWERADDR]] should equal true.

Notice we are only given the starting address our string. How do we know which address the string ends? Let’s agree that if we ever read a numerical 0 value, then the string ends. If you take a look at an ASCII table (listed in Appendix), you can see the numerical value 0 is interpreted as NULL, which by convention denotes the end of a string.

NOTE: In the pseudocode below, ‘\0’ means the numerical value 0. It is different from ‘0’, which is the numerical value 48.

Implement your assembly code in palindrome.asm

Suggested Pseudocode:

String str = “aibohphobia”;

boolean isPalindrome = true

int length = 0;

while (str[length] != ‘\0’) {

length++;

}

int left = 0

int right = length – 1

while(left < right) {

if (str[left] != str[right]) {

isPalindrome = false;

break;

}

left++;

right–;

}

mem[mem[ANSWERADDR]] = isPalindrome;

Deliverables

Turn in the following files on Gradescope:

modulus.asm

buildMaxArray.asm

octalStringToInt.asm

palindrome.asm

Note: Try to start homeworks early. It will be easier to get help if you get stuck, and last minute turn-ins will result in long queue times for grading on Gradescope.

Running the Autograder and Debugging LC-3 Assembly

When you turn in your files on Gradescope for the first time, you may not receive a perfect score. Does this mean you change one line and spam Gradescope until you get a 100? No! You can use a handy Complx feature called “replay strings”.

First off, we can get these replay strings in two places: the local grader, or off of Gradescope. To run the local grader:

Mac/Linux Users:

Navigate to the directory your homework is in (in your terminal on your host machine, not in the Docker container via your browser)

Run the command sudo chmod +x grade.sh

Now run ./grade.sh

Windows Users:

In Git Bash (or Docker Quickstart Terminal for legacy Docker installations), navigate to the directory your homework is in

Run chmod +x grade.sh

Run ./grade.sh

When you run the script, you should see an output like this:



Copy the string, starting with the leading ’B’ and ending with the final backslash. Do not include the quotation marks.



2. Secondly, navigate to the clipboard in your Docker image and paste in the string.



3. Next, go to the Test Tab and click Setup Replay String



4. Now, paste your tester string in the box!



Now, Complx is set up with the test that you failed! The nicest part of Complx is the ability to step through each instruction and see how they change register values. To do so, click the step button. To change the number representation of the registers, double click inside the register box.



If you are interested in looking how your code changes different portions of memory, click the view tab and indicate ’New View’



Now in your new view, go to the area of memory where your data is stored by CTRL+G and insert the address



One final tip: to automatically shrink your view down to only those parts of memory that you care about (instructions and data), you can use View Tab → Hide Addresses → Show Only Code/Data.



Appendix

6.1 Appendix A: ASCII Table



Figure 1: ASCII Table — Very Cool and Useful!

6.2 Appendix B: LC-3 Instruction Set Architecture



6.3 Appendix C: LC-3 Assembly Programming Requirements and Tips

Your code must assemble with NO WARNINGS OR ERRORS. To assemble your program, open the file with Complx. It will complain if there are any issues. If your code does not assemble you WILL get a zero for that file.

Comment your code! This is especially important in assembly, because it’s much harder to interpret what is happening later, and you’ll be glad you left yourself notes on what certain instructions are contributing to the code. Comment things like what registers are being used for and what less intuitive lines of code are actually doing. To comment code in LC-3 assembly just type a semicolon (;), and the rest of that line will be a comment.

Avoid stating the obvious in your comments, it doesn’t help in understanding what the code is doing.

Good Comment

ADD R3, R3, -1 ; counter–

BRp LOOP ; if counter == 0 don’t loop again

Bad Comment

ADD R3, R3, -1 ; Decrement R3

BRp LOOP ; Branch to LOOP if positive

DO NOT assume that ANYTHING in the LC-3 is already zero. Treat the machine as if your program was loaded into a machine with random values stored in the memory and register file.

Following from 3, you can load the file with randomized memory by selecting “File” ⇒ “Advanced Load” and selecting randomized registers/memory.

Do NOT execute any data as if it were an instruction (meaning you should put .fills after HALT or RET). All your program does is interpret the values RAM as instructions until it reaches HALT. If you use .fill before your program HALTS, your program might interpret what you filled as an instruction and try to execute it!

Do not add any comments beginning with @plugin or change any comments of this kind.

Test your assembly. Don’t just assume it works and turn it in.

When translating pseudocode into assembly, don’t skip over the closing brackets! Even though they’re only one character long, perhaps they also might need to be translated into assembly…

Rules and Regulations

7.1 General Rules

Although you may ask TAs for clarification, you are ultimately responsible for what you submit. As such, please start assignments early, and ask for help early. This means that (in the case of demos) you should come prepared to explain to the TA how any piece of code you submitted works, even if you copied it from the book or read about it on the internet.

If you find any problems with the assignment it would be greatly appreciated if you reported them to the author (which can be found at the top of the assignment). Announcements will be posted if the assignment changes.

7.2 Submission Conventions

Do not submit links to files. The autograder does not understand it, and we will not manually grade assignments submitted this way as it is easy to change the files after the submission period ends. You must submit all files listed in theDeliverables section individually to Gradescope as separate files.

7.3 Submission Guidelines

You are responsible for turning in assignments on time. This includes allowing for unforeseen circum-stances. If you have an emergency let us know IN ADVANCE of the due time supplying documenta-tion (i.e. note from the dean, doctor’s note, etc). Extensions will only be granted to those who contact us in advance of the deadline and no extensions will be made after the due date.

You are also responsible for ensuring that what you turned in is what you meant to turn in. After submitting you should be sure to download your submission into a brand new folder and test if it works. No excuses if you submit the wrong files, what you turn in is what we grade. In addition, your assignment must be turned in via Canvas/Gradescope. Under no circumstances whatsoever we will accept any email submission of an assignment. Note: if you were granted an extension you will still turn in the assignment over Canvas/Gradescope.

7.4 Syllabus Excerpt on Academic Misconduct

Academic misconduct is taken very seriously in this class. Quizzes, timed labs and the final examination are individual work.

Homework assignments are collaborative, In addition many if not all homework assignments will be evaluated via demo or code review. During this evaluation, you will be expected to be able to explain every aspect of your submission. Homework assignments will also be examined using computer programs to find evidence of unauthorized collaboration.

What is unauthorized collaboration? Each individual programming assignment should be coded by you. You may work with others, but each student should be turning in their own version of the assignment. Submissions that are essentially identical will receive a zero and will be sent to the Dean of Students’ Office of Academic Integrity. Submissions that are copies that have been superficially modified to conceal that they are copies are also considered unauthorized collaboration.

You are expressly forbidden to supply a copy of your homework to another student via elec-tronic means. This includes simply e-mailing it to them so they can look at it. If you supply an electronic copy of your homework to another student and they are charged with copying, you will also be charged. This includes storing your code on any site which would allow other parties to obtain your code such as but not limited to public repositories (Github), pastebin, etc. If you would like to use version control, use github.gatech.edu

7.5 Is collaboration allowed?

Collaboration is allowed on a high level, meaning that you may discuss design points and concepts relevant to the homework with your peers, share algorithms and pseudo-code, as well as help each other debug code. What you shouldn’t be doing, however, is pair programming where you collaborate with each other on a single instance of the code. Furthermore, sending an electronic copy of your homework to another student for them to look at and figure out what is wrong with their code is not an acceptable way to help them, because it is frequently the case that the recipient will simply modify the code and submit it as their own.
