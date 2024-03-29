---
title: "Autoquiz: A Wrapper to Run Quizme from Terminal"
date: 2018-12-28T06:09:53-06:00
draft: FALSE
tags: ["R"]
---


[Quizme](https://github.com/pchhina/quizme) is an R package that allows you to create a personalized quiz repository and enables you to learn *any* material efficiently. [Autoquiz](https://github.com/pchhina/autoquiz) is a simple wrapper script that allows you to run *quizme* directly from the terminal window. Here I discuss the advantages of running the tool from terminal as well as describe the implementation.

### Benefits of Running Quizme from Command Line
While quizme is quite powerful, integrating it to your daily workflow can be a bit of a challenge. This is mainly because although there are just a few simple functions you need to remember, these are repeated as many times as the number of questions you are practicing. Autoquiz addresses this challenge with the following advantages:

**Run from terminal:** I think this is the biggest advantage of autoquiz. This essentially separates R and Quizme from each other. You can run quizme in a separate terminal window, thus enabling you to learn any material without having any knowledge of R!

**Single key responses:** All the user functions available in quizme can be called now with a single key, which enables the tool to get out of your way. For example, to call `hit()` function as a response that you know the answer, you simply type `h` followed by return.

**Command menu visible at prompt:** You do not have to remember even the single letter function calls. These are available right at the command prompt.

**Fully automated:** The quizzing process itself is automated; all you need to do is launch the tool using `autoquiz` at the terminal. Your most typical responses will be a) yes reponse `(h)it`, no respnse `(m)iss` or adding a new question using `(a)dd`. It will keep on posing new questions automatically until all due questions are completed.

**Cloud synchronization:** This allows you to work on different machines without loosing the progress of your work. I have set it to sync with google cloud, but this can be changed to any other service if needed (Amazon, Dropbox etc) since it uses [rclone](https://rclone.org/) for this functionality.

### Implementation
Key enabler for autoquiz was to launch the R script in interactive mode since we want to allow user to continuously interact with the script. This is done by reading the `R_PROFILE_USER` environment variable from a bash script as described in this [post](https://thesquareplanet.com/blog/interactive-r-scripts/) by Jon Gjengset.

Following line reads the bash script to run R in interactive mode:


```{.bash .white}
#!/usr/local/bin/Rint
```

This is followed by loading of quizme library. Then the `.quizme` repository containing quiz files is synced with google cloud using:


```{.r .white}
system('rclone sync gcloud:quizme ~/.quizme')
```
Then the local `.quizme` repository is read into R environment using `quizme()` function. After this, `readline` and `switch` are used to simplyfy the function calls:


```{.r .white}
input <- 'z' # dummy input to force into while loop
while (input != 'b') {
    if (input == 'h' | input == 'm' | input == 'z') {
    cat('\n')
    ask()
    }
    cat('\n')
    input <- readline(prompt = '(h)it (m)iss (t)ell (b)ye (a)dd (c)hange (s)tatus (r)epeat (w)eek: ')
    cat('\n')
    input <- as.character(input)
    switch(input,
           h = hit(),
           m = miss(),
           t = tell(),
           a = addq(),
           c = changeq(),
           s = show_status(),
           r = ask(),
           w = week_ahead(),
           dl = delete_last())
}
```
If there are no more due questions in the current session, `(b)ye` is called automatically. `(b)ye` can also be called at any time during the quiz. Call to bye will update the local `.quizme` repository.
Finally `rclone` is called again to update the google cloud quizme directory with the local .quizme repository:


```{.r .white}
system('rclone sync ~/.quizme gcloud:quizme')
```

### Learning More about Quizme and Autoquiz

- [Autoquiz](https://github.com/pchhina/autoquiz) to install and try autoquiz
- [Quizme Introduction](../quizme-intro) for information on installation and usage without autoquiz
- [Quizme Development](../quizme-development) for discussion of code and related functions
- [Quizme Design](../quizme-design) on discussion on design philosophy
- [Autoquiz Workflow](../quizme-workflow-with-autoquiz) for an example workflow using autoquiz
- [Quizme Package](https://github.com/pchhina/quizme) to install and try quizme
