How to start
================


Natural language processing is a way to to have a computer understand human langugage. We can manipulate the data as we would do just with numbers but this time on words - it's almost... magical! So here I will apply natural language processing in R on the most magical of series, Harry Potter by J.K. Rowling in regards to sentiment analysis.

You will see that most of the analysis here is done in an inefficient or even "unsophisticated" way", but that's the point - it doesn't have to be difficult or complex! When I first dove into trying to this project, I felt overwhelmed at the idea of extracting information from just plain human language text! I hope that this simplified coding structure makes not only this project, but others seem more approachable.

You can find the plain text versions of the series [here](https://www.stats.ox.ac.uk/~snijders/siena/HarryPotterData.html). Also, if you plan to be playing around with this more this once (like I probably will), you can actually use a package on the [Harry Potter series](https://github.com/bradleyboehmke/harrypotter). Once you have your human language, in this case the Harry Potter novels, converted into a format that a computer can understanding, the possibilities are endless! Here I will play around with some different manipulations on data of the Harry Potter series.

First we must extract the data into a manageable form.
------------------------------------------------------

``` r
#Load Libraries
library(tidytext)
library(stringr)
library(stringi)
library(tidyverse)
```

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
library(ggplot2)
library(viridis)

#Load in the plain text book files
book1 <- "../data/Harry_Potter_and_the_Sorcerer's_Stone.txt"
book2 <- "../data/Harry_Potter_and_the_Chamber_of_Secrets.txt"
book3 <- "../data/Harry_Potter_and_the_Prisoner_of_Azkaban.txt"
book4 <- "../data/Harry_Potter_and_the_Goblet_of_Fire.txt"
book5 <-"../data/Harry_Potter_and_the_Order_of_the_Phoenix.txt"
book6 <- "../data/Harry_Potter_and_the_Half_Blood_Prince.txt"
book7 <-"../data/Harry_Potter_and_the_Deathly_Hallows.txt"

#Read in plain text book files, also converting all text to lower case 
data_1 <- tolower(readChar(book1, file.info(book1)$size))
data_2 <- tolower(readChar(book2, file.info(book2)$size))
data_3 <- tolower(readChar(book3, file.info(book3)$size))
data_4 <- tolower(readChar(book4, file.info(book4)$size))
data_5 <- tolower(readChar(book5, file.info(book5)$size))
data_6 <- tolower(readChar(book6, file.info(book6)$size))
data_7 <-  tolower(readChar(book7, file.info(book7)$size))

#Create one continuous string for each book data
data_1 <- str_replace_all(data_1, "[\r\n]" , "")
data_2 <- str_replace_all(data_2, "[\r\n]" , "")
data_3 <- str_replace_all(data_3, "[\r\n]" , "")
data_4 <- str_replace_all(data_4, "[\r\n]" , "")
data_5 <- str_replace_all(data_5, "[\r\n]" , "")
data_6 <- str_replace_all(data_6, "[\r\n]" , "")
data_7 <- str_replace_all(data_7, "[\r\n]" , "")
```

Now to dive into the analysis!
------------------------------

### Book Length

First, to get our feet wet, let's look at the total length of each book in comparison to the rest of the series by word count. The `library(stringri)` has a useful function known as `stri_stats_latex()` which returns different statistics on a character vector including for our purposes `Words` (number of words).

``` r
#Use library(stringri) to retrieve number of words per book
words1 <- stri_stats_latex(data_1)[4]
words2 <- stri_stats_latex(data_2)[4]
words3 <- stri_stats_latex(data_3)[4]
words4 <- stri_stats_latex(data_4)[4]
words5 <- stri_stats_latex(data_5)[4]
words6 <- stri_stats_latex(data_6)[4]
words7 <- stri_stats_latex(data_7)[4]
#Combine the above into a single vector
words <- c(words1, words2, words3, words4, words5, words6, words7)
```

Now numbers are great, but what is more telling is a visualization of the numbers. Let's look at a barplot of the above arranged from shortest to longest book in the series.

``` r
#Create vector of book titles
names <- c( "Sorcerer's Stone", "Chamber of Secrets", "Prisoner of Azkaban", "Goblet of Fire", "Order of the Phoenix", "Half Blood Prince", "Deathly Hallows")
#Manually arrange the data such that it goes from smallest to biggest word count
wordcount <- data.frame(names, words)
wordcount$words <- wordcount$words[order(wordcount$words, decreasing= FALSE)]
wordcount$names <- factor(wordcount$names, levels = wordcount$names[order(wordcount$words )])

#bar graph
wordcountbar <- ggplot(data = wordcount  , aes(x = names, y = words)) + 
  geom_bar(stat = "identity") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  ggtitle("Word Count by Book") +
  xlab("Book Title") +
  ylab("Total Word Count") 
ggsave("../results/total_wordcount_series.jpg")
```

    ## Saving 7 x 5 in image

``` r
wordcountbar
```
![](https://psarana.github.io/assets/images/total_wordcount_series.jpg)


Interestingly, we see that the length of the book increases in relation to it's position in the series. We see taht the Sorcer's Stone, the first book in the series, is also the shortest and for Deathly Hallows, the last book in the series, it is also the longest.

### Character Importance

If you know anything about the Harry Potter series (or if you are a bandwagoner who only watched the movies), you probably have an idea of the main characters. Some characters that come to mind when thinking about the Harry Potter series are (naturally) Harry, Ron, Hermione, Dumbledore, Snape and Voldemort.Let's see if that intuition relates to what is actually seen in the data we have. Importance will be equated to the number of mentions by first name and/or preferred name over the series.

``` r
# Sum mentions of important characters per book
d1 <- sum(str_count(data_1, "dumbledore"))
d2 <- sum(str_count(data_2, "dumbledore"))
d3 <- sum(str_count(data_3, "dumbledore"))
d4 <- sum(str_count(data_4, "dumbledore"))
d5 <- sum(str_count(data_5, "dumbledore"))
d6 <- sum(str_count(data_6, "dumbledore"))
d7 <- sum(str_count(data_7, "dumbledore"))
h1 <- sum(str_count(data_1, "harry"))
h2 <- sum(str_count(data_2, "harry"))
h3 <- sum(str_count(data_3, "harry"))
h4 <- sum(str_count(data_4, "harry"))
h5 <- sum(str_count(data_5, "harry"))
h6 <- sum(str_count(data_6, "harry"))
h7 <- sum(str_count(data_7, "harry"))
r1 <- sum(str_count(data_1, "ron"))
r2<- sum(str_count(data_2, "ron"))
r3 <- sum(str_count(data_3, "ron"))
r4 <- sum(str_count(data_4, "ron"))
r5 <- sum(str_count(data_5, "ron"))
r6 <- sum(str_count(data_6, "ron"))
r7 <- sum(str_count(data_7, "ron"))
e1 <- sum(str_count(data_1, "hermione"))
e2 <- sum(str_count(data_2, "hermione"))
e3 <- sum(str_count(data_3, "hermione"))
e4 <- sum(str_count(data_4, "hermione"))
e5 <- sum(str_count(data_5, "hermione"))
e6 <- sum(str_count(data_6, "hermione"))
e7 <- sum(str_count(data_7, "hermione"))
s1 <- sum(str_count(data_1, "snape"))
s2 <- sum(str_count(data_2, "snape"))
s3 <- sum(str_count(data_3, "snape"))
s4 <- sum(str_count(data_4, "snape"))
s5 <- sum(str_count(data_5, "snape"))
s6 <- sum(str_count(data_6, "snape"))
s7 <- sum(str_count(data_7, "snape"))
v1 <- sum(str_count(data_1, "voldemort"))
v2 <- sum(str_count(data_2, "voldemort"))
v3 <- sum(str_count(data_3, "voldemort"))
v4 <- sum(str_count(data_4, "voldemort"))
v5 <- sum(str_count(data_5, "voldemort"))
v6 <- sum(str_count(data_6, "voldemort"))
v7 <- sum(str_count(data_7, "voldemort"))

#Create Individual Vector of Counts per Character of Interest
dcount <- c(d1, d2, d3, d4, d5, d6, d7)
hcount <- c(h1, h2, h3, h4, h5, h6, h7)
rcount <- c(r1, r2, r3, r4, r5, r6, r7)
ecount <- c(e1, e2, e3, e4, e5, e6, e7)
scount <- c(s1, s2, s3, s4, s5, s5, s7)
vcount <- c(v1, v2, v3, v4, v5, v6, v7)

#Tidy this up into a Manageable Data Frame
charscount <- data.frame(names)
charscount$dumbledore <- dcount
charscount$harry <- hcount
charscount$ron <- rcount
charscount$hermione <- ecount
charscount$snape <- scount
charscount$voldemort <- vcount
print(charscount)
```

    ##                  names dumbledore harry  ron hermione snape voldemort
    ## 1     Sorcerer's Stone        160  1326  578      271   171        38
    ## 2   Chamber of Secrets        159  1655  844      321    98        25
    ## 3  Prisoner of Azkaban        163  2049 1036      672   246        48
    ## 4       Goblet of Fire        601  3172 1357      872   256       237
    ## 5 Order of the Phoenix        661  4091 1719     1307   384       212
    ## 6    Half Blood Prince       1035  2784 1166      695   384       244
    ## 7      Deathly Hallows        593  3117 1479     1218   297       452

Again, like I mentioned above, numbers are great - but visualizations are definitely better. How I see it, numbers give you a sense of knowing an amount percisely but visualizations are able to give you a sense of context of the importance of that amount in relation to others. With this thought in mind, let's take the numbers we found for mentions of characters per book and book it into a bar graph.

``` r
#bar graph
dcountp <- ggplot(data = charscount  , aes(x = names), group = 1) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  geom_line(aes(y = dcount, colour = "Dumbledore"), group = 1) +
  geom_line(aes(y = hcount, colour = "Harry"), group =1) +
  geom_line(aes(y = rcount, colour = "Ron"), group = 1) +
  geom_line(aes(y = vcount, colour = "Voldemort"), group =1 ) +
  geom_line(aes(y = ecount, colour = "Hermione"), group = 1) +
  geom_line(aes(y = scount, colour = "Snape"), group = 1) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) + 
  ggtitle("Character Mentions in Harry Potter Series") +
  ylab("Total Mentions") +
  scale_x_discrete("Book", labels = c(names)) + 
  scale_colour_manual("", 
                      breaks = c("Harry", "Ron", "Hermione", "Dumbledore" , "Voldemort", "Snape"),
                      values = c("#e41a1c", "#377eb8",  "#ff7f00", "#4daf4a", "#984ea3", "#ffff33"))

dcountp
```

![](https://psarana.github.io/assets/images/main_character_mentions.jpg)

``` r
ggsave("../results/main_character_mentions.jpg")
```

    ## Saving 7 x 5 in image

From our visualization, we are able to see the importance (as defined by total character numbers across the series), of each character in relation to other main characters. As we suspected Harry, Ron and Hermione are at the top - except in Goblet of Fire where Professor Dumbledore is actually by our measure of importance, more of an important character than Hermione.

There are many missing pieces with this analysis. My fellow Potterheads will know that many of these characters go by many different aliases. Looking specifically at Lord Voldemort, he is also referred to as "He-Who-Must-Be-Named" or the "Dark Lord". These type of details will enhance our analysis and make a more accurate conclusion in regards to ranking the importance of main characters. This is a great next step.

I hope this inspired you to ask more questions about this data - where can we go from here? I will continue to play around with this data set in a series, A beginners guide to Natural Language Processing, where code and analysis will get exceedingly more sophisticated but I hope this took away the fear of starting for those hoping to dabble in the field.
