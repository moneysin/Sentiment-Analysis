Untitled
================

GitHub Documents
----------------

This is an R Markdown format used for publishing markdown documents to GitHub. When you click the **Knit** button all R code chunks are run and a markdown file (.md) suitable for publishing to GitHub is generated.

Including Code
--------------

You can include R code in the document as follows:

Libraries used to run the functions used in the code
----------------------------------------------------

``` r
library(data.table)
```

    ## Warning: package 'data.table' was built under R version 3.4.3

``` r
library(ggplot2)
library(wordcloud)
```

    ## Warning: package 'wordcloud' was built under R version 3.4.3

    ## Loading required package: RColorBrewer

``` r
library(caTools)
```

    ## Warning: package 'caTools' was built under R version 3.4.3

``` r
library(xgboost)
```

    ## Warning: package 'xgboost' was built under R version 3.4.3

``` r
library(tm)
```

    ## Warning: package 'tm' was built under R version 3.4.3

    ## Loading required package: NLP

    ## 
    ## Attaching package: 'NLP'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     annotate

``` r
library(e1071)
library(RWeka)
```

    ## Warning: package 'RWeka' was built under R version 3.4.3

    ## 
    ## Attaching package: 'RWeka'

    ## The following object is masked from 'package:caTools':
    ## 
    ##     LogitBoost

Loading data set
----------------

The dataset contains 25000 rows and 3 columns, containing id: generated for each review, sentiments: whether positive(1) or negative(0) based on reviews, review: describing the text

``` r
bag= as.data.frame(read.table("labeledTrainData.tsv", header=T))
str(bag)
```

    ## 'data.frame':    25000 obs. of  3 variables:
    ##  $ id       : Factor w/ 25000 levels "0_3","0_9","1_1",..: 15704 8076 20024 10851 23882 20996 18707 1415 9871 22146 ...
    ##  $ sentiment: int  1 1 0 0 1 1 0 0 0 1 ...
    ##  $ review   : Factor w/ 24904 levels "\b\b\b\bA Turkish Bath sequence in a film noir located in New York in the 50's, that must be a hint at somethin"| __truncated__,..: 24348 517 17316 12571 16735 7766 20881 11186 1345 1049 ...

Text Preprocessing
------------------

``` r
#create a corpus of descriptions
text_corpus <- Corpus(VectorSource(bag$review))
```

``` r
#check first 4 documents
inspect(text_corpus[1:4])
```

    ## <<SimpleCorpus>>
    ## Metadata:  corpus specific: 1, document level (indexed): 0
    ## Content:  documents: 4
    ## 
    ## [1] With all this stuff going down at the moment with MJ i've started listening to his music, watching the odd documentary here and there, watched The Wiz and watched Moonwalker again. Maybe i just want to get a certain insight into this guy who i thought was really cool in the eighties just to maybe make up my mind whether he is guilty or innocent. Moonwalker is part biography, part feature film which i remember going to see at the cinema when it was originally released. Some of it has subtle messages about MJ's feeling towards the press and also the obvious message of drugs are bad m'kay.<br /><br />Visually impressive but of course this is all about Michael Jackson so unless you remotely like MJ in anyway then you are going to hate this and find it boring. Some may call MJ an egotist for consenting to the making of this movie BUT MJ and most of his fans would say that he made it for the fans which if true is really nice of him.<br /><br />The actual feature film bit when it finally starts is only on for 20 minutes or so excluding the Smooth Criminal sequence and Joe Pesci is convincing as a psychopathic all powerful drug lord. Why he wants MJ dead so bad is beyond me. Because MJ overheard his plans? Nah, Joe Pesci's character ranted that he wanted people to know it is he who is supplying drugs etc so i dunno, maybe he just hates MJ's music.<br /><br />Lots of cool things in this like MJ turning into a car and a robot and the whole Speed Demon sequence. Also, the director must have had the patience of a saint when it came to filming the kiddy Bad sequence as usually directors hate working with one kid let alone a whole bunch of them performing a complex dance scene.<br /><br />Bottom line, this movie is for people who like MJ on one level or another (which i think is most people). If not, then stay away. It does try and give off a wholesome message and ironically MJ's bestest buddy in this movie is a girl! Michael Jackson is truly one of the most talented people ever to grace this planet but is he guilty? Well, with all the attention i've gave this subject....hmmm well i don't know because people can be different behind closed doors, i know this for a fact. He is either an extremely nice but stupid guy or one of the most sickest liars. I hope he is not the latter.                                                                                                                                                         
    ## [2] "The Classic War of the Worlds" by Timothy Hines is a very entertaining film that obviously goes to great effort and lengths to faithfully recreate H. G. Wells' classic book. Mr. Hines succeeds in doing so. I, and those who watched his film with me, appreciated the fact that it was not the standard, predictable Hollywood fare that comes out every year, e.g. the Spielberg version with Tom Cruise that had only the slightest resemblance to the book. Obviously, everyone looks for different things in a movie. Those who envision themselves as amateur "critics" look only to criticize everything they can. Others rate a movie on more important bases,like being entertained, which is why most people never agree with the "critics". We enjoyed the effort Mr. Hines put into being faithful to H.G. Wells' classic novel, and we found it to be very entertaining. This made it easy to overlook what the "critics" perceive to be its shortcomings.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             
    ## [3] The film starts with a manager (Nicholas Bell) giving welcome investors (Robert Carradine) to Primal Park . A secret project mutating a primal animal using fossilized DNA, like Â¨Jurassik ParkÂ¨, and some scientists resurrect one of nature's most fearsome predators, the Sabretooth tiger or Smilodon . Scientific ambition turns deadly, however, and when the high voltage fence is opened the creature escape and begins savagely stalking its prey - the human visitors , tourists and scientific.Meanwhile some youngsters enter in the restricted area of the security center and are attacked by a pack of large pre-historical animals which are deadlier and bigger . In addition , a security agent (Stacy Haiduk) and her mate (Brian Wimmer) fight hardly against the carnivorous Smilodons. The Sabretooths, themselves , of course, are the real star stars and they are astounding terrifyingly though not convincing. The giant animals savagely are stalking its prey and the group run afoul and fight against one nature's most fearsome predators. Furthermore a third Sabretooth more dangerous and slow stalks its victims.<br /><br />The movie delivers the goods with lots of blood and gore as beheading, hair-raising chills,full of scares when the Sabretooths appear with mediocre special effects.The story provides exciting and stirring entertainment but it results to be quite boring .The giant animals are majority made by computer generator and seem totally lousy .Middling performances though the players reacting appropriately to becoming food.Actors give vigorously physical performances dodging the beasts ,running,bound and leaps or dangling over walls . And it packs a ridiculous final deadly scene. No for small kids by realistic,gory and violent attack scenes . Other films about Sabretooths or Smilodon are the following : Â¨Sabretooth(2002)Â¨by James R Hickox with Vanessa Angel, David Keith and John Rhys Davies and the much better Â¨10.000 BC(2006)Â¨ by Roland Emmerich with with Steven Strait, Cliff Curtis and Camilla Belle. This motion picture filled with bloody moments is badly directed by George Miller and with no originality because takes too many elements from previous films. Miller is an Australian director usually working for television (Tidal wave, Journey to the center of the earth, and many others) and occasionally for cinema ( The man from Snowy river, Zeus and Roxanne,Robinson Crusoe ). Rating : Below average, bottom of barrel.
    ## [4] It must be assumed that those who praised this film ("the greatest filmed opera ever," didn't I read somewhere?) either don't care for opera, don't care for Wagner, or don't care about anything except their desire to appear Cultured. Either as a representation of Wagner's swan-song, or as a movie, this strikes me as an unmitigated disaster, with a leaden reading of the score matched to a tricksy, lugubrious realisation of the text.<br /><br />It's questionable that people with ideas as to what an opera (or, for that matter, a play, especially one by Shakespeare) is "about" should be allowed anywhere near a theatre or film studio; Syberberg, very fashionably, but without the smallest justification from Wagner's text, decided that Parsifal is "about" bisexual integration, so that the title character, in the latter stages, transmutes into a kind of beatnik babe, though one who continues to sing high tenor -- few if any of the actors in the film are the singers, and we get a double dose of Armin Jordan, the conductor, who is seen as the face (but not heard as the voice) of Amfortas, and also appears monstrously in double exposure as a kind of Batonzilla or Conductor Who Ate Monsalvat during the playing of the Good Friday music -- in which, by the way, the transcendant loveliness of nature is represented by a scattering of shopworn and flaccid crocuses stuck in ill-laid turf, an expedient which baffles me. In the theatre we sometimes have to piece out such imperfections with our thoughts, but I can't think why Syberberg couldn't splice in, for Parsifal and Gurnemanz, mountain pasture as lush as was provided for Julie Andrews in Sound of Music...<br /><br />The sound is hard to endure, the high voices and the trumpets in particular possessing an aural glare that adds another sort of fatigue to our impatience with the uninspired conducting and paralytic unfolding of the ritual. Someone in another review mentioned the 1951 Bayreuth recording, and Knappertsbusch, though his tempi are often very slow, had what Jordan altogether lacks, a sense of pulse, a feeling for the ebb and flow of the music -- and, after half a century, the orchestral sound in that set, in modern pressings, is still superior to this film.

``` r
## clean the data

#remove br
text_corpus= tm_map(text_corpus, removeWords, "br")

#remove punctuation
text_corpus= tm_map(text_corpus, removePunctuation)

#convert into lower case
text_corpus = tm_map(text_corpus,tolower)


#remove number
text_corpus= tm_map(text_corpus, removeNumbers)


#remove stopwords
text_corpus= tm_map(text_corpus, removeWords, c(stopwords("english")))


#remove whitespaces
text_corpus= tm_map(text_corpus, stripWhitespace)


#convert to text document
text_corpus=tm_map(text_corpus, PlainTextDocument)


#perform stemming this should always be performed after text doc conversion
text_corpus= tm_map(text_corpus, stemDocument, language="english")

#inspect(text_corpus)
text_corpus$content=iconv(text_corpus$content,to='utf-8')
```

``` r
#convert to document term matrix
docterm_corpus = DocumentTermMatrix(text_corpus)
dim(docterm_corpus)
```

    ## [1] 25000 79841

``` r
#remove sparse terms
newdocterm_corpus= removeSparseTerms(docterm_corpus, sparse=0.95)
dim(newdocterm_corpus)
```

    ## [1] 25000   377

This gives the words which are present in atleast 5% of the documents and 95% shows the sparsity meaning which are not relevant for analysis purpose

Finding frequency of the terms which are present in atlest 5% of the documents
------------------------------------------------------------------------------

``` r
#find frequent terms
cols= colSums(as.matrix(newdocterm_corpus))
docfeatures= data.table(name= names(cols), count=cols)

#most frequent and least frequent terms
docfeatures[order(-count)][1:10]
```

    ##        name count
    ##  1:    movi 50592
    ##  2:    film 47232
    ##  3:     one 26909
    ##  4:    like 22185
    ##  5:    just 17656
    ##  6:    time 15540
    ##  7:    good 14925
    ##  8:    make 14547
    ##  9:     get 14065
    ## 10: charact 13993

``` r
docfeatures[order(count)][1:10]
```

    ##           name count
    ##  1: throughout  1361
    ##  2:       stop  1421
    ##  3:     overal  1423
    ##  4:      basic  1446
    ##  5:      light  1457
    ##  6:    impress  1458
    ##  7:    qualiti  1459
    ##  8:       went  1460
    ##  9:     extrem  1462
    ## 10:    written  1468

Plotting ggplot to see which word has count/frequency above 10000
-----------------------------------------------------------------

``` r
ggplot(docfeatures[count>10000], aes(reorder(name, -count), count)) + geom_bar(stat="identity", fill="red", color="black")
```

![](guithub_files/figure-markdown_github/unnamed-chunk-8-1.png)

``` r
#check association of terms of top features
findAssocs(newdocterm_corpus,"movi", corlimit=0.7)
```

    ## $movi
    ## numeric(0)

Wordcloud
---------

``` r
wordcloud(names(cols), cols, min.freq = 100,scale=c(6,0.1), colors=brewer.pal(6, "Dark2"))
```

![](guithub_files/figure-markdown_github/unnamed-chunk-10-1.png)

Preparing f=dataframe and then splitting the data into train and test set
-------------------------------------------------------------------------

``` r
train = as.data.frame(as.matrix(newdocterm_corpus))
train$sentiment = bag$sentiment
colnames(train)=make.names(colnames(train))

#Splitting
sp= sample.split(train$sentiment, SplitRatio = 0.8)

#Forming train and test set
sp_train= subset(train, sp==T)
sp_test= subset(train, sp==F)
```

Different algorithms to test the model
--------------------------------------

1.  Logistic Regression

``` r
model_lg<- glm (sentiment ~ ., data = sp_train, family = binomial(link='logit'))
```

    ## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

``` r
pred =  predict(model_lg, newdata= sp_test , type='response')
pred= ifelse(pred>0.5,1,0)
confmat= table(pred, sp_test$sentiment)
accuracy= sum(diag(confmat))/sum(confmat)
accuracy
```

    ## [1] 0.8232

1.  Naive Bayes

``` r
model_nm=naiveBayes(as.factor(sentiment)~., data=sp_train)
pred= predict(model_nm, sp_test)
confmat= table(pred, sp_test$sentiment)
accuracy= sum(diag(confmat))/sum(confmat)
accuracy
```

    ## [1] 0.7944

Till now it was unigram model

Creating a bigram model and the same steps has to be followed,just tweaking in between, instead of corpus use of Vcorpus and for bigram use 2 words at a time
-------------------------------------------------------------------------------------------------------------------------------------------------------------

``` r
#create a corpus of descriptions
vcorpus <- VCorpus(VectorSource(bag$review))

## clean the data


#remove br
vcorpus= tm_map(vcorpus, removeWords, "br")


#remove punctuation
vcorpus= tm_map(vcorpus, removePunctuation)

#convert into lower case
vcorpus = tm_map(vcorpus,tolower)


#remove number
vcorpus= tm_map(vcorpus, removeNumbers)


#remove stopwords
vcorpus= tm_map(vcorpus, removeWords, c(stopwords("english")))


#remove whitespaces
vcorpus= tm_map(vcorpus, stripWhitespace)


#convert to text document
vcorpus=tm_map(vcorpus, PlainTextDocument)


#perform stemming this should always be performed after text doc conversion
vcorpus= tm_map(vcorpus, stemDocument, language="english")
```

Use a bigram tokenizer to create document term matrices with bigrams
--------------------------------------------------------------------

``` r
bigramtoken= function(x) NGramTokenizer(x, Weka_control(min=2, max=2))
dtm_v= DocumentTermMatrix(vcorpus, control= list(tokenize= bigramtoken))
dtm_v_sparse= removeSparseTerms(dtm_v, 0.993)
```

Preparing the data frame
------------------------

``` r
train = as.data.frame(as.matrix(dtm_v_sparse))
train$sentiment = bag$sentiment
colnames(train)=make.names(colnames(train))

#Splitting the data
sp= sample.split(train$sentiment, SplitRatio = 0.8)

#Formimg training and testing data
sp_train= subset(train, sp==T)
sp_test= subset(train, sp==F)
```

Different algorithms to test the model

1)Logistic Regression

``` r
model_lg<- glm (sentiment ~ ., data = sp_train, family = binomial(link='logit'))
pred =  predict(model_lg, newdata= sp_test , type='response')
pred= ifelse(pred>0.5,1,0)
confmat= table(pred, sp_test$sentiment)
accuracy= sum(diag(confmat))/sum(confmat)
accuracy
```

    ## [1] 0.7166

1.  Naive Bayes

``` r
model_nm=naiveBayes(as.factor(sentiment)~., data=sp_train)
pred= predict(model_nm, sp_test)
confmat= table(pred, sp_test$sentiment)
accuracy= sum(diag(confmat))/sum(confmat)
accuracy
```

    ## [1] 0.6954

Including Plots
---------------

You can also embed plots, for example:

![](guithub_files/figure-markdown_github/pressure-1.png)

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
