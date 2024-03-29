---
title: "Quizme Design"
date: 2018-12-23T15:53:00-06:00
draft: FALSE
tags: ["R"]
output:
  html_document:
    keep_md: yes
---

The idea of [quizme](https://github.com/pchhina/quizme) started with a need to learn R more effectively as I could not spend more than few hours a day outside of my regular life. My first approach to the problem, while I thought was neat, needed a lot of manual intervention. In this design, I had two text files - one containing questions and another containing questions and answers. A simple function would randomly select 10 questions. You would open this test file in a text editor, write code to test your understanding, then go back to the file containing answers to see if your answers are correct. This approach had some serious drawbacks:

1. There is no way to evaluate if you are improving; the function just randomly picks 10 questions.
2. You have to maintain two separate files containing same information.
3. You have to switch between files to check the answers.
4. It is not efficient to share just the function with other users without proper documentation. 

### Design Goals and Solutions
Quizme was my solution to address these drawbacks. I drew some ideas from books I read earlier on the subject of memory and learning ([Make It Stick](https://www.amazon.com/Make-Stick-Science-Successful-Learning/dp/0674729013/ref=sr_1_1?ie=UTF8&qid=1545611894&sr=8-1&keywords=make+it+stick) and [A Mind for Numbers: How to Excel at Math and Science](https://www.amazon.com/Mind-Numbers-Science-Flunked-Algebra/dp/039916524X/ref=pd_bxgy_14_img_2?_encoding=UTF8&pd_rd_i=039916524X&pd_rd_r=5ff2d190-0714-11e9-a244-c1f675b879f4&pd_rd_w=2akPJ&pd_rd_wg=6dhLG&pf_rd_p=6725dbd6-9917-451d-beba-16af7874e407&pf_rd_r=8VVX0ZC67249QR0E8286&psc=1&refRID=8VVX0ZC67249QR0E8286)) and a software solution I had used before, [Anki](https://apps.ankiweb.net/). I set out with the following design goals and the solutions driven by these goals:

1. **Simple** I do not have any formal training in UI/UX design but in my current job, I get frequent opportunities to discuss what a good design should be. While for some applications (graphic design tools for example) it is unavoidable to have many buttons and dialogs, many others can do without (linux utilities for example) fancy UIs

- *Solution*: Keep the interface in command-line only
![](simple_interface.png)<!-- -->


2. **Intelligent** There has to be some way of recording the performance so that tool can schedule the next quiz intelligently to minimize the time needed to learn a certain concept

- *Solution*: This is where the tool takes care of which questions to quiz you on based on your past performance. Based on whether you were able to solve the problem and the time it took you to solve, the tool will schedule when to ask you the same question next time. Two concepts are employed to this end: a) **mixing up your practice**, questions from available pool of *new* questions are randomly chosen which are brought into *learning* category. So if you have an assortment of questions from different subjects, these may enter in a single quiz thereby mixing things up. This has the potential to create stronger connections withing the brain. b) **spaced repetetion**, where the *learning* questions are ranked based on your performance: a problem that took longer to recall will be asked more often than the problem that took shorter to recall. This makes efficient use of your learning time.

3. **Ease of use** If you have to do more work to learn or remember how to use the tool, it becomes a distraction that can potentially take away our limited amount of cognitive energy

- *Solution*: Show the commands at the command prompt. In the example below, if you want the tool to show you the answer, you simply press 't' followed by return key
![](ease_of_use.png)


4. **Fast** As the data grows, it should not slow down the response which can become frustrating enough to give up on the tool

- *Solution*: The underlying data structures for ranking, questions and metadata uses [data.table](https://github.com/Rdatatable/data.table/wiki) under the hood, which is the fastest library to wrangle in-memory data 

5. **Expandable** If i need to improve/change the algorithm later, the metadata has to be kept separate from quiz data

- *Solution*: a separate data table (called 'testlog') tracks all the performance statisctics of the tester. This data can be used later to develop/improve the algorithm

6. **Shareable**  Not only this would allow me to share the tool with others, it makes the documentation and maintainence easier and allows for collaboration to make additional improvements

- *Solution*: create a package. Another thing I did in this regard is to create a command line wrapper around the R code. This extends the usability to non-R users by allowing them to launch the tool from command line and learn any language or subject without the need to learn/know R programming

### Features

1. Easily add question-answer pairs
2. Active learning respository and total number of questions are separated so you can freely add as much learning material
3. Topic tags
4. Image answers
5. Cloud synchronization
6. Week ahead status
7. Control of number of new questions to learn

### Learning More

- [Quizme Introduction](../quizme-intro) for information on installation and usage without autoquiz
- [Quizme Development](../quizme-development) on discussion of code and related functions
- [Autoquiz Workflow](../quizme-workflow-with-autoquiz) for an example workflow using autoquiz
- [Quizme Package](https://github.com/pchhina/quizme) to install and try it!
- [Autoquiz](https://github.com/pchhina/autoquiz) to install and try autoquiz
