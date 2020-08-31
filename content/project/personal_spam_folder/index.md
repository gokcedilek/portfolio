---
title: Personal Spam Folder
summary: My first project!
tags:
  - personal
date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart
# links:
#   - icon: twitter
#     icon_pack: fab
#     name: Follow
#     url: https://twitter.com/georgecushen
# url_code: ""
# url_pdf: ""
# url_slides: ""
# url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides: example
---

Timeline: Jul - Aug 2020

**Technical Learnings:** The idea of this project, which was my first introduction to machine learning, is to build a machine learning model for [classifying](https://machinelearningmastery.com/types-of-classification-in-machine-learning/) emails in a user's inbox as emails that will be read or will not be read. The model uses the actual email data of the user, and the project functions as if creating a personal spam folder (it doesn't literally create the folder, but identifies (aims to identify as accurately as possible) emails that would go into that "personal spam folder" if it existed).

(Long) note before project description: This project was my first introduction to machine learning. Initially, I was thinking of taking an online machine learning course or something similar in order to get a "thorough" introduction to most of the mystery and most of the main subtopics to learn within machine learning. Even though I had come to believe that figuring out things on your own without necessarily taking a whole course (which would likely include things you wouldn't need to complete your project idea) is very valuable, I thought that machine learning is such a complicatedly interconnected field that without having a proper introduction to many of the subtopics at the same time, I couldn't proceed much. I think I was wrong!:)

When I first wanted to start a machine learning project, I asked about my "dilemma" about taking a course or "diving straight in" to a friend of mine, whom I also see as a mentor. She suggested the latter, and said that I could "hack my way through" and learn the required knowledge from tutorials, blog posts, etc. I liked this idea (her statement is cool, isn't it?:), and I appreciate she suggested going with the latter - I believe I learned a lot with this project, and I'm excited to not only describe my project here, but also make additions to it in the future!

**Description:** My project consists of a Python module, and 3 scheduled background tasks. I designed the first task to be the "data fetching" task, the second task to be the "model training" task, and the third task to be the "data visualization" task. In order to run the tasks periodically, I used a Python task queue library, [Celery](https://docs.celeryproject.org/en/stable/getting-started/introduction.html). Celery provides a task scheduler and worker nodes, but not the way of communication between them - for this, I used [Redis](https://redis.io/) as a [message broker](https://en.wikipedia.org/wiki/Message_broker) with Celery.
These tasks are described below:

**Task #1: Data Fetching** This task is divided into 2 sub-pathways in itself. The first case is for fetching data for the first time, and the second case is for the subsequent data fetches - both of which are described in step #4.

1. I used a Python IMAP4 client library, [imaplib](https://docs.python.org/2/library/imaplib.html), to get email data from a real inbox. Fetching my own data, instead of using an online dataset was a very fun experience, because it allowed me to decide on & go to through various preparation stages: retrieving emails via an IMAP library, extracting the information I need from the emails, and doing this data fetching regularly to keep getting new emails from the same inbox (described below). All along with these, and in my opinion most importantly, it gave me hands-on experience with doing custom data preprocessing for my own data, in order to clean/fix the unnecessary information in my data and put the data in a proper format that would be valuable for the classification model.

2. Along with its message broker functionality, I also used Redis as a database. In order to only fetch emails since the last email that was processed when the task executes, I used email [uid's](https://stackoverflow.com/a/37163120/11223254) (unique identification number), stored the uid we need to start fetching emails from as a Redis key, and used this information when fetching data with the IMAP4 client library.

3. I did data preprocessing before saving / working with the emails. This is because, text classification problems, in the field of [natural language processing](https://en.wikipedia.org/wiki/Natural_language_processing) may not need the raw data fetched/collected as it is, and in fact in most cases can better be solved after doing some processing on the raw data. The custom data cleaning I did includes using [regular expressions](https://en.wikipedia.org/wiki/Regular_expression) to identify tokens (meaningful units of a sentence) and [lemmatization](https://en.wikipedia.org/wiki/Lemmatisation) with [POS tagging](https://en.wikipedia.org/wiki/Part-of-speech_tagging).

4. After the data preprocessing, the task includes 2 possible paths:

- First data fetch (fetching all the emails currently available in the inbox, with their read/unread status based on when we are fetching): For the first data fetch, I implemented a methodology to split the data into the test set and the training set. This methodology works by separating 30% of the initial data we got from the inbox as the test set, in a way that half of the test set consists of randomly chosen read emails, and the other half consists of randomly chosen unread emails (this 50% - 50% balance isn't the case with the training set - I did it this way thinking that whatever data we can train the model with would be useful, but the balance of read/unread emails matters for the test set so as to evaluate the performance of the model properly). After the split, I stored the test and the training data to separate database tables (I chose to use a [PostgreSQL database](https://www.postgresql.org/), instead of Redis, because my task required having a structured data tables for implementing the later stages, and Redis didn't exactly meet these requirements, as explained in detail in [here](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-1-what-is-redis/1-1-1-redis-compared-to-other-databases-and-software/#:~:text=1%20Redis%20compared%20to%20other,No%20SQL%20or%20non%2Drelational%20.), so that the second task can access the emails for building the classification model and evaluating the model.

- Next data fetches (fetching the new emails that haven't been used by the application, with their read/unread status based on when we are fetching): Subsequent data fetches are very similar to the procedure described above (and are simpler). This time, instead of first splitting the data into the test & training sets, I started by shuffling the list of emails we got (this is also done as part of the test-train splitting procedure described above - this is in order to ensure there is more or less a random order of read/unread emails in the sets, to avoid situations where for example the first 200 emails in an inbox might all be read, and the next 100 might all be unread, and this could affect the learning of the model in undesired/irrelevant ways). Then, similar to above, I stored the shuffled emails to the training data table. Essentially, the difference between the two cases is about whether we are adding both to the test & to the training set, or only to the training set.

**Task #2: Model Training** This task consists of 1) determining if we need to (re)train the classification model (see #1 below), 2) encoding text data before training the model (see #2 below), 3) applying [hyperparameter tuning](https://en.wikipedia.org/wiki/Hyperparameter_optimization) with [grid search cross-validation](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) and fitting the data to the resulting model (see #3 below), 4) extracting statistics for training & testing processes (see #4 below), and 5) storing the statistics (see #5 below):

1. describe
2. describe
3. describe
4. describe
5. describe

**Task #3: Visualization** describe

**Non-Technical Learnings:** As I mentioned above, I would like to share the reasons why I think resolving the personal dilemma I had with this "dive straight in" way of learning about machine learning (pardon the pun^^) was a good way:

- For one thing, the process of going through this project has raised my self-confidence significantly, compared to when I first started. This is primarily due to the following several areas that I believe I got to improve myself in: effective self-teaching, problem-solving in creative ways, going from level 0 in an area to ... I guess, a better level :) even in one week's time - through digging into resources, combining past knowledge with new learnings, and trying things out. As it is the case with every new challenge in life, I ran into a lot of issues while developing the project. This gave me the opportunities of figuring out subtle details, solving my own issues, thinking practically & creatively in the context of my own problem / data / model, exploring various solutions / options, all along with truly understanding the details as to how I approached certain things individually, and how things "fall into place" in a bigger picture, much more comprehensively than I would have if I didn't run into these problems. After all, learning is usually very similar to putting puzzle pieces together!:)

- The second point I would like to make about what this project has helped me with is, the experience of going through the process of "creating a project flow/pipeline" (like data flow/pipeline:). I know it probably sounds a little bit unclear, but, what I mean by this is essentially the process of learning/building something from scratch, and making decisions along the way. This includes: determining the problem you are trying to solve, determining what are the pieces that make up your approach to the problem / solution (and this can of course change while progressing through the project) - breaking down what you would like to do into smaller parts (yes, this idea is a classic, but a very, very useful, and important, and required(!) classic:), understanding how to make those small parts work, understanding how to connect those working parts together. As a bonus, creating your own project flow also includes exploring a variety of possible approaches, solution ideas, tools, and technologies!:) Since this project includes a lot of data processing & creating a logical set of steps to follow to train and improve a model properly, I do think this "project flow" experience made/makes this project be among of the most enjoyable learning experiences I had, and taught me things that could be carried on to other projects / experiences in life as well:)

- Lastly, one other thing I cannot forget to mention is, the importance & the experience of "taking detailed notes". I am not sure if putting quotes are making everything sound more confusing than they should be:), but, how this fits into this project is as follows: I usually keep Google Drive folders and files for the learning experiences I am involved in, this includes courses at university, co-op work experiences I had, and past CS projects I worked on. I have seen how much this has benefited me over time, and this project wasn't an exception! What's more, with the personal CS projects I got to work on this summer, including this project, I took this "taking notes" approach I had made a habit of to a new level! Each day I worked on the projects, I kept a journal for recording my progress, my research, resources utilized, the problems I had, how I solved those problems, my learnings, my accomplishments, my ideas, my plans and to-do's for the coming days - not in a particular format, but however way I wanted for that day. I think this is the #1 thing that helped me keep going with this project as I ran into difficulties!:)
