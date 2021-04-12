---
title: Twitter User Recommender
summary: Twitter user recommendation system written in Prolog!
tags:
  - class
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

Timeline: Mar 2021

I did this project with one other friend for [UBC's CPSC 312 course](https://courses.students.ubc.ca/cs/courseschedule?pname=subjarea&tname=subj-course&dept=CPSC&course=312), and the code for our project can be found [here](https://github.com/shlyyzy/twitterUserRecommender).

This program allows users to input a search query about tweet content they are interested in, and returns a list of user accounts with popular & recent tweets matching the user’s preference.

After examining the grammar & constraints of the Twitter search API, we implemented a dictionary in Prolog for ourselves that defines how a valid request (user input) to the API should be structured. See [here](https://github.com/shlyyzy/twitterUserRecommender/blob/main/dictionary.pl) for the dictionary. **We defined the components of a valid request as:**

- `operator`: an `operator` is simply pre-defined to be `and` / `or`.
- `filter_option`: a `filter_option` is simply pre-defined to be a set of special filter keywords of the API, such as `media`, `retweets`, `verified`, that are used in the form of `filter:media`.
- `keyword`: a keyword is defined to be anything that is not an `operator`, not a `filter_option`, and the word `filter`. The word `filter` is defined as a special keyword that separates the `query` portion from the `options` portion of the user's input, described below.

```
keyword([X|L], L, E, C, C) :-
  \+ operator([X|L], L, E, C, C),
  \+ filter_option([X|L], L, E, C, C),
  X \== 'filter',
  check_keyword(X, R).
```

- `query`: a `query` represents the first part of the user input / request, i.e., everything before the word `filter`. a `query` is:
  - either a list of `keyword`s (such as "grumpy cat")
  ```
  op_phrase(L0,L2,E,C0,C2) :- keyword(L0,L1,E,C0,C1), op_phrase(L1, L2, E, C1, C2).
  ```
  - or a list of `keyword`s combined with `operator`s (such as "grumpy cat or cat")
  ```
  op_phrase(L0,L3,E,C0,C3) :- keyword(L0,L1,E,C0,C1), operator(L1,L2,E,C1,C2), op_phrase(L2, L3, E, C2, C3).
  ```
  - or a single `keyword` (such as "cat")
  ```
  op_phrase([L],L1,_,C0,C1) :- keyword([L],L1,_,C0,C1).
  ```

The `query` then might also end with the special word `filter`, in order to signal that after this `query` portion, there are a list of filters, but if it does include `filter`, it needs to be followed by a non-empty array of words (if we didn't want to input `filter_option`s, we wouldn't put `filter`):

```
op_phrase([filter | L], L, _, C0, C1) :- length(L, Length), Length > 0.
```

- `options`: `options` represents the second part of the user input, i.e., everything after the word `filter`. `options` is:

  - either a list of `filter_option`s combined with `operator`s (such as "media and links or retweets")

  ```
  options(L0,L3,E,C0,C3) :-
  filter_option(L0,L1,E,C0,C1),
  operator(L1,L2,E,C1,C2),
  options(L2, L3, E, C2, C3).
  ```

  - or a single `filter_option` (such as "media")

  ```
  options([L],L1,_,C0,C1) :- filter_option([L],L1,_,C0,C1).
  ```

- given these parts, the whole `phrase` itself is simply:
  - either a `query` (`op_phrase` in the code) followed by `options` (such as "grumpy cat or cat filter media and links")
  ```
  phrase(L0, L2, E, C0, C2) :- op_phrase(L0, L1, E, C0, C1), options(L1, L2, E, C1, C2).
  ```
  - or just a `query` with no filter keywords (such as "grumpy cat or cat")
  ```
  phrase(L0, L1, E, C0, C1) :- op_phrase(L0, L1, E, C0, C1).
  ```
  - or a single `query` keyword (such as "cat")
  ```
  phrase([L0], L1, E, C0, C1) :- op_phrase([L0], L1, E, C0, C1).
  ```

The `phrase` represents how we validate a user input before making a request to the API! Once we get the user input, we process it to:

- check if the input is a `phrase`

```
get_constraints_from_question(Q,A,C) :-
    phrase(Q,End,A,C,[]), // look at everything from Q until End
    member(End,[[]]). // where End is defined to be the empty list
```

- split the input into the `query` and `options` parts

```
deconstruct([H|T], [H | Q], X) :- deconstruct(T, Q, X). // if the current word is not "filter" put it to the 2nd argument of `deconstruct` (as one of the return values), recurse on the rest of the list
deconstruct([filter | T], [], T). // if the current word is "filter", put everything after that to the 3rd argument of `deconstruct` (as one of the return values)

get_parts(Ln, Q, F) :-
  member(filter, Ln), // if "filter" exists in user input, put `query` into `Q`, and `options` into `F`
  deconstruct(Ln, Q, F).
get_parts(Ln, Ln, []) :-
  \+ member(filter, Ln). // otherwise, we only have a `query` with no filter options, put everything into `Q`
```

See [here](https://github.com/shlyyzy/twitterUserRecommender/blob/main/userRecommenderSystem.pl) for details.

Last but not least, after the validate the request, we [send it over to the API](https://github.com/shlyyzy/twitterUserRecommender/blob/main/api.pl). The user accounts are found in the JSON response as follows:

```
{
   "statuses": [
      {...
      "user": {
        "id": 1298997255609790464,
        "id_str": "1298997255609790464",
        "name": "Avery ❦",
        "screen_name": "JE0NGHOUL",
      }
      ...}
    ]
}
```

Thus, we parse this as follows:

```
get_screen_names(JSON.statuses, RecommendedUsers, NumResults). // look at the statuses array, return the user account names as well as the number of distinct user accounts we found
```

```
get_screen_names([], [], 0).
get_screen_names([H|T], R, M) :- get_screen_names(T, R, N), member(H.user.screen_name, R), M is N. // if the current username has already been added to the result R, skip over it, recurse on the rest of the list
get_screen_names([H|T], [H.user.screen_name | R], M) :- get_screen_names(T, R, N), \+ member(H.user.screen_name, R), M is N+1. // if the current username has not been added to the result R, prepend it to R, increment the count, and recurse on the rest of the list
```

NOTES: We used [this API](https://developer.twitter.com/en/docs/twitter-api/v1/tweets/search/api-reference/get-search-tweets), and we found that Twitter & Postman have a very cool [integration](https://documenter.getpostman.com/view/9956214/T1LMiT5U) that allowed us to import the Twitter search API to our Postman team workspace, and we were able to see & use the API endpoints very easily!
