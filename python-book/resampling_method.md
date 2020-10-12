---
jupyter:
  jupytext:
    metadata_filter:
      notebook:
        additional: all
        excluded:
        - language_info
    text_representation:
      extension: .Rmd
      format_name: rmarkdown
      format_version: '1.0'
      jupytext_version: 0.8.6
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
resampling_with:
    ed2_fname: 05-Chap-1
---

# The Resampling method {#resampling-method}

This chapter is a brief introduction to the resampling method of solving
problems in probability and statistics. A simple illustrative problem is
stated, and then the step-by-step solution with resampling is shown,
using both by-hand methods and a computer program. The older conventional
formulaic approach to such a problem is then discussed. The conventional
analytic method requires that you understand complex formulas, and too often
one selects the wrong formula. In contrast, resampling requires that you first
understand the physical problem fully. Then you simulate a statistical model of
the physical problem with techniques that are intuitively obvious, and you
estimate the probability with repeated random sampling.

## The resampling approach in action

Recall the problem from section \@ref(what-problems) in which the contractor
owns 12 ambulances. The chance that any one ambulance will be unfit for
service on any given day is about 1 in 10, based on past experience. You want
to know the probability that on a particular day---tomorrow--- *three or more*
ambulances will be out of action. The resampling approach produces the
estimate as follows.

### Randomness from physical methods

We collect 10 coins, and mark one of them with a pen or pencil or tape
as being the coin that represents "out-of-order;" the other nine coins
stand for "in operation." This set of 10 coins is a "model" of a
situation where there is a one-in-ten chance---a probability of .10 (10
percent)---of *one* particular ambulance being out-of-order on a given day.
Next, we put the coins into a little jar or bucket, draw out one coin, and
mark down whether or not that coin is the coin marked "out-of-order."
That drawing of the single coin from the bucket represents the chance that
any one given ambulance among our 12 ambulances (perhaps the one with the
lowest license-plate number) will be out-of-order tomorrow.

Then we put the drawn coin back in the bucket, shake all the coins, and
again draw out a coin. We mark down whether that second-drawing coin is
or is not the "out-of-order" coin, and that outcome stands for a second
ambulance in the fleet. We do this *12* times to represent our 12
ambulances, replacing the coin after each drawing, of course. Those 12
drawings represent one day.

At the end of the 12 draws we count how many out-of-orders we have
got for that "day," checking whether there are *three or more*
out-of-orders. If there are three or more, we write down in another
column "yes"; if not, we write "no." The work we have done up to now
represents one experimental trial of the model for a single day.

Then we repeat perhaps 50 or 100 times the entire experiment described
above. Each of those 50 or 100 experimental trials represents a single
day. When we have collected evidence for 50 or 100 experimental days, we
determine the proportion of the experimental days on which three or more
ambulances are out of order. That proportion is an estimate of the
probability that three or more ambulances will be out of order on a given
day---the answer we seek. This procedure is an example of Monte Carlo
simulation, which is the heart of the resampling method of statistical
estimation.

A more direct way to answer this question would be to examine the firm's
actual records for the past 100 days or 500 days to determine how many
days had three or more ambulances out of order. But the resampling procedure
described above gives us an estimate even if we do not have such
long-term information. This is realistic; it is frequently the case in
the workaday world that we must make estimates on the basis of
insufficient history about an event.

A quicker resampling method than the coins could be obtained with 12
ten-sided dice or spinners. Each one of the dice, marked with one of its
ten sides as "out-of-order," would indicate the chance of a single ambulance
being out of order on a given day. A single pass with the 12 dice or
spinners allows us to count whether three or more ambulances turn up out of
order. So in a single throw of the 12 dice we can get an
experimental trial that represents a single day. And in a hundred quick
throws of the 12 dice---which probably takes less than 5
minutes---we can get a fast and reasonably-accurate answer to our
question. But getting hold of ten-sided dice might be a nuisance.

## Randomness from your computer

Your computer gives you many convenient ways of getting random numbers for
resampling.  You can get random numbers from spreadsheet programs like Excel,
Numbers or LibreOffice [^excel-rand], or from websites like
<https://www.random.org>. We can use these numbers to simulate our problem.
For example, we can ask the computer to make us a random number between 0 and
9 (inclusive) to represent one ambulance. If we say that the digit
0 represents "out-of-order" and the digits 1 -- 9 represent "in operation,"
then any one random digit gives us a trial observation for a single ambulance.
To get an experimental trial for a single day we look at 12 digits and count
the number of zeros. If the number of zeros is three or more, then we write
"yes." We then look at one hundred or two hundred sets of 12 digits and count
the proportion of sets whose 12 digits show three or more ambulances being
"out-of-order." Once again, that proportion estimates the probability that
three or more ambulances will be out-of-order on any given day.

[^excel-rand]: For random numbers in spreadsheets, see the `RAND` and
  `RANDBETWEEN` functions in any of Excel, Numbers or LibreOffice.  But read on,
  and you will soon find that you have outgrown spreadsheets.

Soon we will do all these steps with some Python code, but for now,
let's say we have used one of the methods above to get 12 random numbers from
0 through 9, and we have done that 25 times, to simulate 25 days of 12
ambulances. We could arrange those numbers in a table like table
\@ref(tab:veh-numbers):


Table: (\#tab:veh-numbers)25 simulations of 12 ambulances

|   | T1| T2| T3| T4| T5| T6| T7| T8| T9| T10| T11| T12|
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|---:|---:|---:|
|1  |  6|  4|  5|  3|  5|  8|  4|  4|  7|   1|   6|   4|
|2  |  4|  1|  5|  3|  1|  2|  8|  5|  6|   4|   5|   8|
|3  |  5|  9|  4|  1|  0|  3|  7|  5|  0|   5|   1|   2|
|4  |  0|  8|  1|  4|  7|  0|  2|  9|  2|   1|   6|   9|
|5  |  2|  5|  3|  9|  0|  6|  5|  5|  2|   8|   1|   7|
|6  |  0|  0|  6|  5|  8|  9|  1|  5|  6|   9|   5|   4|
|7  |  6|  2|  0|  4|  0|  5|  0|  0|  1|   4|   6|   0|
|8  |  3|  6|  6|  7|  1|  7|  8|  2|  8|   5|   8|   4|
|9  |  2|  1|  6|  3|  3|  1|  8|  1|  2|   2|   0|   9|
|10 |  6|  4|  3|  0|  2|  6|  0|  5|  6|   9|   7|   5|
|11 |  0|  3|  1|  5|  4|  9|  9|  2|  3|   5|   1|   8|
|12 |  4|  2|  1|  4|  9|  3|  2|  7|  3|   8|   0|   6|
|13 |  3|  7|  9|  7|  6|  8|  8|  9|  8|   2|   8|   2|
|14 |  0|  6|  4|  8|  3|  5|  7|  6|  2|   7|   2|   7|
|15 |  7|  1|  6|  1|  9|  0|  4|  9|  1|   8|   8|   0|
|16 |  6|  3|  0|  8|  8|  9|  3|  5|  1|   9|   1|   9|
|17 |  9|  1|  4|  2|  3|  1|  4|  4|  5|   2|   0|   7|
|18 |  7|  5|  7|  2|  1|  7|  6|  9|  4|   7|   9|   4|
|19 |  2|  3|  0|  8|  1|  6|  2|  9|  7|   2|   0|   0|
|20 |  8|  9|  9|  4|  1|  9|  7|  1|  3|   4|   6|   3|
|21 |  3|  3|  2|  4|  5|  4|  2|  3|  8|   4|   5|   6|
|22 |  6|  9|  5|  4|  7|  1|  9|  7|  1|   5|   5|   0|
|23 |  4|  4|  5|  8|  0|  0|  8|  3|  1|   7|   6|   6|
|24 |  9|  6|  5|  4|  9|  8|  5|  7|  2|   9|   9|   1|
|25 |  9|  6|  6|  3|  6|  9|  1|  8|  9|   5|   9|   1|

To get the answer for each day, we count the number of zeros in each row.  The
counts go in the final column called "#0" (for "number of zeros").


Table: (\#tab:veh-numbers-counts)25 simulations of 12 ambulances, with counts

|   | T1| T2| T3| T4| T5| T6| T7| T8| T9| T10| T11| T12| #0|
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|---:|---:|---:|--:|
|1  |  6|  4|  5|  3|  5|  8|  4|  4|  7|   1|   6|   4|  0|
|2  |  4|  1|  5|  3|  1|  2|  8|  5|  6|   4|   5|   8|  0|
|3  |  5|  9|  4|  1|  0|  3|  7|  5|  0|   5|   1|   2|  2|
|4  |  0|  8|  1|  4|  7|  0|  2|  9|  2|   1|   6|   9|  2|
|5  |  2|  5|  3|  9|  0|  6|  5|  5|  2|   8|   1|   7|  1|
|6  |  0|  0|  6|  5|  8|  9|  1|  5|  6|   9|   5|   4|  2|
|7  |  6|  2|  0|  4|  0|  5|  0|  0|  1|   4|   6|   0|  5|
|8  |  3|  6|  6|  7|  1|  7|  8|  2|  8|   5|   8|   4|  0|
|9  |  2|  1|  6|  3|  3|  1|  8|  1|  2|   2|   0|   9|  1|
|10 |  6|  4|  3|  0|  2|  6|  0|  5|  6|   9|   7|   5|  2|
|11 |  0|  3|  1|  5|  4|  9|  9|  2|  3|   5|   1|   8|  1|
|12 |  4|  2|  1|  4|  9|  3|  2|  7|  3|   8|   0|   6|  1|
|13 |  3|  7|  9|  7|  6|  8|  8|  9|  8|   2|   8|   2|  0|
|14 |  0|  6|  4|  8|  3|  5|  7|  6|  2|   7|   2|   7|  1|
|15 |  7|  1|  6|  1|  9|  0|  4|  9|  1|   8|   8|   0|  2|
|16 |  6|  3|  0|  8|  8|  9|  3|  5|  1|   9|   1|   9|  1|
|17 |  9|  1|  4|  2|  3|  1|  4|  4|  5|   2|   0|   7|  1|
|18 |  7|  5|  7|  2|  1|  7|  6|  9|  4|   7|   9|   4|  0|
|19 |  2|  3|  0|  8|  1|  6|  2|  9|  7|   2|   0|   0|  3|
|20 |  8|  9|  9|  4|  1|  9|  7|  1|  3|   4|   6|   3|  0|
|21 |  3|  3|  2|  4|  5|  4|  2|  3|  8|   4|   5|   6|  0|
|22 |  6|  9|  5|  4|  7|  1|  9|  7|  1|   5|   5|   0|  1|
|23 |  4|  4|  5|  8|  0|  0|  8|  3|  1|   7|   6|   6|  2|
|24 |  9|  6|  5|  4|  9|  8|  5|  7|  2|   9|   9|   1|  0|
|25 |  9|  6|  6|  3|  6|  9|  1|  8|  9|   5|   9|   1|  0|

Each value in the last column of \@ref(tab:veh-numbers-counts) is the count of
zeros in that row, and therefore, the result from our simulation of one day.

We can estimate how often three or more ambulances would break down by looking for
values of three or greater in the last column.  We find there are
2 rows with three or more in the last column.
Finally we divide this number of rows by the number of trials (25)
to get an estimate of the *proportion* of days with three or more breakdowns.
The result is 0.08.

## Building the problem using Python

Here we rush ahead to show you how to do this simulation in Python.

In the text that follows we go through the Python code for the
simulation.  Please don't expect to understand this code right away.  The rest
of this book goes into more detail about how Python code works, and how
you can use Python to build your own simulations.  Here we want to show
you what this code looks like, to give you an idea of where we are headed.

For each line of the code below, we give a detailed explanation, but you can
also *run* the code yourself, to see it in action.  The link below allows you
to open this code as a *notebook*, and run it.  But, you may want to read the
next chapter "about the technology", to learn more about how to run this
notebook.  For now, concentrate on the text here, explaining what the code
means.

The core of the program to solve the trucks problem above begins with
this command to the computer:

`           GENERATE 20 1,10 a         `

This command orders the computer to randomly GENERATE twenty numbers
between "1" and "10." Inasmuch as each truck has a 1 in 10 chance of
being defective, we decide arbitrarily that a "1" stands for a defective
truck, and the other nine numbers (from "2" to "10") stand for a
non-defective truck. The command orders the computer to store the
results of the ran-

dom drawing in a location in the computer's memory to which we give a
name such as "A" or "BINGO."

The next key element in the core of the program is:

`           COUNT a =1 b         `

This command orders the computer to COUNT the number of "1's" among the
twenty numbers that are in location A following the random drawing
carried out by the GENERATE operation. The result of the COUNT will be
somewhere between 0 and 20, the number of trucks that might be
out-of-order on a given day. The result is then placed in another
location in the computer's memory that we label B.

Now let us place the GENERATE and COUNT commands within the entire
program that we use to solve this problem, which is:

`           REPEAT 400         `

Repeat the simulation 400 times

`           GENERATE 20 1,10 a         `

Generate 20 numbers, each between "1" and "10," and put them in vector
a. Each number will represent a truck, and we let 1 represent a
defective truck.

`           COUNT a =1 b         `

Count the number of defective trucks, and put the result in vector b.

`           SCORE b z         `

Keep track of each trial's results in z.

`           END         `

End this trial, then go back and repeat the process until all 400 trials
are complete, then proceed.

`           COUNT z > 3 k         `

Determine how many trials resulted in more than 3 trucks out of order.
(This can also be written COUNT z =\> 4 k, for 4 or more out of order.)

`           DIVIDE k 400 kk         `

Convert to a proportion.

`           PRINT kk         `

Print the result.

Note: The file "trucks" on the Resampling Stats software disk contains
this set of commands.

The SCORE statement that follows the COUNT operation simply keeps track
of the results of each trial, placing the number of defective trucks
that occur in each trial in a location that we usually call "z." This is
done in each of the 400 trials that we make, and the result eventually
is a "vector" with 400 numbers in it.

In order to make 400 repetitions of our experiment---we could have
decided to make a thousand or some other number of repetitions---we put
REPEAT 400 before the GENERATE, COUNT, and SCORE statements that
constitute a single trial. Then we complete each repetition "loop" with
END.

Since our aim is to count the number of days in which more than 3 (4 or
more) defective trucks occur, we use the COUNT command to count how many
times in the 400 days recorded in our SCORE vector at the end of the 400
trials more than 3 defects occurred, and we place the result in still
another location "k." This gives us the total number of days where 4 or
more defective trucks are seen to occur. Then we DIVIDE the number in
"k" by 400, the number of trials. Thus we obtain an estimate of the
chance, expressed as a probability between 0 and 1, that 4 or more
trucks will be defective on a given day. And we store that result in a
location that we decide to call "kk," so that it will be there when the
computer receives the next command to PRINT that result on the screen.

Can you see how each of the operations that the computer carries out are
analogous to the operations that you yourself executed when you solved
this problem using a ten-sided spinner or a random-number table? This is
exactly the procedure that we will use to solve every problem in
probability and statistics that we must deal with. Either we will use a
device such as coins or a random number table as an analogy for the
physical process we are interested in (trucks becoming defective, in
this case), or we will simulate the analogy on the computer using the
RESAMPLING STATS program.

Simple as it is, the RESAMPLING STATS program called "TRUCKS" may not
seem simple to you at first glance. But it is vastly simpler than the
older conventional approach to such problems that has routinely been
taught to students for decades.


## How resampling differs from the conventional approach

In the standard approach the student learns to choose and solve a
formula. Doing the algebra and arithmetic is quick and easy. The
difficulty is in choosing the correct formula. Unless you are a
professional mathematician, it may take you quite a while to arrive at
the correct formula---considerable hard thinking, and perhaps some
digging in textbooks. More important than the labor, however, is that
you may come up with the wrong formula, and hence obtain the wrong
answer. Most students who have had a standard course in probability and
statistics are quick to tell you that it is not easy to find the correct
formula, even immediately after finishing a course (or several courses)
on the subject. After leaving school, it is harder still to choose the
right formula. Even many people who have taught statistics at the
university level (including this writer) must look at a book to get the
correct formula for a problem as simple as the ambulances, and then we are
not always sure of the right answer. This is the grave disadvantage of
the standard approach.

In the past few decades, resampling and other Monte Carlo simulation
methods have come to be used extensively in scientific research. But in
contrast to the material in this book, simulation has mostly been used
in situations so complex that mathematical methods have not yet been
developed to handle them. Here are examples of such situations:

<!---
Better examples.  These are out of date.
-->

1.  For a spaceship that will travel to Mars, calculating the correct flight
    route involves a great many variables, too many to solve with formulas.
    Hence, the Monte Carlo simulation method is used.

2.  The Navy might want to know how long the average ship will have to
    wait for dock facilities. The time of completion varies from ship to
    ship, and the number of ships waiting in line for dock work varies
    over time. This problem can be handled quite easily with the
    experimental simulation method, but formal mathematical analysis
    would be difficult or impossible.

3.  What are the best tactics in baseball? Should one bunt? Should one
    put the best hitter up first, or later? By trying out various
    tactics with dice or random numbers, Earnshaw Cook (in his book
    *Percentage Baseball* ), found that it is best never to bunt, and
    the highest-average hitter should be put up first, in contrast to
    usual practice. Finding this answer would have been much more difficult
    with the analytic method.

4.  Which search pattern will yield the best results for a ship
    searching for a school of fish? Trying out "models" of various
    search patterns with simulation can provide a fast answer.

5.  What strategy in the game of Monopoly will be most likely to win?
    The simulation method systematically plays many games (with a
    computer) testing various strategies to find the best one.

But those five examples are all complex problems. This book and its
earlier editions break new ground by using this method for *simple
rather than complex problems* , especially in statistics rather than
pure probability, and in teaching *beginning rather than advanced*
students to solve problems this way. (Here it is necessary to emphasize
that the resampling method is used to *solve the problems themselves
rather than as a demonstration device to teach the notions found in the
standard conventional approach* . Simulation has been used in elementary
courses in the past, but only to demonstrate the operation of the
analytical mathematical ideas. That is very different than using the
resampling approach to solve statistics problems themselves, as is done
here.)

Once we get rid of the formulas and tables, we can see that statistics
is a matter of *clear thinking, not fancy mathematics* . Then we can get
down to the business of learning how to do that clear statistical
thinking, and putting it to work for you. *The study of probability* is
purely mathematics (though not necessarily formulas) and technique. But
*statistics has to do with meaning* . For example, what is the meaning
of data showing an association just discovered between a type of
behavior and a disease? Of differences in the pay of men and women in
your firm? Issues of causation, acceptability of control, design of
experiments cannot be reduced to technique. This is "philosophy" in the
fullest sense. Probability and statistics calculations are just one
input. Resampling simulation enables us to get past issues of
mathematical technique and focus on the crucial statistical elements of
statistical problems.

If you intend to go on to advanced statistical work, the older standard
method can be learned alongside resampling methods. Your introduction to
the conventional method may thereby be made much more meaningful.

