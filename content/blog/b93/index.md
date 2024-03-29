---
title: "Quizme Development"
date: 2018-12-25T18:34:58-06:00
draft: FALSE
tags: ["R"]
---
### Overall Structure
At the beginning following data structures are created (or read if these exist
from an earlier session):

**qtbl** (*question table*): a data.table containing questions uniquely identified by *id* column

**soltbl** (*solution table*): a list containing answers again uniquely identified by *id* column

**testlog** (*test log*): a data.table that logs user responses each time they take the quiz.
This contains columns on when the question was asked, how much time it
took to answer and whether you knew the answer or not

**ranktbl** (*ranking table*): a data.table that ranks/orders the questions based on
testlog data. This contains a *status* column based on which further control on 
ranking is established

**slog** (*session log*): a data.table containing info on each quiz session

### User Functions

`quizme()`: This creates (or reads into memory) all the data structures described
in the above section.

`addq()`: Updates the *qtbl* and *soltbl* objects. User input is collected using `scan()`
function and stored in a character vector. First element (assumed as question) of 
this vector is added to *qtbl* while the rest (assumed as single or multiline answer)
is added to *soltbl*. *ranktbl* is updated with this question using `addToRankingTbl()`
and is added to a subset of *new* questions with year 2100 as due date to force it 
to the bottom of the list. This enables the user to add as many new questions as they want
without getting overwhelmed with more test questions.

**NOTE**: Although addq() takes a character argument (tags) which will be added to qtbl
column, I have found and employed a more concise way to add tags: start the question
with a single letter followed by colon. For example, *r: question* can be used to
add an R language question. It has the following advantages:
1. It forces you to organize your tags in a more meaningful way. I think few tags are
   better than none or too many.
2. Allows you to add the tags at the same time when adding a question with couple of 
   extra keystrokes.

**NOTE**: Adding images as answers is also allowed. This is done by adding the 
text *image* to the answer and storing the image file as *<id.png>* in `./images`
directory.

`ask()`: Prompts the user with a question. The question belongs to a subset of
*ranktbl* rows that are due *today*. Also, *learning* questions are given higher priority
than the *new* questions. Within the subclass of learning and new questions, shuffling 
is done (using `sample()`) each time a question is asked. If there are no more 
questions that are due on a given day, user is prompted to add more questions if they
wish to do so using a call to `newMove()`.

`tell()`: Displays the answer to the last question asked. If the answer was in 
image format, that image is displayed in R's graphics window.

`hit()`: Response to the currently displayed question if the user answered the
question correctly. *testlog* table is updated with this information. In addition,
it also calls `updatetime()` which updates the ranktbl with the new due date/time for
this question.

`miss()`: Response to the currently displayed question if the user did not answered 
the question correctly. *testlog* table is updated with this infomation.

`bye()`: To close the quiz session and update all the data tables. This is 
automatically called at the end of the quiz session. This can also be called
in the middle of the quiz session as well. This makes it adaptive to the time available:
if you just have 1 or 2 minutes, you can practice a few questions, and if you have
more time, you can work to finish the quiz in one session.

`changeq()`: To edit the currently asked question. You may need this in situation where either simply there was a typo in the question/answer or you later learned a better answer that you want to replace your older answer with.

`show_status()`: This shows a summary of your learning including total number of questions
in your quiz repository categorized by learning status (new, learning, learned, mastered).
These are also shown by topic. Number of questions due today and tomorrow is also
displayed.

`week_ahead()`: Displays the number of questions due for each day in the next seven days.
This helps you plan your week in terms of scheduling more or less time for the quiz.

### Backend Functions

`addToRankingTbl()`: This function adds the new question to *ranktbl* and then calls
`updateranktbl()`.

`updateranktbl()`: Reorders the questions in *ranktbl* by due date, then by status and 
finally by rank.

**NOTE**: ordering by rank was used originally to make it a function of the order in 
which the questions are added. Now that the questions are added randomly from a *pool of
new questions*, rank column has become redundant. This will be removed in a later version.

`updatetime()`: This is where the spaced repetetion concept is implemented.
This function calculates the interval as to when the correct question will be asked again.
It uses the following logic to carry out the task:

For a new question and first correct response, time is not updated but rank is set to the
   highest. This is done to present the brand new question twice in the same session
   for better memorization.

   **NOTE**: Here again, setting the rank is redundant. Earlier it was done to push this
   question to the bottom of the current list so it is not asked immediately again. Now
   that shuffling is carried out at the time of asking the question, this is not needed.

If a new question is answered correctly twice, 4 hours is added to its due column. This
   is done to push the question to the next session (assuming a session does not last longer
   than 4 hours).

If the new question is answered correctly again in the second session, 
   - 24 hours are added to its due time
   - status is changed from *new* to *learning*

If a *learning* question is answered correctly, it calculates the interval based on 
   two times:
   
   a) last interval for this question using:
   
`days_elapsed()`: This function is called from `updatetime()` to calculate interval
for *learning* questions. It calculates the number of days elapsed between the last two 
showings of a question.

   b) how long did it take to answer the question using :

`t_learning()`:

   - if it took less than 30 seconds to answer the question, 3 days are added to the last 
     interval. We can call this as *easy* question. For example, if the last interval was
     10 days, this question will be asked again after 13 days (10 + 3) from today,
   - if it took more than 30 seconds but less than 2 minutes, 2 days are added to the
     last interval. We can think of this as *medium* difficulty question, and finally
   - if it took more than 2 minutes to answer, 1 day is added to the last interval. We can think of
     this as *hard* question.

`updaterank()`: This function is called if the response to the question is miss(), i.e.,
the user failed to recall the correct answer. It does not update the interval, but only the rank,
so it keeps the question in the same session until it is answered correctly.

`mvPastdueToToday()`: Called by `quizme()` to move all the questions that are past due to today. This is done to keep due times simple. There is no need to fret about overdue questions. It also helps in more realistic interval calculations. For example, if you answered a question that was due a week back and let's say the total interval to add for next showing is 3 days, this question will remain overdue even though you answered it correctly today, which does not make sense.

### Controlling New Questions to Learn

Quizme allows you to add as many questions to the repository as you want because
the number of questions you are learning actively is controlled independently. The
following set of functions control how many *new* questions you want to learn. At the 
end of the quiz session (when all due questions are completed, 6 new questions are
automatically added, and if those are completed as well, any number of additional 
questions can still be added (if available in the repository)).

`newParked()`: Helper function used in `newMove()`. Returns total number of *new*
questions (only added to the repository but never practiced).

`newActive()`: Returns total number of *new* questions that are actively being learned.

`newDelta()` : Additional questions to be added to meet the target of 6 new questions.

`newMove()` : Main function that moves the requested number of questions from *new*
repository to actively *learning*. It simply randomly selects the requested number of
questions from *new* repository and changes their due date to today.

**NOTE**: After using the tool for a couple of months now, it appears that the logic to 
first add 6 questions automatically and then ask the user if they want to add additional
questions is more confusing than it is useful. It would be simpler just to directly
prompt the user to add the desired number of questions.

### Learning More

- [Quizme Introduction](../quizme-intro) for information on installation and usage without autoquiz
- [Quizme Design](../b92) on discussion on design philosophy
- [Autoquiz Workflow](../b94) for an example workflow with autoquiz
- [Quizme Package](https://github.com/pchhina/quizme) to install and try it!
- [Autoquiz](https://github.com/pchhina/autoquiz) to install and try autoquiz
