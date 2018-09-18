## Murder Mystery Steps
**Read instructions**
`cat instructions`

```
There's been a murder in Terminal City, and TCPD needs your help.

To figure out whodunit, go to the 'mystery' subdirectory and start working from there.

You'll want to start by collecting all the clues at the crime scene (the 'crimescene' file).

The officers on the scene are pretty meticulous, so they've written down EVERYTHING in their officer reports.

Fortunately the sergeant went through and marked the real clues with the word "CLUE" in all caps.

If you get stuck, open one of the hint files (from the CL, type 'cat hint1', 'cat hint2', etc.).

To check your answer or find out the solution, open the file 'solution' (from the CL, type 'cat solution').

To get started on how to use the command line, open cheatsheet.md or cheatsheet.pdf (from the command line, you can type 'nano cheatsheet.md').

Don't use a text editor to view any files except these instructions, the cheatsheet, and hints.
```

**Fortunately the sergeant went through and marked the real clues with the word "CLUE" in all caps.**

`grep 'CLUE' mystery/crimescene`

```
CLUE: Footage from an ATM security camera is blurry but shows that the perpetrator is a tall male, at least 6'.

CLUE: Found a wallet believed to belong to the killer: no ID, just loose change, and membership cards for AAA, Delta SkyMiles, the local library, and the Museum of Bash History. The cards are totally untraceable and have no name, for some reason.

CLUE: Questioned the barista at the local coffee shop. He said a woman left right before they heard the shots. The name on her latte was Annabel, she had blond spiky hair and a New Zealand accent.
```

**Grep Annabel from the people file**
`grep 'Annabel' mystery/people`

```
Annabel Sun F 26 Hart Place, line 40

Oluwasegun Annabel M 37 Mattapan Street, line 173

Annabel Church F 38 Buckingham Place, line 179

Annabel Fuglsang M 40 Haley Street, line 176
```

**Find the interview id through the address place**

`grep 'INTERVIEW' mystery/streets/Hart_Place //SEE INTERVIEW #47246024`

or

`cat mystery/streets/Hart_Place | sed '40!d' //SEE INTERVIEW #47246024`

**Read the interview**

`cat mystery/interviews/interview-47246024`
```
//Ms. Sun has brown hair and is not from New Zealand.  Not the witness from the cafe.
Go through all the Annabel names(bold) to find the proper interview reaction

Interviewed Ms. Church at 2:04 pm.
Witness stated that she did not see anyone she could identify as the shooter,
that she ran away as soon as the shots were fired.

However, she reports seeing the car that fled the scene.
Describes it as a blue Honda, with a license plate that starts with "L337" and ends with "9"
Filter through the vehicle file to find the vehicle that starts with 'L337' , type : 'Honda' and color: 'Blue'
```

`grep -A 5 'L337*' mystery/vehicles | grep -A 5 'Honda' | grep -A 5 'Blue'`
```
Owner	            Height	    Meet Height Requirement of at least 6'
Erika Owens	      6'5"	      Yes
Aron Pilhofer	    5'3"	      No
Heather Billings	5'2"	      No
Joe Germuska	    6'2"	      Yes
Jeremy Bowers	    6'1"	      Yes
Jacqui Maher	    6'2"	      Yes
```

**Grep owner's name to find address and use that address to retrieve the interview ID**

`grep 'Jeremy Bowers' mystery/people`
```
Jeremy Bowers    M    34    Dunstable Road, line 284
```

**Cat the file for the specific line number provided to retrieve the interview ID**

`cat mystery/streets/Dunstable_Road | sed '284!d'`
```
SEE INTERVIEW #9620713
```

**Cat the interview ID**

`cat mystery/interviews/interview-9620713`

```
Home appears to be empty, no answer at the door.
After questioning neighbors, appears that the occupant may have left for a trip recently.
Considered a suspect until proven otherwise, but would have to eliminate other suspects to confirm.
```

**Check the answer**

`cat solutions`


**Checking Your Answer on Mac/Linux:**

```
Copy and paste the following command, replace John Doe with the suspect's name you want to check,
and execute it from inside the main clmystery directory:
```

```
echo "Jeremy Bowers" | $(command -v md5 || command -v md5sum) | grep -qif /dev/stdin encoded &&
echo CORRECT\! GREAT WORK, GUMSHOE. || echo SORRY, TRY AGAIN.
```

`CORRECT! GREAT WORK, GUMSHOE.`
