# Project

Our research focuses on sentence retrieval for dialogues and specifically on the search for sentences that can be used for generating next turns in dialogues. 

To the best of our knowledge, there is no publicly available dataset for this task. Hence, we propose a novel annotated dataset which is described in our paper.
## Dialogues

A dialogue is considered valid for our validation and test sets if it is conducted between two users A and B with no less than 4 turns. User A is the user who turns the initial question in the dialogue and User B can be anyone else. 

The last turn in the dialogue must belong to User B and may link to sections in Wikipedia. Dialogues whose last turn contains any link to Wikipedia are called Wikipedia grounded dialogues, and others are simply called dialogues.

We excluded a large number of instances where:

- The language of the dialogue is not English

- The subreddit of the dialogue belongs to subreddits with offensive content

- The content of the dialogue includes words from  a public [list](https://github.com/LDNOOBW/List-of-Dirty-Naughty-Obscene-and-Otherwise-Bad-Words/blob/master/en) of dirty words. We also expanded this list by concatenating all the phrases found there

- The turns contain URLs, except the last turns in Wikipedia grounded dialogues which point to sections that can be found in our Wikipedia corpus

- There is a turn that is shorter than 5 tokens and longer than 70 tokens

## Candidates

We created 3 repositories based on 5,636,036 Wikipedia papers: documents, sentences and sections. We used [Wikipedia dumps](https://dumps.wikimedia.org/enwiki/20200101/enwiki-20200101-pages-articles-multistream.xml.bz2), while the majority of the parsing process was handled by an [external package](https://github.com/attardi/wikiextractor.git) from 2020-01-01 and we have made improvements manually.

For each dialogue, we propose a ranked list of 50 candidate sentences from Wikipedia. 
## Human Evaluation

To obtain human evaluations for the relevance of each candidate, we used the Amazon Mechanical Turk platform. We recruited “master” workers and used only the submissions from those who read the instructions for the task, as indicated by their success in our honeypots.

The dataset contains only dialogues with at least one relevant sentence, according to the workers.
## Dataset

Our dataset contains 846 annotated dialogues from Reddit – 400 dialogues and 446 Wikipedia grounded dialogues. We collected data from Feb-2011 until Dec-2013 and constructed complete dialogues after preprocessing the submission and comment files provided by the [Pushift.io](https://files.pushshift.io/reddit/) archives.

We split the data into two files: 

    - dialogues.json 

    - wikipedia_grounded_dialogues.json

Each dialogue in our dataset is represented by a dictionary object as detailed below:

- id - the dialogue ID which is composed of the subreddit ID and the submission ID
- subreddit - the web forum of a particular topic in Reddit
- title - the title of the dialogue as written by User A
- target - in dialogues is simply the last turn, and in Wikipedia grounded dialogues is the first turn that contains a link to a section from Wikipedia in a turn whose number is no less than 4
- context - list of turns that precede the target turn. Every turn in the context, including the target, is represented by the following dictionary:
    - author_id - the ID of the author of the turn
    - author_name - the author name
    - body - the text of the turn
    - score - the number of upvotes minus the number of downvotes given to the turn on Reddit

- candidates - a dictionary object whose keys are “pos” and “neg” and their values are lists of relevant and non-relevant candidates, respectively. Each candidate is represented by the following dictionary:

    - id - the sentence ID which is composed of the document ID, section ID, passage ID, sentence ID and the paper title to which the sentence belongs
    - title - the paper title to which the sentence belongs
    - body - the text of the sentence
    - score - the sentence score computed by the initial ranker
