---
title: "String Manipulation"
author:    
    -  "RSN_Thinklabz_Chennai"

output:
  html_document:
    keep_md: true
    toc: yes
  
---





# Introduction
This notes is aimed at explaining methods in R to manipulate Strings using 'base' and 'stringr' package.   
  
  
- What is String manipulation ?   
String manipulation (or string handling) is the process of changing, parsing, splicing, pasting or analyzing strings.

- Why String manipulation is Important ?   
Strings usually contain most important information about individual, institution, location etc. handling this data is paramount in Data Science. Effective handling of strings will help us in understanding the data better.  
   
- What can we do with string data ?  
In R we can perform variety of operations with string data. the ones we are going to cover in this notes is given below   
   01) Finding Length of String  
   02) Casefolding   
   03) Title form   
   04) Sentence form   
   05) Trim   
   06) Truncating String   
   07) Abbreviate 
   08) Join
   09) Substring
   10) Splitting string   
   11) Set union      
   12) Set intersection 
   13) Set difference    
   14) Set equality     
   15) Existence of string in a set   
   16) Sorting    
   17) Find pattern / position
   18) Finding number of times a pattern occurs in a string
   19) Find and replace
   20) Extracting word

Let us dive into World of strings

# Loading package needed
Base package is already loaded by default. We just need to load stringr. Please install the package if you are using it for the first time

```r
library(stringr)
```

For functions in stringr package we shall denote it as 
**`stringr::function()`** throughout the notes

# Datasets used

We are making use of datasets available with stringr package

```r
data("fruit")
data("sentences")
```

## Know more about data

### A Vector of words
   
'fruit' data available in 'stringr' package has the names of fruits in a form of vector.   
   The following is a glimpse of fruit data 
<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Names of fruits</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> apple </td>
  </tr>
  <tr>
   <td style="text-align:left;"> apricot </td>
  </tr>
  <tr>
   <td style="text-align:left;"> avocado </td>
  </tr>
  <tr>
   <td style="text-align:left;"> banana </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bell pepper </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bilberry </td>
  </tr>
</tbody>
</table>
 

```
Fruit data totally have 80 fruit names and these are individual srings in a vector
```



### A Vector of sentences
'sentences' data which is available in 'stringr' package has a vector of  sentences which can be used for learning string manipulation.   
The following is a glimpse of 'sentence' data 
<table class="table table-striped" style="width: auto !important; ">
<caption>vector of sentences</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> The birch canoe slid on the smooth planks. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Glue the sheet to the dark blue background. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> It's easy to tell the depth of a well. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> These days a chicken leg is a rare dish. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rice is often served in round bowls. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> The juice of lemons makes fine punch. </td>
  </tr>
</tbody>
</table>


```
Number of sentences we totally have is  720 and these are individual srings in a vector
```

We shall use some of the fruit names and sentences randomly from the above dataset in the notes

# What can we do with String data

## Finding Length of String
Length of string can be obtained using **`stringr::str_length(string name)`** function

```r
sample_fruit=fruit[c(8,16,24,32)]
sample_fruit
```

```
[1] "blackcurrant" "cherry"       "damson"       "goji berry"  
```

```r
str_length(sample_fruit)
```

```
[1] 12  6  6 10
```


## Casefolding string
In some situations we may wish to process our string data in a way to either convert them all to upper or lower case. This can be done with **`casefold()`**, we give input vector and **`upper=T/F`** argument is used to specify whether we wish to change it to upper case or lower case. By default it taked the argument as upper=F

```r
#To uppercase
f_upper=casefold(head(fruit),upper=T)
print(f_upper)
```

```
[1] "APPLE"       "APRICOT"     "AVOCADO"     "BANANA"      "BELL PEPPER"
[6] "BILBERRY"   
```


```r
#To lowercase
f_lower=casefold(head(fruit),upper=F)
print(f_lower)
```

```
[1] "apple"       "apricot"     "avocado"     "banana"      "bell pepper"
[6] "bilberry"   
```

In some scenario we wish to capitalize each word in a string and this can be done using **`stringr::str_to_title()`**, we specify input vector and **`locale`** is used for representing string in other language,default is English 

```r
# Sample data
sample=sentences[c(2,3)]
print(sample)
```

```
[1] "Glue the sheet to the dark blue background."
[2] "It's easy to tell the depth of a well."     
```

```r
#Each word capitalized
cap=str_to_title(sample, locale = "en")
print(cap)
```

```
[1] "Glue The Sheet To The Dark Blue Background."
[2] "It's Easy To Tell The Depth Of A Well."     
```

```r
# setting locale as Turkish
str_to_title("it is invariable",locale="tr")
```

```
[1] "It Is Invariable"
```


Capitalizing a sentence can be done using **`stringr::str_to_sentence()`**

```r
string_2=casefold(sentences[c(19,40)])
string_2
```

```
[1] "the salt breeze came across from the sea."
[2] "what joy there is in living."             
```

```r
sent=str_to_sentence(string_2)
print(sent)
```

```
[1] "The salt breeze came across from the sea."
[2] "What joy there is in living."             
```

## Trimming white spaces
White spaces in a string data can be removed using **`trimws()`**, **`which`** argument is used to specify whether we wish to trim right side or left side or both sides. 

```r
#text with blank line in the beginning
text="   
    
    
There are no secrets to success. It is the result of preparation, hard work, and learning from failure.                                 "
#Original text
print(text)
```

```
[1] "   \n    \n    \nThere are no secrets to success. It is the result of preparation, hard work, and learning from failure.                                 "
```

```r
#By default it trims both sides
trimws(text)
```

```
[1] "There are no secrets to success. It is the result of preparation, hard work, and learning from failure."
```

```r
#we can specify with lesser letters also for specifying which side to trim
trimws(text,which="l")#for left side
```

```
[1] "There are no secrets to success. It is the result of preparation, hard work, and learning from failure.                                 "
```

```r
trimws(text,"ri")#for right side
```

```
[1] "   \n    \n    \nThere are no secrets to success. It is the result of preparation, hard work, and learning from failure."
```

```r
trimws(text,"bot")#for both sides
```

```
[1] "There are no secrets to success. It is the result of preparation, hard work, and learning from failure."
```

## Truncating a String
A string can be truncated in to specified width (minimum width is 5). This can be done using **`stringr::str_trunc()`**, we need to give the string as input and **`width`** argument is used to specify width of resulting string and **`side `** argument is used to specify which side to truncate i.e right, left or center.An useful application of this can be found where we are asked to enter recovery email for security reasons while logging in to mail.

```r
string1=c("person1useraccount@mail.com","user2account@mail.com")
#center truncate
str_trunc(string1,width=max(6,round(nchar(string1)/2)),side="center")
```

```
[1] "person...l.com" "user2a...l.com"
```

```r
string_2=sentences[c(1,55)];string_2
```

```
[1] "The birch canoe slid on the smooth planks."
[2] "A saw is a tool used for making boards."   
```

```r
#Right truncate
str_trunc(string_2,width=max(6,round(nchar(string_2)/2)),side="right")
```

```
[1] "The birch canoe sl..." "A saw is a tool us..."
```

```r
#Left truncate
str_trunc(string_2,width=max(6,round(nchar(string_2)/2)),side="left")
```

```
[1] "...the smooth planks." "...for making boards."
```

## To abbreviate strings
Sentences can be abbreviated with the help of **`abbreviate()`** function.   
This function takes the string and gives back the abbreviation based on number of minimum characters we specify (by default it is 4 ) and considers mostly first letters of words which appears after space and subsequent letters from other words are taken hierarchically if needed. We use **`minlength`** argument to specify minimum length of the abbreviation. For any specified minlength it takes all the first letters of words and length of abbreviation is always greater than or equal to number of words. 
**`use.classes`** argument is used to mention whether hierarchically lower case letters should be removed first or not

```r
some_statements=c("Rolling on floor laughing","one"," Be right back","In my opinion","To be announced","as soon as possible")
abbreviation=abbreviate(some_statements,minlength=1,use.classes = F)
as.data.frame(abbreviation)->y

kbl(y,caption="abbreviate example") %>%
  kable_styling(bootstrap_options = "striped", full_width = F, position = "left")
```

<table class="table table-striped" style="width: auto !important; ">
<caption>abbreviate example</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> abbreviation </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Rolling on floor laughing </td>
   <td style="text-align:left;"> Rofl </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one </td>
   <td style="text-align:left;"> o </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Be right back </td>
   <td style="text-align:left;"> Brb </td>
  </tr>
  <tr>
   <td style="text-align:left;"> In my opinion </td>
   <td style="text-align:left;"> Imo </td>
  </tr>
  <tr>
   <td style="text-align:left;"> To be announced </td>
   <td style="text-align:left;"> Tba </td>
  </tr>
  <tr>
   <td style="text-align:left;"> as soon as possible </td>
   <td style="text-align:left;"> asap </td>
  </tr>
</tbody>
</table>


## To Join multiple string objects to single string

With the **`paste()/paste0()`**  function we can merge several strings in a vector in to one string. We can use **`collapse`**  argument within the function for this purpose

```r
words=fruit[c(1,11,43,17,47)];words
```

```
[1] "apple"        "boysenberry"  "kumquat"      "chili pepper" "lychee"      
```

```r
paste0(words,collapse = " ")
```

```
[1] "apple boysenberry kumquat chili pepper lychee"
```

joining multiple individual strings in to single string can be done using **`sep`** argument in the function

```r
x2="This "
x3=" is "
x4=" an "
x5=" example"
paste(x2,x3,x4,x5,sep = "_")
```

```
[1] "This _ is _ an _ example"
```

```r
paste(x2,x3,x4,x5,sep = "|")
```

```
[1] "This | is | an | example"
```

## Getting substring

When we are interested in some part of a string we use **`substr(string,start,stop)`** function to slice it from original using position. We are giving start and end positions of the characters as arguments in the function.


```r
#Preparing long string
long_string=paste0(sentences[c(1,44,25,66)],collapse = " ")
long_string
```

```
[1] "The birch canoe slid on the smooth planks. The wide road shimmered in the hot sun. The beauty of the view stunned the young boy. Cars and busses stalled in snow drifts."
```

```r
# Getting substring
substr(long_string,1,42)
```

```
[1] "The birch canoe slid on the smooth planks."
```

## Splitting string in to list

Sometimes a long string needs to be split into parts so that each word of it can be accessed for particular purpose. **`strsplit(x,split="character used to split")`** function is used to break a single string in to list of strings. **`split`** is used tospecify which character we use for splitting


```r
# For splitting we can use either single character or more
long_string=paste0(sentences[c(1,44,25,66)],collapse = " ");long_string
```

```
[1] "The birch canoe slid on the smooth planks. The wide road shimmered in the hot sun. The beauty of the view stunned the young boy. Cars and busses stalled in snow drifts."
```

```r
sample_2 =strsplit(long_string,split="\\.")# using escape sequence for .
# using /
strsplit("20/abc 56/90t/1","/")
```

```
[[1]]
[1] "20"     "abc 56" "90t"    "1"     
```

```r
#using B
strsplit("GeniusBisBpatience.","B")
```

```
[[1]]
[1] "Genius"    "is"        "patience."
```

```r
#using 1
strsplit("It, is` the weight,1 not numbers of experiments that is to be regarded","1"
)
```

```
[[1]]
[1] "It, is` the weight,"                               
[2] " not numbers of experiments that is to be regarded"
```

## List to  Vector of strings
Once we obtain the list we can convert it to iterable form called as Character vector using **`unlist(list_name)`**

```r
#list to vector
unlist(sample_2)
```

```
[1] "The birch canoe slid on the smooth planks"    
[2] " The wide road shimmered in the hot sun"      
[3] " The beauty of the view stunned the young boy"
[4] " Cars and busses stalled in snow drifts"      
```

## Set Operations
R has dedicated functions for performing set operations on two given "character" vectors. Let's see some of them below

### Set union
For uniting two sets of strings we use **`union(set1,set2)`**. The resulting set will contain elements exist in either set1 or set2


```r
#Set union
set=fruit[c(1,5,9,24)];set
```

```
[1] "apple"        "bell pepper"  "blood orange" "damson"      
```

```r
set1=fruit[c(1,2,10,25,9)];set1
```

```
[1] "apple"        "apricot"      "blueberry"    "date"         "blood orange"
```

```r
union(set,set1)
```

```
[1] "apple"        "bell pepper"  "blood orange" "damson"       "apricot"     
[6] "blueberry"    "date"        
```

### Set intersection
To get the common elements of 2 sets we use **`intersect(set1,set2)`**.  The resulting set will contain elements that exist in both set1 and set2

```r
#set intersection
set
```

```
[1] "apple"        "bell pepper"  "blood orange" "damson"      
```

```r
set1
```

```
[1] "apple"        "apricot"      "blueberry"    "date"         "blood orange"
```

```r
intersect(set,set1)
```

```
[1] "apple"        "blood orange"
```

### Difference
We might be interested in getting the difference of the elements between two character vectors i.e strings which are in set1 but not in 2. This can be done using **`setdiff(set1,set2)`**

```r
# two character vectors
set1 <- c("some", "random", "few", "words")
set2 <- c("some", "many", "none", "few")

# difference between set5 and set6
setdiff(set1, set2)
```

```
[1] "random" "words" 
```

### Equality (without order)
The function **`setequal()`** allows us to test the equality of two character vectors. If the vectors contain the same elements, this returns TRUE (FALSE otherwise) 

```r
# Equality without order
# three character vectors
set3 <- c("some", "random", "strings")
set4 <- c("thor", "bean", "teddy", "few")
set5 <- c("strings", "random", "some")

# Equality of set 3 and 4
setequal(set3, set4)
```

```
[1] FALSE
```

```r
#Equality of set 3 and 5 
setequal(set3, set5)
```

```
[1] TRUE
```

### Exact equality (with order)
Sometimes we want to test whether two vectors are exactly equal (element by element). For instance, testing if set3 is exactly equal to set5. Although both vectors contain the same set of elements, they are not exactly the same vector. Such test can be performed with the function **`identical()`**

```r
set3 <- c("some", "random", "strings")

set5 <- c("strings", "random", "some")

# Equality with Order
# set3 identical to set3?
identical(set3, set3)
```

```
[1] TRUE
```

```r
# set3 identical to set5?
identical(set3, set5)
```

```
[1] FALSE
```

### Element contained with
If we wish to test if an element is contained in a given set of character strings we can do so with **`is.element()`**

```r
# three vectors
set10 <- c("some", "stuff", "to", "play", "with")
element_1 <- "play"
element_2 <- "crazy"

# element 1 in set10?
is.element(element_1, set10)
```

```
[1] TRUE
```

```r
# element 2 in set10?
is.element(element_2, set10)
```

```
[1] FALSE
```

Alternatively, we can use the binary operator **`%in%`** to test if an element is contained in a given set. The function returns TRUE if the first operand is contained in the second, and it returns FALSE otherwise

```r
# element 1 in set10?
element_1 %in% set10
```

```
[1] TRUE
```


## Sorting
The function **`sort()`** allows you to sort the elements of a vector, either in increasing order (by default) or in decreasing order using the argument decreasing.   
*NOTE *: Numbers are always before alphabets

```r
# Soting Character vector(default is increasing)
set11 = c("today", "produced", "example", "beautiful", "a", "nicely","10000")

# sort (increasing order)
sort(set11)
```

```
[1] "10000"     "a"         "beautiful" "example"   "nicely"    "produced" 
[7] "today"    
```

```r
# sort (decreasing order)
sort(set11, decreasing = TRUE)
```

```
[1] "today"     "produced"  "nicely"    "example"   "beautiful" "a"        
[7] "10000"    
```

## Finding Existence of pattern
Finding Existence of word in a string is useful since keywords play major role in search algorithms.   
Let's take3 sentences randomly from our data

```r
s=sentences[c(3,24,221)]
# Let us see all the three random sentences we have taken.
print(s)
```

```
[1] "It's easy to tell the depth of a well." 
[2] "The swan dive was far short of perfect."
[3] "A pencil with black lead writes best."  
```
Now if we wish to know whether 'the' exists in the sentences we can use **`str_detect()`** 
this returns Boolean values indicating presence 

```r
str_detect(s, "the")
```

```
[1]  TRUE FALSE FALSE
```

so the pattern "the" exists only in 1st sentence of our word as per the function.   
The word 'the' exists in 1st and 2nd sentences but in 2nd sentence first letter T is capitalized. To detect the pattern ignoring the case whether upper or lower we can use **`ignore_case`** argument.   

```r
str_detect(s,regex( "the",ignore_case=T))
```

```
[1]  TRUE  TRUE FALSE
```

## Finding a pattern's position of occurence in vector of strings
**`grep()`**
This function searches for matches of certain character pattern in a vector of character strings and returns the indices that yield a match.The function searches for matches of certain character pattern in a vector of character strings and returns a logical vector indicating which elements of the vector contained a match.

```r
#getting position of occurence
grep("s1",c("science group s1","science group","another group s1"))
```

```
[1] 1 3
```

```r
#getting boolean values indication presence
grepl("s1",c("science group s1","science group","another group s1"))
```

```
[1]  TRUE FALSE  TRUE
```

## Counting number of times pattern occurs
We can get the count which indicates number of times a pattern occurs in a string using **`stringr::str_count()`**

```r
string=c("s1 science group s1","science group","another group s1");string
```

```
[1] "s1 science group s1" "science group"       "another group s1"   
```

```r
str_count(string, "s1")
```

```
[1] 2 0 1
```

## Find and replace
**`gsub(pattern, replacement, x ),`**
This command is used to find a pattern and replace it in a string. In this function also we can use **`ignore.case`** argument to consider upper and lower case strings as same

```r
fr="Data is an interesting field to work with"
gsub("data","Data Science",fr,ignore.case=T)
```

```
[1] "Data Science is an interesting field to work with"
```

```r
gsub("data ","Data Science",fr)
```

```
[1] "Data is an interesting field to work with"
```

## Extracting word using its position
**`stringr::word()`**
We can extract a part of sentence by position of words in it using above function


```r
sentences1 <- sentences[c(12,15,66)];sentences1
```

```
[1] "A rod is used to catch pink salmon."    
[2] "Help the woman get back to her feet."   
[3] "Cars and busses stalled in snow drifts."
```

```r
#Extracting words from first word to last word
word(sentences1,start= 1,end=-1,sep=" ")
```

```
[1] "A rod is used to catch pink salmon."    
[2] "Help the woman get back to her feet."   
[3] "Cars and busses stalled in snow drifts."
```

```r
#Extracting till last word starting from first word for first element second word from second element and so on 
word(sentences1,start=1:3,end=-1,sep=" ")
```

```
[1] "A rod is used to catch pink salmon." "the woman get back to her feet."    
[3] "busses stalled in snow drifts."     
```

We can use a wide range of separators based on scenario to extract words

```r
statement="To, improve, is, to, change, so, to, be, perfect, is, to, change, often"
word(statement,4,sep=fixed(','))
```

```
[1] " to"
```


# Conclusion
In this notes we have seen some of the operations that can be performed with String data, some of them are from stringr package and others are from base package. Most of these operations may be required at the time of data cleaning and also at the time of analysis.   


