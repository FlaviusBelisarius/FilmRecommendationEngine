# Film Recommendation Engine
A film recommendation engine based on TMDB dataset. 

### What the engine will do: 
User gives a movie name as input, then the engine will recommend 5 other movies to the user based on the similarity and the quality.  
### Presuppositions for this model: 
1, If people like a movie, they are also more likely to like other movies which are similar to the movie they like. 
2, The more people vote for a movie, the more reliable the movie rating (IMDB score) is. 
3, The popularity of movies is positively correlated with the quality of movies in most cases. 
### Explanation of the mathematical model: 
After calculating the “content similarity” between the movies by Euclid distance, 31 different movies that are closest in content to the movies entered by the user are selected. The Euclid distance between films is derived from the content of films, but the similarity of films is not limited to this. Similar release years and similar evaluations can be regarded as a reflection of similarity. But the “content similarity” will only use at here. 
The engine will recommend the 5 “best” films among the 31 movies. We use a new score to rank the 31 movies. This score is calculated according to this model:   
![Image text]<div align=center>(https://github.com/FlaviusBelisarius/FilmRecommendationEngine/blob/master/img-storage/ReadmePic1.png)  
The IMDB is the IMDB score (vote_average in dataset). The Φ represents gaussian function.   
![Image text]<div align=center>(https://github.com/FlaviusBelisarius/FilmRecommendationEngine/blob/master/img-storage/ReadmePic2.png)  
The two gaussians represent two different aspects: release time and the number of user votes. 
### The first gaussian: Release time gaussian function (the closer the better)
As we assumed, similar release years could also be regarded as a kind of similarity. For the first gaussian, the c1(expected value of published year) is the published year of the movie user input. The x will be the published year of the 31 different movies. I set σ1 = 20 currently(which is not a good idea, 15, 30 and 40 also works, it seems like 20 works best). This is because many movies in the dataset were released from 1980s to 2010s. I need to set σ1 has same scale with (x-c). 
The smaller the difference between the release year of the movie entered by the user and the movie currently selected to calculate the score, the greater the value obtained by the first Gaussian function.
### The Second gaussian: Number of user votes gaussian function (the larger the better)
The second gaussian is related to user votes. As we all know, the more votes a movie has, the more reliable of its IMDB score. A movie with a score of 9 stars by three people is likely to be worse than a movie with a score of 8 stars by 1000 people. So we should also consider the influence of number of user votes. The c2(expected value of number of user votes) is maximum number of votes among the 32 movies(including the user input movie). And the x will be the number of votes of the 31 different movies. 
Some popular movies may have thousands of comments, while some other movies may only have dozens. In other words, the (x-c) might be very large, and the difference between the maximum number of votes and the number of votes of other movies in 32 movies often has the same scale with the maximum number of votes. I know the number of user votes is very important, but I don't want the second Gaussian function to be a decisive factor in our model. So I set σ2 here equal to maximum number of votes, which makes (x-c) and σ2 are in the same scale in most cases. 
