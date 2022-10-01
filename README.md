# crawling-twitter
# crawling twitter menggunakan twitter kode API dari tweeter developer
# harap diketik manual untuk mengindari kesalahan indentation

# pertama import dulu library berikut. kalau belum install bisa dengan pip install di cmd
import tweepy
import csv
import pandas as pd

# access token, access token secret, api key, api key secret kode diperoleh dari tweeter developer
access_token =""
access_token_secret =""
api_key =""
api_key_secret =""

# search key adalah keyword dari pecarian tweet yang hendak di crawling
auth = tweepy.OAuthHandler(api_key,api_key_secret)
auth.set_access_token(access_token,access_token_secret)
api = tweepy.API(auth,wait_on_rate_limit=True)
search_key = ""

csvFile = open(search_key+".csv","+a",newline="",encoding="utf-8")
csvWriter = csv.writer(csvFile)
create = []
userid = []
username =[]
teks =[]

for tweet in tweepy.Cursor(api.search_tweets,q=search_key,count=1000,lang="id",since="2022-09-26",until="2022-09-28").items():
    print(tweet.created_at,tweet.id,tweet.user.name,tweet.text)
    create.append(tweet.created_at)
    userid.append(tweet.id)
    username.append(tweet.user.name)
    teks.append(tweet.text.encode("utf-8"))
    tweets = [tweet.created_at,tweet.id,tweet.user.name,tweet.text.encode("utf-8")]
    csvWriter.writerow(tweets)
    
dictTweets = {"waktu":create, "id":userid, "username":username, "text":teks}
df = pd.DataFrame(dictTweets,columns=["waktu","id","username","text"])
df
