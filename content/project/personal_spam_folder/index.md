---
title: Personal Spam Folder
summary: My first machine learning project!
tags:
  - personal
date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart
links:
  - icon: github
    icon_pack: fab
    name: ""
    url: https://github.com/gokcedilek/personal_spam_folder
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

**(Long) note before project description:** This project was my first introduction to machine learning. Initially, I was thinking of taking an online machine learning course or something similar in order to get a thorough introduction to the main subtopics within machine learning. Even though I have come to believe that figuring out things on your own without necessarily taking a whole course (which would likely include things you wouldn't need to complete your project idea) is very valuable, I thought that machine learning is such a complicatedly interconnected field that without having a proper introduction to many of the subtopics at the same time, I couldn't proceed much. I think I was wrong!:)

When I first wanted to start my first machine learning project, I asked about my "dilemma" about taking a course or "diving straight in" to a friend of mine, whom I also see as a mentor. She suggested the latter, and said that I could "hack my way through" and learn the required knowledge from tutorials, blog posts, etc... as with anything! I liked this idea (her statement is cool, isn't it?:), and I appreciate she suggested going with the latter - I believe I learned a lot with this project, and I'm excited to not only describe my project here, but also make additions to it in the future!

Additionally, it was my younger sister who called this project a "personal spam folder" when I described to her, so the name credit goes to her!;)

**Description:** The idea of this project is to build a machine learning model for [classifying](https://machinelearningmastery.com/types-of-classification-in-machine-learning/) emails in a user's inbox as emails that will be read or will not be read. The model uses the actual email data of the user, and the project functions as if creating a personal spam folder (it doesn't actually create the folder, but identifies (aims to identify as accurately as possible) emails that would go into that "personal spam folder" if it existed).

My project consists of a Python module, and 3 scheduled background tasks. I designed the first task to be the "data fetching" task, the second task to be the "model training" task, and the third task to be the "data visualization" task. In order to run the tasks periodically, I used a Python task queue library, [Celery](https://docs.celeryproject.org/en/stable/getting-started/introduction.html). Celery provides a task scheduler and worker nodes, but not the way of communication between them - for this, I used [Redis](https://redis.io/) as a [message broker](https://en.wikipedia.org/wiki/Message_broker) with Celery.
These tasks are described below:

**Task #1: Data Fetching** This task is divided into 2 sub-pathways in itself. The first case is for fetching data for the first time, and the second case is for the subsequent data fetches - both of which are described in step #4.

1. I used a Python IMAP4 client library, [imaplib](https://docs.python.org/2/library/imaplib.html), to get email data from a real inbox. Fetching my own data, instead of using an online dataset was a very fun experience, because it allowed me to decide on & go through various preparation stages: retrieving emails via an IMAP library, extracting the information I need from the emails, and doing this data fetching regularly to keep getting new emails from the same inbox (described below). All along with these, and in my opinion most importantly, it gave me hands-on experience with doing custom data preprocessing for my own data, in order to clean/fix the unnecessary information in my data and put the data in a proper format that would be valuable for the classification model (described below).

2. Along with its message broker functionality, I also used Redis as a database. In order to only fetch emails since the last email that was processed when the task executes, I used email [uid's](https://stackoverflow.com/a/37163120/11223254) (unique identification number), stored the uid we need to start fetching emails from as a Redis key, and used this information when fetching data with the IMAP4 client library.

3. I did data preprocessing before saving the emails. This is because, text classification problems in the field of [natural language processing](https://en.wikipedia.org/wiki/Natural_language_processing), may not need the raw data fetched/collected as it is, and in fact in most cases can better be solved after doing some processing on the raw data. The custom data cleaning I did includes using [regular expressions](https://en.wikipedia.org/wiki/Regular_expression) to identify tokens (meaningful units of a sentence) and [lemmatization](https://en.wikipedia.org/wiki/Lemmatisation) with [POS tagging](https://en.wikipedia.org/wiki/Part-of-speech_tagging).

4. After the data preprocessing, the task includes 2 possible paths:

- First data fetch (fetching all the emails currently available in the inbox, with their read/unread status based on when we are fetching): For the first data fetch, I implemented a methodology to split the data into the test set and the training set. This methodology works by separating 30% of the initial data we got from the inbox as the test set, in a way that half of the test set consists of randomly chosen read emails, and the other half consists of randomly chosen unread emails (this 50% - 50% balance isn't the case with the training set - I did it this way thinking that whatever data we can train the model with would be useful, but the balance of read/unread emails matters for the test set so as to evaluate the performance of the model properly). After the split, I stored the test and the training data to separate database tables (I chose to use a [PostgreSQL database](https://www.postgresql.org/), instead of Redis, because my task required having a structured data tables for implementing the later stages, and Redis didn't exactly meet these requirements, as explained in detail in [here](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-1-what-is-redis/1-1-1-redis-compared-to-other-databases-and-software/#:~:text=1%20Redis%20compared%20to%20other,No%20SQL%20or%20non%2Drelational%20.)), so that the second task can access the emails for building the classification model and evaluating the model.

- Next data fetches (fetching the new emails that haven't been used by the application, with their read/unread status based on when we are fetching): Subsequent data fetches are very similar to the procedure described above (and are simpler). This time, instead of splitting the data into test & training sets, the emails are only used for training. I started by shuffling the list of emails we got (which is also done as part of the test-train splitting procedure described above - this is in order to ensure there is more or less a random order of read/unread emails in the sets, to avoid situations where for example the first 200 emails in an inbox might all be read, and the next 100 might all be unread, and this could affect the learning of the model in irrelevant ways). Then, similar to above, I stored the shuffled emails to the training data table.

**Task #2: Model Training** This task reads the emails stored to the database by task #1, and outputs two csv files containing statistics that belong to model training and model evaluation, to be plotted by task #3. It consists of the following steps:

1. Similar to storing a key in Redis indicating the uid of the email we need to start fetching from in task #1, I used another Redis key indicating the number of emails used to train the model up to the current run of task #2. This way, I started by determining if the classification model needs to be re-trained (see #3 below) - if it is the case that there are new emails in the training data table.
2. Before inputting to the model, I used [hash encoding](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.HashingVectorizer.html) to encode my text data (convert to a numeric form). Hash encoding works by mapping input text to token counts using a given number of tokens ("buckets"). The difference between hash vectorizing and another popular method called count vectorizing is that count vectorizing requires building a dictionary of unique tokens, thus could possibly use many more buckets than hash encoding, whereas hash encoding could lead to [hash collisions](<https://en.wikipedia.org/wiki/Collision_(computer_science)>) if the bucket count is not large enough. Because I had two text features (sender of the email, and subject of the email), I had the option of 1) combining the text data and then encoding altogether, 2) encoding sender & subject separately, and then combining the resulting arrays. I went with the latter, as the sender & subject fields have quite different content, and might require differences in encoding (such as, a different number of bucket/token count).
3. I used [sklearn's Multinomial Naive Bayes classifier](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html) after doing my research about which classifier to use and exploring different options. The reason I chose Multinomial NB is because it is suitable for classifying discrete features like token counts, as I'm getting via hash encoding above. Before fitting the data, I applied [hyperparameter tuning](https://en.wikipedia.org/wiki/Hyperparameter_optimization) with [grid search cross-validation](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) to determine the best model to use. [(K-fold) Cross validation](https://scikit-learn.org/stable/modules/cross_validation.html) is a form of validation that does not require using a separate validation set - it works by using only the training set, repeatedly splitting the set into k "folds", and using k - 1 folds for training and 1 fold for testing. The first section of the linked page has great diagrams explaining cross validation! [Grid Search](https://en.wikipedia.org/wiki/Hyperparameter_optimization#Grid_search) is a way of determining the best hyperparameter values for a classifier, it exhaustively searches through a set of hyperparameter values and chooses the model to optimize the value of a defined metric/statistic by performing k-fold cross validation on each combination of hyperparameter values. I chose to use Grid Search because I had a small number of hyperparameters & combinations so the performance wouldn't suffer from exhaustive search, and grid search is guaranteed to find the optimal set of hyperparameters. I performed grid search as to maximize the classification [accuracy](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html), however, I also monitored the values of other metrics [balanced accuracy](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.balanced_accuracy_score.html), [precision](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html), [recall](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html), and [f1](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html) for the next step.
4. This step consists of extracting statistics for training & testing processes.

Training: Extracting the training statistics include finding the best set of hyperparameters from grid search (that maximize the accuracy, as mentioned above), and extracting the mean value of the k-folds of cross validation with the best set of hyperparameters, for each one of the metrics mentioned above.

Testing: Extracting the testing statistics include making predictions for the test set, and computing the metrics mentioned above using the predicted labels and the actual labels.

5. The last step of this task consists of storing the training & testing statistics to separate csv files. The columns of the csv files represent the metric, and the rows of the files represent the number of the training / testing round (iteration).

**Task #3: Visualization** This task reads the csv files, and creates one double bar graph for each of the monitored metrics. Each bar graph shows the metric values vs the iteration number, one bar for the training score for that iteration, and one bar for the testing score. One thing to realise about these plots is: The training data keeps changing (we re-train when there is new data available), whereas the test data remains the same (the initial set of emails we spared) across the iterations, thus, the training bars show us how the model evolves over time with new data, and the testing bars show us how the metrics change on the same test data as the model evolves over time.

**Note:** I have a list of ideas to make additions to this project, and I will keep updating this page when I do so! A few ideas I'm excited to explore are looking at other classifiers, statistics, encoding techniques, and packaging up my application to run in a Docker container (I have already attempted this, and failed due to some errors with Dockerizing my Conda environment - but will come back to it as time permits:), and doing better error handling if something goes wrong with the Postgres database or Redis.

**Non-Technical Learnings:** As I mentioned above, I would like to share the reasons why I think resolving the personal dilemma I had with this "dive straight in" way of learning about machine learning (pardon the pun^^) was a good way:

- First of all, I think that the process of going through this project has raised my self-confidence noticeably, compared to when I first started. Self-teaching, thinking creatively in the context of my own problem / data / model, combining past knowledge with new learnings, trying out various options and debugging (using [Google Colab](https://colab.research.google.com/) was a fun way of doing that!:), and going from level 0 in an area to ... better levels(^^) in a shorter time than I've expected are all great things that contributed to this!

- Another thing I truly enjoyed while working on this project is the experience of "creating a project flow/pipeline" (I think of this as being similar to a data pipeline:). It might sound a little bit unclear, but, what I mean by this is essentially the process of learning/making something from scratch, and making decisions along the way about how to structure this pipeline, without having strict guidelines to follow. I think, an important part of this "creating a project flow" is breaking down what you would like to do into smaller parts (yes, this idea is a classic, but a very, very useful, important, and required(!) classic:), understanding how to make those small parts work, and understanding how to connect those smaller parts together - into a pipeline!

- The last thing I cannot forget to mention is, the importance & the experience of taking detailed notes. I call this my "progress journ(al/ey)" - with the projects I got to work on this summer, each day I worked on a project, I kept a journal (that is, a Google Drive doc:) for recording my progress, my research, resources utilized, the problems I encountered, learnings, accomplishments, ideas, plans and to-do's - anything, really!:) - not in a particular format, but however way I wanted for that day. I have seen how much this has benefited me over time, that I feel confident about saying that these journals were probably the most helpful things in making me keep going as I ran into difficulties!
