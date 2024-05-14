# Diving into the Digital Public Space: An Partisanship Analysis of Twitter Search Results and For You Feeds
#### Ben Saldich, Hamza Belgroun & Harshad Gaikwad  – Spring 2024


## Table of Contents

[1. Introduction](#intro)

[2. Literature Review](#litrev)

[3. Methodology](#methods)

[4. Result and Analysis ](#results)

[5. Discussion](#discussions)

[6. Conclusion](#conclusions)

[7. References](#bibliography)



<a name="intro"></a>
## Introduction
<p>
Intro here
</p>


<a name="litrev"></a>
## Literature Review
<p>
Lit Review here
</p>

<a name="methods"></a>
## Methodology
<p>
An initial seedlist of conservative and liberal influencers were identified using Audiense, a social intelligence platform that uses followership data to cluster Twitter accounts. We used two seedlists of conservative influencers and liberal influencers. We then extracted the 20K most engaged tweets from any of these users since 1/1/2024, and pivoted the extracted data on how many posts each authored and how many engagements their posts garnered. These two scores were normalized on a 0:1 scale within +/-3σ (any score above 3σ was matched to the highest index of 1). The follower lists of our Twitter accounts were comprised of the 100 highest scoring influencers from each list, with a highest total possible score of 2. Manual qualitative evaluation was performed to assess the political polarity of these influencers. As predicted, these individuals published highly political, highly partisan content. One influencer appeared in both lists, a comedian whose political stance is liberal but whose “anti woke” comedy garners support from conservatives. 

We created four Twitter accounts, a conservative account, a liberal account, an account following all 199 partisan influencers (known as the “everyone” account), and an account following no one (the “no one” account). Before creating each account, a VPN was turned on and set to Phoenix, AZ, USA. Each Twitter account was created using a new protonmail email account and using a developer browser. In order to emulate a truly neutral profile, we selected all interest areas. Twitter requires you to follow at least 1 account, so for the ‘no one’ account, the local police dispatch Twitter account in Phoenix AZ was followed and unfollowed immediately after account creation. 

Data collection was captured using Zeeschuimer, a tool developed by the Digital Methods Initiative at the University of Amsterdam. Zeeschuimer allows an individual to capture data natively as they use social media. Once accounts were created, data was collected on the ‘For You’ feeds of each profile, a feed that combines content posted by those whom an account is following with suggested content: 

- con_fy (N = 10,888)
- lib_fy (N = 11,444)
- noone_fy (N = 2,692)
- everyone_fy (N = 11,556)

<p>
We also collected a number of datasets using search terms. These search term datasets were collected in tandems: two computers were set up with the same VPN set to the same location (Phoenix, AZ) and searches were executed at the same exact time using the same wifi network. In this way, the influence of timing and location based factors on search results were minimized. Given equipment limitations, only two search result datasets could be recorded at a given time. The following pairs of search terms were used: 
  </p>

  
<p>
<p align="center">
  <img width="588" alt="Screenshot 2024-05-14 at 11 15 27" src="https://github.com/bensaldich/ddps_final/assets/71343656/463053f3-1990-46a6-8475-49c13641f740">

</p>

The tweet body column in our dataset was cleaned, performing lemmatization and removing stop words, URLs, and punctuation. For the search results dataframe pairs, a binary column was created, with a 1 corresponding to tweets that were included in the paired dataset, and a 0 corresponding to tweets that were unique to that search result. The degree of equivalency in our dataset pairs varied between (59.7% and 77.9%), demonstrating that a significant majority of the tweets appeared in both search results.

Because our goal was to measure the influence of followership on search results, we first needed a way to quantify the partisanship of the For You datasets. This cleaned data was scored using a scaled F-score from the scattertext library to measure the significance of terms in between two categories (for our purposes liberal and conservative). This scaled F-Score balances the competing interests of high recall and precision. By combining our Conservative and Liberal 'For You' Results into one dataframe, with a new column pertaining to what each tweet came from, we were able to create a partisanship corpus, with one and two-word terms and a ‘partisanship’ score measuring the degree to which that term appeared in the conservative tweets relative to the liberal tweets. This partisanship score is measured on a 0-1 scale, with terms more prevalent among the conservative tweets garnering a score of 1. This partisanship corpus provides a measure of the significance of certain terms used more prevalently by one political affiliation in relation to the other.

Once created, we applied those partisanship scores to the words contained in our various data. For each tweet of each search result dataframe, we created a dictionary containing all words in that tweet that appear in the ‘For You’ term list as keys and their respective partisanship scores as their values. Because scattertext identifies both one and two-word terms, two-word terms made up of one word terms will have two or even three corresponding scores. Take the example of ‘Joe Biden’, a two-word term with a score of XXX. This score indicates that conservatives tend to use the term ‘Joe Biden’ more than do liberals. However, both ‘Joe’ and ‘Biden’ are also included in the partisanship corpus, with two different scores, and as such, any tweet containing ‘Joe Biden’ will generate three scores, potentially distorting the results. To overcome this, we attempted to remove any one-word terms underlying two-word terms when a two-word term was present. This preserves the relative partisanship score of the underlying one-word terms, but utilizes the partisanship score of two-word terms when they appear in the text. 

From this dictionary, we created columns for 1) the number of terms identified in the partisanship corpus, 2) the sum of the values corresponding to  the identified terms, and 3) the sum of those scores divided by the number of terms. This final column partisanship_score will be our approximation of the relative partisanship of a given tweet. 

These columns were added to all datasets, including both the search results and the ‘For You’ datasets. We then performed additional data cleaning, removing tweets containing fewer than four identified terms to ensure that we were only scoring tweets with enough relevant data to derive a partisanship score. 

</p>

<a name="results"></a>
## Results and Analysis
<p>
  
Before determining the potential influence of followership on search results, it is first important to measure the uniqueness of search results. As Figures 1-3 demonstrate, Search results performed at the same time and in the same location produce relatively equivalent search results within the first few hundred results, but begin to decrease the further one scrolls in the results. In Figures XXX and YYY, the proportion of equivalent results diminishes over time before beginning to increase again later in the dataset. This can be explained by subsequent runs of the same search result, as the results are not infinite. 

 Turning to the partisanship scores recorded in the search results, a few notable results stand out. The first is that, for all but one dataset, the average partisanship scores for each dataset is more conservative than liberal (Figure X). This indicates that the text contained within the tweets provided by the search results align more closely with terms found in the conservative ‘For You’ dataset than they do with the ‘Liberal’ dataset. This was the case for both the no one and everyone ‘For You’ datasets, as well as the Trump/Biden search results and the immigration search results. The one search result that displayed a marginal alignment with terms found in the liberal dataset was our Gaza OR Israel OR Palestine query, a topic that we assumed would generate more discussion among progressive circles. 

  </p>
 
 <p>
   
<p align="center">
  <img width="749" img align="center" src="https://github.com/bensaldich/ddps_final/assets/71343656/6804a132-fa78-478e-afb4-f0ac52651dc1">

</p>
  
  **Figure 4: Partisanship of Datasets Relative to Partisanship Corpus**
  
  <p>

Given that each search result dataset pertains to a particular topic (immigration, Trump/Biden, or Israel/Palestine), there is no way to separate the partisanship of the words used from the partisanship of the underlying topic. This makes the partisanship of our search results more disputable. But the ‘For You’ feeds do not display such inherent partisanship, and because of this, the slight conservative lean of both the ‘no one’ and ‘everyone’ For You datasets suggests that Twitter might suggest slightly conservative-aligned to a user with a “neutral” Twitter feed (be that a feed that follow an equal mix of liberal and conservative voices, or a truly empty Twitter profile). The extent to which this phenomenon is replicable beyond our data remains to be seen, and further research will be necessary to ascertain whether this conservative preference is due to an algorithmic preference for conservative content, or a preference for more highly engaged content that happens to be conservative. 

Comparing the breakdown of partisanship scores within the search result pairs, we can both see the similarities of these search results (verified by the proportion of equivalent tweets shared between the pairs), as well as the relatively normal but right-leaning distribution of partisanship, with the largest proportion of Tweets containing a partisanship score between 0.4–0.6. 

Figure X depicts the distribution of scores based on the number of ID’d terms in each scores (following the removal of tweets containing fewer than 4 terms). It appears that the tweets coded as most liberal by and large have fewer ID'd terms.

  </p>

<a name="discussions"></a>
## Discussion
<p>
Discussion here

</p>


<a name="conclusions"></a>
## Conclusion
<p>
Conclusion here

</p>

<a name="bibliography"></a>
## References
<p>

 references here
</p>

