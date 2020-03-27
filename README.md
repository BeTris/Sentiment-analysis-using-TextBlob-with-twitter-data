# Sentiment-analysis-using-TextBlob-with-twitter-data
from textblob import TextBlob
import sys,tweepy
from tweepy import OAuthHandler
import matplotlib.pyplot as plt
def percentage(part,whole):
    return(100*(float(part)/float(whole)))
ckey='YOUR CONSUMER KEY HERE'
csecret='YOUR CONSUMER SECRET KEY HERE'
atoken='YOUR ACCESS TOKEN HERE'
asecret='YOUR ACCESS SECRET KEY HERE'
auth=OAuthHandler(ckey,csecret)
auth.set_access_token(atoken,asecret)
api=tweepy.API(auth)
searchTerm=input("enter keyword/hashtag to search about:")
noOfSearchTerms=int(input("Enter how many tweets to analyse: "))
tweets=tweepy.Cursor(api.search,q=searchTerm,lang="en").items(noOfSearchTerms)
positive=0
negative=0
neutral=0
polarity=0
for tweet in tweets:
    analysis=TextBlob(tweet.text)
    polarity +=analysis.sentiment.polarity
    if(analysis.sentiment.polarity==0):
        neutral+=1
    elif(analysis.sentiment.polarity>0):
        positive+=1
    elif(analysis.sentiment.polarity<0):
        negative+=1
positive=percentage(positive,noOfSearchTerms)
negative=percentage(negative,noOfSearchTerms)
neutral=percentage(neutral,noOfSearchTerms)
polarity=float(polarity)/float(noOfSearchTerms)
positive=float(format(positive,".2f"))
negative=float(format(negative,".2f"))
neutral=float(format(neutral,".2f"))

print("How people are reacting on "+searchTerm+" by analysing "+str(noOfSearchTerms)+" Tweets.\n")
if(polarity ==0.00):
    print("neutral\n")
elif(polarity<0.00):
    print("negative\n")
elif(polarity >0.00):
    print("positive\n")
labels=['Positive ['+str(positive)+'%]','Neutral['+str(neutral)+'%]','Negative['+str(negative)+'%]']
sizes=[positive,neutral,negative]
colors=['pink','magenta','violet']
patches,text=plt.pie(sizes,colors=colors,startangle=90)
plt.legend(patches,labels,loc='best')
plt.title("How people are reacting on "+searchTerm+" by analysing "+str(noOfSearchTerms)+' Tweets.')
plt.axis('equal')
plt.tight_layout()
plt.show()
