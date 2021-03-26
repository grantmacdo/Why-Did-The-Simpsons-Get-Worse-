# Why Did The Simpsons Get Worse
An attempt to use data to identify answers to why The Simpsons got worse?

Firstly, I was interested in confirming a long held belief that The Simpsons has gotten worse over the years. Secondly, I was interested in using data to idenify any possibel explanations for why this happened. Once I could source some data and understand it, I could identify some more specific questions to answer.  

# Source of Data
Thankfully, I was able to use data provided by Adam Reevesman (https://github.com/areevesman/the-simpsons) as a starting point. The data set available provided a detailed script for every episode (up to Season 26 Episode 16) stating exactly what a character said and the location each line was spoken at. 
Furthermore, IMDb ratings were provided for every episode, which although not a perfect way to measure quality, gave me a numeric value to use as a quality indicator.

# Libraries used
pandas, numpy and matplotlib.pyplot as plt

# Understanding the Data
The data included a detailed script for each episode, including whuich character spoke each line and where it was spoken. Using these scripts, I would be able to identify which characters and locations feature most haveily, and investigate how they impacted on the quality of the show. Quick intial analysis uncovered the following:
    The most common line in the scripts is "Homer Simpson: (ANNOYED GRUNT)" which occurs 219 times, d'oh!
    The character with the most lines is Homer Simpson
    The location where most lines are spoken is the Simpson's home
    The most common line spoken (by more than one character) is 'no' (426 times)
    Over 25% of lines are spoken in one of 5 locations (Simpson Home 22%, Springfield Elementary School 4.5%, Moe's Tavern 3% Springfield Nuclear Power Plant 2%, Kwik-E-Mart 1%)

# Cleaning and Checking Data
When loading the initial data, Pythoin highlighted some potential issues. Upon inspection of the line numbers listed within the csv file, it appears that some lines of dialogue have run into one another. There were a total of 27 lines within the data which had more than 13 columns within the data. I deleted these columns, potentially losing 54 lines of dialogue. Since this accounts for around 0.036% of the data, I was happy to lose it. Furthermore, the dialogue was not concentrated around any one particular episode or season and so would have a minimal impact on any findings. 

One of the fields in the data was 'speaking_line' which should have been a boolean variable to indicate whether each line was spoken by a character, or was merely setting a scene. I found that within this column 5 different values were used. A few rows had text values of 'TRUE' or 'FALSE' instead of teh Boolean values. These text values were replaced with the appropriate Boolean values. Furthermore, one row, had a line of dialogue instead of True/False. Upon inspection of this row I found that this was line being spoken by Abrham Lincoln. I was able to correct several columns for this row and still include it in analysis. 

The initial data had a word count column which was supposed to count the number of words spoken in each line of dialogue. To confirm this column was correct I calculated my own word count column. The only discrepancie found came from lines where no words were spoken. In my word count check, I treated these as NaN values, whereas the intial data gave a word count of zero. This will have no impact on future analysis, and so our check confirmed that the word count column was accurate. 

# Data Analysis
## Word Count by Character for Each Episode
I wanted to create a dataframe where I had each character along my columns, and each episode ID as my rows. I then calculated the total words spoken by every character for each episode using a picot table. This then let me perform further analysis. I performed some spot checks to make sure tha data seemed reasonable. There were no episodes which had a word count which seemed completely at odds with the mean. Furthermore, by looking at the total words spoken in 10 episodes by Homer and Marge (two of the main characters in the show) these seemes reasonable. 

## Characters Speaking the Most Words
I counted up the total words spoken by each character to find 20 characters who spoke the most lines. 

## IMDb Ratings
I added a column to my pivot table containing the IMDb rating for each episode. This let me find the correlation between episode_id (this is basically an episode count starting at one) and IMDb rating for that episode. I calculated a Pearson correlation coefficient of -0.75 showing a clear negative correlation between episode numnber and IMDb rating. This is clear evidence that The Simpsons deteriorated over time. 

## Correlation Between How Many Words a Character Speaks and IMDb Rating
I wanted to determine whether the appearance individidual characters was more or less likely to result in a higher IMDb rating. I looked at the top 20 characters (based on word count) and found whether these values were correlated to the IMDb rating. We find that number of words spoken by Smithers and Mr Burns have the highest positive correlation (0.21 and 0.14, respectively), whereas Lenny and Carl have the largest negative correlation (although modest at around -0.1). However, as shown previously, later episodes have lower ratings, so I wanted to investigate whether there was a correlation between the episode number and how much each of these charctars spoke. 

I found some negative correlation between the number of words Smithers speaks and the episode_id. As the series progresses, Smithers appears to have fewer words. Homer appears to be in a similar position (although this is less prominent). Words spoken by Marge are also negatively correlated, although the correlation is lower again, Lenny appears to have more words in later episodes compared to early episodes. So, it appears, at least to some extent, that by appearing more in the later, less well received episodes, it may look as though Lenny and Carl are to blame, however, it's not fair to lay the blame at Lenny and Carl's door!

## Total Word Count per Episode
The results above made me wonder whether there was a trend with word count as the show progressed. I found a correlation of -0.33, showing that as the show has went on, the total number of words spoken per episode certainly looks to have decreased. Unsurpsringly, I also found a correlation between IMDb score and word count. This is hardly surprising. We have already shown that latter episodes have lower IMDb ratings. We also just found that latter episodes have fewer words. Hence, we woudl expect there to be a negative correlation between total words in an episode and  IMDb rating. However, we obviously can't conclude that the IMDb ratings are lower because fewer words are spoken. 

## Correlation Between Proportion of Words a Character Speaks and IMDb Rating
I performed the same analysis described in the section above, but instead of using word count, I used proportion of the total words spoken by a character in an episode. However, this change had little no impact on the results. The results from above stand. 

# Correlation Between Location of Dialogue and IMDb Rating
To consder location an additional pivot table was created showing how many words were spoken in each location across all episodes. As with the character analysis, we will first consider raw word count, then consider proportion of words. 

Reasonableness checks confirmed that our pivot table seemed reasonable. 

I found that the Power Plant has the highest positive correlation with IMdb rating whereas 'Springfield Street' and 'Springfield' both had the hughest negative correlation. However, when considering how often these epiodes appeared in later episodes. I found that the power plant appears less frequently in later episodes, whereas 'Springfield Street' and 'Springfield' both appear more often. Again, we cannot say that these episodes are directly responsible for higher/lower IMDb scores.

# Proportion of Dialogue Spoken in Top 5, 10 and 20 Locations
Our entire data set contained 1,300,000 words being spoken. The top 5 locations account for around 34% of all dialogue spoken. The top 20 locations account for around 38% of all dialogue spoken. The top 20 locations account for around 44% of all dialogue spoken.This led me to wonder whether there was a link between the IMDb score, and the more unusual the locations.

As a big fan of the show in the early years, one of the issues, in my opinion was the growing reliance on new locations (for example, visiting Japan and the UK). I wondered if there was connection between how much time was spent in the more standard locations and quality of episode.We will now create a new column in our pivot table, to calculate the total words spoken in the top 5, 10 and 20 locations per episode.

I found that when considering the top 5, 10 and 20 locations, proportionally speaking, these locations were used less post episode 200 than before this point.

First, considering just the top 5 locations, I found that there is a small negative correlation between the proportion of dialogue in these locations and episode number. However, I did find a slightly larger postive correlation between the proportion of dialogue in these 5 locations and the IMDb score, suggesting that episodes more heavily based in these locations were slighly more likely to have a higher IMDb rating. I found similar results when considering the Top 10 and 20 locations, although the results were less prominent.


## Another Look at the Simpson's Home Location

When inspecting the Location column of my data, I noticed that the location labels could overlap. This was particularly the case with the Simpsons home. I found that around 20 locations could probably be classed as being within (or very close) to the Simpsons home. I then calculated the total number of words spoken within thes elocations in and around the house. I found that there was a negative correlation between the proportion of words spoken in these lcoations and episode number, and a positive correlation with IMDb rating. Again, it is difficult to say the lower IMDb ratings were caused by deviation away from these locations, or these locations merely happened to feature more heavily earlier in the series.

# Conclusions

We initially found a clear link between episode number and IMDb scores, with the latter episodes generally having lower ratings. When considerign what other factors affected the IMDb rating we almost always found that these factors were also related to episode number. Hence, it is hard to identify which factors were key in influecing IMDb scores. Howeever, we have been able to identify many trends that have occured over the run of the show. For example, less words are spoken as the show goes on, and certain charcters and locations are certainly being used more (or less) as the show develops. 
