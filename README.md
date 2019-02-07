<h2>Executive Summary</h2>
Love books? Want to chat about them on Reddit? Well, you're in luck -- there are numerous book related subreddits available. But which one is appropriate? Can we identify the unique characteristics of a typical post to each of the subreddits and predict which subreddict a particular post came from? That is the goal of this project.

In this project, I used the reddit api to gather ~1000 posts from each of 7 book-themed subreddits:
<ul>
  <li>books</li>
<li>Fantasy</li>
<li>sciencefiction</li>
<li>booksuggestions</li>
<li>whatsthatbook</li>
<li>bookclub</li>
<li>YAlit</li>
</ul>
The following features were gathered or calculated for each post:
<ul>
<li>ups: # of votes post has received so far</li>
<li>downs: # of downvotes post has received so far</li>
<li>num_comments: # of comments made about post</li>
<li>num_cross posts: # times post has been cross posted on another subreddit</li>
<li>title length: (calculated field) length of the title</li>
<li>text length: (calculated field) length of the text</li>
<li>external_url: (calculated field) 1 if post contains a url for a domain other than reddit.com, otherwise 0</li>
<li>hour_of_day: (calculated field) hour of day (24 hour clock) post was made</li>
<li>words in title: TFIDF score for cleaned words in title</li>
</ul>

The step to generate these features was included in a pipeline to make it easier to consistently apply the same transformations to the validation set and future test sets.

In order to evaluate the words in the post title, they had to first be cleaned up. This involved removing digits, punctuation, english stop words and special "giveaway" words (i.e. Fantasy or SciFi). The remaining words were then lemmatized (based on their assigned part of speech tag) and finally tokenized (broken into a list of individual words) and assigned a TFIDF score.

The cleaned vector plus the above fields were then fed to a number of classification models and scored based their accuracy and area under the curve in predicting the subreddit they came from.

Tests were run on unigrams (single words), bigrams and both unigrams and bigrams from the post title. Using bigrams resulted in decreased accuracy, but the results on unigrams plus bigrams were fairly equivalent. The final model used both. Including unigrams from the post text worsened accuracy and the text was not included in the final model.

A number of different classification models were used: Logistic Regression, Support Vector Classifer, Naive Bayes, Random Forst and CatBooost. Interestingly, a basic Logistic Regression worked as well or better than a more sophisticated CatBoost model. Each of those models, however, used very different features to classify the posts. Logistic Regression relied heavily on words in the post title, while CatBoost used some of the calculated fields, such as external_url, ups, text_len, num_comments and title_len before relying on title words

For a more detailed write up on this topic, please refer to my blog post: https://medium.com/dataexplorations/chatting-about-books-can-we-predict-which-book-themed-subreddit-a-particular-post-came-from-9022e79db99c
