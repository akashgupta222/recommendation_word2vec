
**Challenge Recommendation** 


 The goal is to recommend challenges to users seeing their previously solved challenges. More details about the competition and dataset can be found at https://www.hackerrank.com/contests/machine-learning-codesprint/challenges/hackerrank-challenge-recommendation
 
**Preprocessing:**
1. Form a time­sorted sequence of challenges each user has solved.  
2. Keep a set of the solved challenges for each user.  
3. Keep a track of the unsolved challenges and the number of failed attempts for each 
user. 
4. Store the challenges sequence and solved challenges and unsolved challenges in a 
dictionary with the key as the user id.  
5. To train word2vec model, make a list of sentences where each sentence is the list of 
challenges a user has attempted.  
 
**Model Chosen:** 

I chose word2vec model to recommend items to a user using the principles of collaborative 
filtering.  
Word2vec is used in the field of NLP where is converts the words to vector representations such 
that the syntactically and semantically similar words are placed closer to each other.  
I used the skip­gram algorithm with negative sampling in which we try to predict the surrounding 
words seeing a word.  
 
Here to model our problem to the word2vec model, a sentence is the sequence of challenges 
which a user has attempted.  
The advantages of word2vec for collaborative filtering are:  
1. It captures the challenges which appear together. This is the theme of collaborative 
filtering. The similar challenges proposed by this model has a positive cosine similarity 
with the challenges already done by the user.  
2. It captures the challenges which appear in the same context.For instance, it tries to 
capture the challenges which are generally done by beginners and to a beginner it will 
suggest those challenges.  
 
I prioritized the challenges solved by the users over the unsolved ones. 
The algorthim which i used is, for each user : 
1. I got the domains of the recent challenges (challenges done in the past 1 day) for each 
user. 
2. I got the challenges with positive cosine similarity with the challenges the user has 
already solved. The challenges which had similarity greater than 0.3 and their domains 
were a part of the recent domains were recommended.  
3. If we still dont get enough (10) challenges, see outside the recent domains to 
recommend challenges with cosine similarity greater than 0.3.  
4. If we still dont have enough challenges, use all challenges as opposed to only solved 
challenges to find the similar challenges and recommend the ones in recent domains 
and having similarity greater than 0.3.  5. If we still dont have enough challenges, look outside the recent domains and 
recommend challenges having similarity greater than 0.3.  
 
Also, to fine tune the algortihm i used some collective intelligence techniques like not 
recommending the challenges which have a very low successfull submission rate or very low 
total submissions. 
 
Also, as a result of some exploratory analysis by seeing the journey of a user between different 
challenges, i found out that it if the last submission by a hacker is unsucessfull, the very next 
submission will be of the same challenge. Plots for some users are attached in the end for 
reference. So, if the last challenge is unsolved, i always add that challenge to the 
recommendations list.   
 
The hyper parameters for the word2vec model (window size, negative sampling, number of 
iterations) were tuned by experimenting with different combinations. For this data, i found out 
the parameters window size=20, negative sampling=15 , number of iterations= 25 to be the 
best.  
 
The hyper parameters for the recommender system, ie minimum cosine similarity(=0.3), number 
of days to consider recent challenges(=1), minimum number of submissions to a challenge(=10) 
and successfull submission rate(=0.2) were found out similarly by experimentation.  
 
**Dataset quality:**  
The domains column had many nan values even for the contest c8ff662c97d345d2. I did some 
tests and imputed this value to be similar to Algorithms domain for the challenges in this 
particular contest.  
Also, the difficulty column was not reliable. Some of the challenges having very low successfull 
submission rate had a difficulty very easy and vice versa. Also i did a lookup of those challenges 
on the hackerrank website and did not find consistency with the labels provided by hackerrank 
(ie easy, medium and hard)  
 
 
 
 
 
 
 
 
 
 
 
Copyright © 2016 Akash Gupta Fig 1.1 and 1.2 ­ Plots for the submissions for 2 users. X­axis has the challenge id and y axis 
has submission time in ascending order (increasing as we go up).  
 
Copyright © 2016 Akash Gupta 
