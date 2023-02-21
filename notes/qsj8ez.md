> import streamlit as st import streamlit as st import snscrape.modules.twitter as sntwitter import pandas as pd st.title('Scraping Twitter') # Input query and number of tweets to scrape query = st.text\_input('Enter a search query:', 'data science') num\_tweets = st.number\_input( 'Number of tweets to scrape:', min\_value=1, max\_value=1000000, step=1) # Input since and until dates since\_date = st.date\_input("Since Date", value=None, min\_value=None, max\_value=None, key=None) until\_date = st.date\_input("Until Date", value=None, min\_value=None, max\_value=None, key=None) # Scrape tweets and store data in a dataframe tweets\_list = \[\] for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query + ' lang:id since:' + since\_date.strftime('%Y-%m-%d') + ' until:' + until\_date.strftime('%Y-%m-%d')).get\_items()): if i >= num\_tweets: break tweets\_list.append(\[tweet.id, tweet.date, tweet.content, tweet.user.username, tweet.user.followersCount, tweet.url, tweet.user.id\]) tweets\_df = pd.DataFrame(tweets\_list, columns=\[ 'Tweet Id', 'Datetime', 'Text', 'Username', 'Followers', 'URL', 'User Id'\]) # Display data st.write(tweets\_df) # Download as text file if st.button('Download as Text File'): t = '\\t'.join(tweets\_df.columns) + '\\n' for index, row in tweets\_df.iterrows(): t += '\\t'.join(\[str(elem) for elem in row.values\]) + '\\n' with open('tweets.txt', 'w') as f: f.write(t) st.download\_button( label="Download Tweets as Text", data=t, file\_name='tweets.txt', mime='text/plain' ) # Download as csv file if st.button('Download as CSV File'): csv = tweets\_df.to\_csv(index=False) with open('tweets.csv', 'w') as f: f.write(csv) st.download\_button( label="Download Tweets as CSV", data=csv, file\_name='tweets.csv', mime='text/csv' ) Tambahkan script tersebut dengan datavisualization
> 
> To add data visualization to the existing script, you can use various libraries such as `matplotlib`, `seaborn`, or `plotly`. Here's an example using `plotly` to create a simple bar chart showing the number of tweets per user:
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> import plotly.express as px
> 
> st.title('Scraping Twitter')
> 
> # Input query and number of tweets to scrape
> query = st.text_input('Enter a search query:', 'data science')
> num_tweets = st.number_input(
>     'Number of tweets to scrape:', min_value=1, max_value=1000000, step=1)
> 
> # Input since and until dates
> since_date = st.date_input("Since Date", value=None,
>                            min_value=None, max_value=None, key=None)
> until_date = st.date_input("Until Date", value=None,
>                            min_value=None, max_value=None, key=None)
> 
> # Scrape tweets and store data in a dataframe
> tweets_list = []
> for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query + ' lang:id since:' + since_date.strftime('%Y-%m-%d') + ' until:' + until_date.strftime('%Y-%m-%d')).get_items()):
>     if i >= num_tweets:
>         break
>     tweets_list.append([tweet.id, tweet.date, tweet.content, tweet.user.username,
>                        tweet.user.followersCount, tweet.url, tweet.user.id])
> tweets_df = pd.DataFrame(tweets_list, columns=[
>                          'Tweet Id', 'Datetime', 'Text', 'Username', 'Followers', 'URL', 'User Id'])
> 
> # Display data
> st.write(tweets_df)
> 
> # Create bar chart of number of tweets per user
> if not tweets_df.empty:
>     tweet_counts = tweets_df['Username'].value_counts()
>     fig = px.bar(tweet_counts, x=tweet_counts.index, y=tweet_counts.values)
>     fig.update_layout(title="Number of Tweets per User")
>     st.plotly_chart(fig)
> 
> # Download as text file
> if st.button('Download as Text File'):
>     t = '\t'.join(tweets_df.columns) + '\n'
>     for index, row in tweets_df.iterrows():
>         t += '\t'.join([str(elem) for elem in row.values]) + '\n'
>     with open('tweets.txt', 'w') as f:
>         f.write(t)
>     st.download_button(
>         label="Download Tweets as Text",
>         data=t,
>         file_name='tweets.txt',
>         mime='text/plain'
>     )
> 
> # Download as csv file
> if st.button('Download as CSV File'):
>     csv = tweets_df.to_csv(index=False)
>     with open('tweets.csv', 'w') as f:
>         f.write(csv)
>     st.download_button(
>         label="Download Tweets as CSV",
>         data=csv,
>         file_name='tweets.csv',
>         mime='text/csv'
>     )
> ```
> 
> In the code above, we first import `plotly.express` and create a bar chart using the `px.bar` function. We pass in the tweet counts as the x and y values, and set the chart title using the `update_layout` function. We then use `st.plotly_chart` to display the chart in the Streamlit app.
> 
> Note that we also added a check to make sure the `tweets_df` dataframe is not empty before creating the chart. This is to avoid errors if no tweets are found for the given search query and date range.
> 
> mengatasi No module named 'plotly'
> 
> Jika Anda mendapat pesan kesalahan "No module named 'plotly'", itu berarti Anda belum menginstal Plotly, yang merupakan pustaka visualisasi data interaktif. Anda dapat menginstal Plotly dengan menjalankan perintah berikut di terminal atau command prompt:
> 
> `pip install plotly`
> 
> Setelah menginstal Plotly, pastikan untuk mengimpor pustaka di script Anda dengan menambahkan baris berikut:
> 
> python
> 
> ```python
> import plotly.express as px
> ```
> 
> Anda juga dapat menggunakan pustaka visualisasi data lainnya seperti Matplotlib atau Seaborn jika Anda tidak ingin menggunakan Plotly. Pastikan untuk menginstal dan mengimpor pustaka yang sesuai sebelum menggunakannya dalam script Anda.