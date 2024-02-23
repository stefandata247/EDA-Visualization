### Import Libraries


```python
import plotly.graph_objects as go
from plotly.offline import init_notebook_mode, iplot
import plotly.express as px
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np # linear algebra


df = pd.read_csv("netflix_titles_nov_2019.csv")

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>show_id</th>
      <th>title</th>
      <th>director</th>
      <th>cast</th>
      <th>country</th>
      <th>date_added</th>
      <th>release_year</th>
      <th>rating</th>
      <th>duration</th>
      <th>listed_in</th>
      <th>description</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>81193313</td>
      <td>Chocolate</td>
      <td>NaN</td>
      <td>Ha Ji-won, Yoon Kye-sang, Jang Seung-jo, Kang ...</td>
      <td>South Korea</td>
      <td>November 30, 2019</td>
      <td>2019</td>
      <td>TV-14</td>
      <td>1 Season</td>
      <td>International TV Shows, Korean TV Shows, Roman...</td>
      <td>Brought together by meaningful meals in the pa...</td>
      <td>TV Show</td>
    </tr>
    <tr>
      <th>1</th>
      <td>81197050</td>
      <td>Guatemala: Heart of the Mayan World</td>
      <td>Luis Ara, Ignacio Jaunsolo</td>
      <td>Christian Morales</td>
      <td>NaN</td>
      <td>November 30, 2019</td>
      <td>2019</td>
      <td>TV-G</td>
      <td>67 min</td>
      <td>Documentaries, International Movies</td>
      <td>From Sierra de las Minas to Esquipulas, explor...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>2</th>
      <td>81213894</td>
      <td>The Zoya Factor</td>
      <td>Abhishek Sharma</td>
      <td>Sonam Kapoor, Dulquer Salmaan, Sanjay Kapoor, ...</td>
      <td>India</td>
      <td>November 30, 2019</td>
      <td>2019</td>
      <td>TV-14</td>
      <td>135 min</td>
      <td>Comedies, Dramas, International Movies</td>
      <td>A goofy copywriter unwittingly convinces the I...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>3</th>
      <td>81082007</td>
      <td>Atlantics</td>
      <td>Mati Diop</td>
      <td>Mama Sane, Amadou Mbow, Ibrahima Traore, Nicol...</td>
      <td>France, Senegal, Belgium</td>
      <td>November 29, 2019</td>
      <td>2019</td>
      <td>TV-14</td>
      <td>106 min</td>
      <td>Dramas, Independent Movies, International Movies</td>
      <td>Arranged to marry a rich man, young Ada is cru...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>4</th>
      <td>80213643</td>
      <td>Chip and Potato</td>
      <td>NaN</td>
      <td>Abigail Oliver, Andrea Libman, Briana Buckmast...</td>
      <td>Canada, United Kingdom</td>
      <td>NaN</td>
      <td>2019</td>
      <td>TV-Y</td>
      <td>2 Seasons</td>
      <td>Kids' TV</td>
      <td>Lovable pug Chip starts kindergarten, makes ne...</td>
      <td>TV Show</td>
    </tr>
  </tbody>
</table>
</div>



Shape of the dataset


```python
df.shape
```




    (5837, 12)



Name of the columns


```python
df.columns
```




    Index(['show_id', 'title', 'director', 'cast', 'country', 'date_added',
           'release_year', 'rating', 'duration', 'listed_in', 'description',
           'type'],
          dtype='object')



Check for NULL Values


```python
df.isnull().sum()
```




    show_id            0
    title              0
    director        1901
    cast             556
    country          427
    date_added       642
    release_year       0
    rating            10
    duration           0
    listed_in          0
    description        0
    type               0
    dtype: int64



Check for unique values


```python
df.nunique()
```




    show_id         5837
    title           5780
    director        3108
    cast            5087
    country          527
    date_added      1092
    release_year      71
    rating            14
    duration         194
    listed_in        449
    description     5829
    type               2
    dtype: int64



Check for Duplicate values


```python
df.duplicated().sum()
```




    0



Make a copy of the dataset¶


```python
df = df.copy()
```


```python
df.shape
```




    (5837, 12)




```python
Drop NULL values
```


```python
df=df.dropna()
df.shape
```




    (3447, 12)




```python
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>show_id</th>
      <th>title</th>
      <th>director</th>
      <th>cast</th>
      <th>country</th>
      <th>date_added</th>
      <th>release_year</th>
      <th>rating</th>
      <th>duration</th>
      <th>listed_in</th>
      <th>description</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>81213894</td>
      <td>The Zoya Factor</td>
      <td>Abhishek Sharma</td>
      <td>Sonam Kapoor, Dulquer Salmaan, Sanjay Kapoor, ...</td>
      <td>India</td>
      <td>November 30, 2019</td>
      <td>2019</td>
      <td>TV-14</td>
      <td>135 min</td>
      <td>Comedies, Dramas, International Movies</td>
      <td>A goofy copywriter unwittingly convinces the I...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>3</th>
      <td>81082007</td>
      <td>Atlantics</td>
      <td>Mati Diop</td>
      <td>Mama Sane, Amadou Mbow, Ibrahima Traore, Nicol...</td>
      <td>France, Senegal, Belgium</td>
      <td>November 29, 2019</td>
      <td>2019</td>
      <td>TV-14</td>
      <td>106 min</td>
      <td>Dramas, Independent Movies, International Movies</td>
      <td>Arranged to marry a rich man, young Ada is cru...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>5</th>
      <td>81172754</td>
      <td>Crazy people</td>
      <td>Moses Inwang</td>
      <td>Ramsey Nouah, Chigul, Sola Sobowale, Ireti Doy...</td>
      <td>Nigeria</td>
      <td>November 29, 2019</td>
      <td>2018</td>
      <td>TV-14</td>
      <td>107 min</td>
      <td>Comedies, International Movies, Thrillers</td>
      <td>Nollywood star Ramsey Nouah learns that someon...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>6</th>
      <td>81120982</td>
      <td>I Lost My Body</td>
      <td>Jérémy Clapin</td>
      <td>Hakim Faris, Victoire Du Bois, Patrick d'Assum...</td>
      <td>France</td>
      <td>November 29, 2019</td>
      <td>2019</td>
      <td>TV-MA</td>
      <td>81 min</td>
      <td>Dramas, Independent Movies, International Movies</td>
      <td>Romance, mystery and adventure intertwine as a...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>7</th>
      <td>81227195</td>
      <td>Kalushi: The Story of Solomon Mahlangu</td>
      <td>Mandla Dube</td>
      <td>Thabo Rametsi, Thabo Malema, Welile Nzuza, Jaf...</td>
      <td>South Africa</td>
      <td>November 29, 2019</td>
      <td>2016</td>
      <td>TV-MA</td>
      <td>107 min</td>
      <td>Dramas, International Movies</td>
      <td>The life and times of iconic South African lib...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>10</th>
      <td>81172899</td>
      <td>Payday</td>
      <td>Cheta Chukwu</td>
      <td>Baaj Adebule, Ebiye Victor, Meg Otanwa, Bisola...</td>
      <td>Nigeria</td>
      <td>November 29, 2019</td>
      <td>2018</td>
      <td>TV-MA</td>
      <td>110 min</td>
      <td>Comedies, Independent Movies, International Mo...</td>
      <td>After an expensive night out, two flatmates ge...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>12</th>
      <td>81172908</td>
      <td>The Accidental Spy</td>
      <td>Roger Russell</td>
      <td>Ramsey Nouah, Christine Allado, Ayo Makun, Emm...</td>
      <td>Nigeria</td>
      <td>November 29, 2019</td>
      <td>2017</td>
      <td>TV-14</td>
      <td>104 min</td>
      <td>Action &amp; Adventure, Comedies, International Mo...</td>
      <td>Nursing a broken heart, an IT specialist moves...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>14</th>
      <td>81172901</td>
      <td>The Island</td>
      <td>Toka McBaror</td>
      <td>Sambasa Nzeribe, Segun Arinze, Tokunbo Idowu, ...</td>
      <td>Nigeria</td>
      <td>November 29, 2019</td>
      <td>2018</td>
      <td>TV-14</td>
      <td>93 min</td>
      <td>Dramas, International Movies, Thrillers</td>
      <td>When a colonel uncovers controversial intel ab...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>16</th>
      <td>81033086</td>
      <td>Holiday Rush</td>
      <td>Leslie Small</td>
      <td>Romany Malco, Sonequa Martin-Green, Darlene Lo...</td>
      <td>United States</td>
      <td>November 28, 2019</td>
      <td>2019</td>
      <td>TV-PG</td>
      <td>94 min</td>
      <td>Children &amp; Family Movies, Dramas</td>
      <td>A widowed radio DJ and his four spoiled kids n...</td>
      <td>Movie</td>
    </tr>
    <tr>
      <th>21</th>
      <td>60020826</td>
      <td>The Score</td>
      <td>Frank Oz</td>
      <td>Robert De Niro, Edward Norton, Marlon Brando, ...</td>
      <td>Germany, Canada, United States</td>
      <td>November 28, 2019</td>
      <td>2001</td>
      <td>R</td>
      <td>124 min</td>
      <td>Dramas, Thrillers</td>
      <td>Ready-to-retire safecracker Nick, flamboyant f...</td>
      <td>Movie</td>
    </tr>
  </tbody>
</table>
</div>



Convert Date Time format¶



```python
## add new features in the dataset
df["date_added"] = pd.to_datetime(df['date_added'])
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month

df['season_count'] = df.apply(lambda x : x['duration'].split(" ")[0] if "Season" in x['duration'] else "", axis = 1)
df['duration'] = df.apply(lambda x : x['duration'].split(" ")[0] if "Season" not in x['duration'] else "", axis = 1)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>show_id</th>
      <th>title</th>
      <th>director</th>
      <th>cast</th>
      <th>country</th>
      <th>date_added</th>
      <th>release_year</th>
      <th>rating</th>
      <th>duration</th>
      <th>listed_in</th>
      <th>description</th>
      <th>type</th>
      <th>year_added</th>
      <th>month_added</th>
      <th>season_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>81213894</td>
      <td>The Zoya Factor</td>
      <td>Abhishek Sharma</td>
      <td>Sonam Kapoor, Dulquer Salmaan, Sanjay Kapoor, ...</td>
      <td>India</td>
      <td>2019-11-30</td>
      <td>2019</td>
      <td>TV-14</td>
      <td>135</td>
      <td>Comedies, Dramas, International Movies</td>
      <td>A goofy copywriter unwittingly convinces the I...</td>
      <td>Movie</td>
      <td>2019</td>
      <td>11</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>81082007</td>
      <td>Atlantics</td>
      <td>Mati Diop</td>
      <td>Mama Sane, Amadou Mbow, Ibrahima Traore, Nicol...</td>
      <td>France, Senegal, Belgium</td>
      <td>2019-11-29</td>
      <td>2019</td>
      <td>TV-14</td>
      <td>106</td>
      <td>Dramas, Independent Movies, International Movies</td>
      <td>Arranged to marry a rich man, young Ada is cru...</td>
      <td>Movie</td>
      <td>2019</td>
      <td>11</td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td>81172754</td>
      <td>Crazy people</td>
      <td>Moses Inwang</td>
      <td>Ramsey Nouah, Chigul, Sola Sobowale, Ireti Doy...</td>
      <td>Nigeria</td>
      <td>2019-11-29</td>
      <td>2018</td>
      <td>TV-14</td>
      <td>107</td>
      <td>Comedies, International Movies, Thrillers</td>
      <td>Nollywood star Ramsey Nouah learns that someon...</td>
      <td>Movie</td>
      <td>2019</td>
      <td>11</td>
      <td></td>
    </tr>
    <tr>
      <th>6</th>
      <td>81120982</td>
      <td>I Lost My Body</td>
      <td>Jérémy Clapin</td>
      <td>Hakim Faris, Victoire Du Bois, Patrick d'Assum...</td>
      <td>France</td>
      <td>2019-11-29</td>
      <td>2019</td>
      <td>TV-MA</td>
      <td>81</td>
      <td>Dramas, Independent Movies, International Movies</td>
      <td>Romance, mystery and adventure intertwine as a...</td>
      <td>Movie</td>
      <td>2019</td>
      <td>11</td>
      <td></td>
    </tr>
    <tr>
      <th>7</th>
      <td>81227195</td>
      <td>Kalushi: The Story of Solomon Mahlangu</td>
      <td>Mandla Dube</td>
      <td>Thabo Rametsi, Thabo Malema, Welile Nzuza, Jaf...</td>
      <td>South Africa</td>
      <td>2019-11-29</td>
      <td>2016</td>
      <td>TV-MA</td>
      <td>107</td>
      <td>Dramas, International Movies</td>
      <td>The life and times of iconic South African lib...</td>
      <td>Movie</td>
      <td>2019</td>
      <td>11</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



### Data Visualization

Plot Movie and TV Shows


```python
# count plot on single categorical variable
sns.countplot(x ='type', data = df)
fig = plt.gcf()
fig.set_size_inches(10,10)
plt.title('Type')
 
# Show the plot
plt.show()
```


    
![png](output_22_0.png)
    


Rating of TV Shows and Movies


```python
ax = sns.countplot(x=df['rating'],
                   order=df['rating'].value_counts(ascending=False).index);

abs_values = df['rating'].value_counts(ascending=False).values

ax.bar_label(container=ax.containers[0], labels=abs_values)
plt.title('Rating')
plt.show()
```


    
![png](output_24_0.png)
    


Relation between Type and Rating


```python
sns.countplot(x ='rating', hue = "type", data = df)
plt.title('Relation between Type and Rating')
 
# Show the plot
plt.show()
```


    
![png](output_26_0.png)
    


Pie-chart for the Type: Movie and TV Shows¶

Pie-chart for Rating¶


```python
df['rating'].value_counts().plot.pie(autopct='%1.1f%%',shadow=True,figsize=(10,8))
plt.title('Rating')
plt.show()
```


    
![png](output_29_0.png)
    


Sort Content by old movies on Netflix


```python
small = df.sort_values("release_year", ascending = True)
small = small[small['duration'] != ""]
small[['title', "release_year"]][:15]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>release_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4931</th>
      <td>The Battle of Midway</td>
      <td>1942</td>
    </tr>
    <tr>
      <th>4940</th>
      <td>Tunisian Victory</td>
      <td>1944</td>
    </tr>
    <tr>
      <th>4923</th>
      <td>Know Your Enemy - Japan</td>
      <td>1945</td>
    </tr>
    <tr>
      <th>3085</th>
      <td>The Stranger</td>
      <td>1946</td>
    </tr>
    <tr>
      <th>4924</th>
      <td>Let There Be Light</td>
      <td>1946</td>
    </tr>
    <tr>
      <th>4939</th>
      <td>Thunderbolt</td>
      <td>1947</td>
    </tr>
    <tr>
      <th>5817</th>
      <td>White Christmas</td>
      <td>1954</td>
    </tr>
    <tr>
      <th>257</th>
      <td>Rebel Without a Cause</td>
      <td>1955</td>
    </tr>
    <tr>
      <th>220</th>
      <td>Forbidden Planet</td>
      <td>1956</td>
    </tr>
    <tr>
      <th>204</th>
      <td>Cat on a Hot Tin Roof</td>
      <td>1958</td>
    </tr>
    <tr>
      <th>222</th>
      <td>Gigi</td>
      <td>1958</td>
    </tr>
    <tr>
      <th>4199</th>
      <td>Ujala</td>
      <td>1959</td>
    </tr>
    <tr>
      <th>4195</th>
      <td>Singapore</td>
      <td>1960</td>
    </tr>
    <tr>
      <th>203</th>
      <td>Butterfield 8</td>
      <td>1960</td>
    </tr>
    <tr>
      <th>254</th>
      <td>Ocean's Eleven</td>
      <td>1960</td>
    </tr>
  </tbody>
</table>
</div>



Sort Content by TV Shows on Netflix


```python
small = df.sort_values("release_year", ascending = True)
small = small[small['season_count'] != ""]
small[['title', "release_year"]][:15]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>release_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5073</th>
      <td>Ken Burns: The Civil War</td>
      <td>1990</td>
    </tr>
    <tr>
      <th>5721</th>
      <td>The Blue Planet: A Natural History of the Oceans</td>
      <td>2001</td>
    </tr>
    <tr>
      <th>5760</th>
      <td>Planet Earth: The Complete Collection</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>4905</th>
      <td>Ouran High School Host Club</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>3925</th>
      <td>Geronimo Stilton</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>5639</th>
      <td>Frozen Planet</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>4933</th>
      <td>The Fear</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>777</th>
      <td>Reply 1997</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>3715</th>
      <td>Brave Miss World</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5341</th>
      <td>Oliver Stone's Untold History of the United St...</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5269</th>
      <td>Sadqay Tumhare</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>385</th>
      <td>Black Money Love</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>776</th>
      <td>Reply 1994</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5116</th>
      <td>Metallica: Some Kind of Monster</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5129</th>
      <td>Camelia la Texana</td>
      <td>2014</td>
    </tr>
  </tbody>
</table>
</div>



### Content from Different Countries


```python
country_codes = {'afghanistan': 'AFG',
 'albania': 'ALB',
 'algeria': 'DZA',
 'american samoa': 'ASM',
 'andorra': 'AND',
 'angola': 'AGO',
 'anguilla': 'AIA',
 'antigua and barbuda': 'ATG',
 'argentina': 'ARG',
 'armenia': 'ARM',
 'aruba': 'ABW',
 'australia': 'AUS',
 'austria': 'AUT',
 'azerbaijan': 'AZE',
 'bahamas': 'BHM',
 'bahrain': 'BHR',
 'bangladesh': 'BGD',
 'barbados': 'BRB',
 'belarus': 'BLR',
 'belgium': 'BEL',
 'belize': 'BLZ',
 'benin': 'BEN',
 'bermuda': 'BMU',
 'bhutan': 'BTN',
 'bolivia': 'BOL',
 'bosnia and herzegovina': 'BIH',
 'botswana': 'BWA',
 'brazil': 'BRA',
 'british virgin islands': 'VGB',
 'brunei': 'BRN',
 'bulgaria': 'BGR',
 'burkina faso': 'BFA',
 'burma': 'MMR',
 'burundi': 'BDI',
 'cabo verde': 'CPV',
 'cambodia': 'KHM',
 'cameroon': 'CMR',
 'canada': 'CAN',
 'cayman islands': 'CYM',
 'central african republic': 'CAF',
 'chad': 'TCD',
 'chile': 'CHL',
 'china': 'CHN',
 'colombia': 'COL',
 'comoros': 'COM',
 'congo democratic': 'COD',
 'Congo republic': 'COG',
 'cook islands': 'COK',
 'costa rica': 'CRI',
 "cote d'ivoire": 'CIV',
 'croatia': 'HRV',
 'cuba': 'CUB',
 'curacao': 'CUW',
 'cyprus': 'CYP',
 'czech republic': 'CZE',
 'denmark': 'DNK',
 'djibouti': 'DJI',
 'dominica': 'DMA',
 'dominican republic': 'DOM',
 'ecuador': 'ECU',
 'egypt': 'EGY',
 'el salvador': 'SLV',
 'equatorial guinea': 'GNQ',
 'eritrea': 'ERI',
 'estonia': 'EST',
 'ethiopia': 'ETH',
 'falkland islands': 'FLK',
 'faroe islands': 'FRO',
 'fiji': 'FJI',
 'finland': 'FIN',
 'france': 'FRA',
 'french polynesia': 'PYF',
 'gabon': 'GAB',
 'gambia, the': 'GMB',
 'georgia': 'GEO',
 'germany': 'DEU',
 'ghana': 'GHA',
 'gibraltar': 'GIB',
 'greece': 'GRC',
 'greenland': 'GRL',
 'grenada': 'GRD',
 'guam': 'GUM',
 'guatemala': 'GTM',
 'guernsey': 'GGY',
 'guinea-bissau': 'GNB',
 'guinea': 'GIN',
 'guyana': 'GUY',
 'haiti': 'HTI',
 'honduras': 'HND',
 'hong kong': 'HKG',
 'hungary': 'HUN',
 'iceland': 'ISL',
 'india': 'IND',
 'indonesia': 'IDN',
 'iran': 'IRN',
 'iraq': 'IRQ',
 'ireland': 'IRL',
 'isle of man': 'IMN',
 'israel': 'ISR',
 'italy': 'ITA',
 'jamaica': 'JAM',
 'japan': 'JPN',
 'jersey': 'JEY',
 'jordan': 'JOR',
 'kazakhstan': 'KAZ',
 'kenya': 'KEN',
 'kiribati': 'KIR',
 'north korea': 'PRK',
 'south korea': 'KOR',
 'kosovo': 'KSV',
 'kuwait': 'KWT',
 'kyrgyzstan': 'KGZ',
 'laos': 'LAO',
 'latvia': 'LVA',
 'lebanon': 'LBN',
 'lesotho': 'LSO',
 'liberia': 'LBR',
 'libya': 'LBY',
 'liechtenstein': 'LIE',
 'lithuania': 'LTU',
 'luxembourg': 'LUX',
 'macau': 'MAC',
 'macedonia': 'MKD',
 'madagascar': 'MDG',
 'malawi': 'MWI',
 'malaysia': 'MYS',
 'maldives': 'MDV',
 'mali': 'MLI',
 'malta': 'MLT',
 'marshall islands': 'MHL',
 'mauritania': 'MRT',
 'mauritius': 'MUS',
 'mexico': 'MEX',
 'micronesia': 'FSM',
 'moldova': 'MDA',
 'monaco': 'MCO',
 'mongolia': 'MNG',
 'montenegro': 'MNE',
 'morocco': 'MAR',
 'mozambique': 'MOZ',
 'namibia': 'NAM',
 'nepal': 'NPL',
 'netherlands': 'NLD',
 'new caledonia': 'NCL',
 'new zealand': 'NZL',
 'nicaragua': 'NIC',
 'nigeria': 'NGA',
 'niger': 'NER',
 'niue': 'NIU',
 'northern mariana islands': 'MNP',
 'norway': 'NOR',
 'oman': 'OMN',
 'pakistan': 'PAK',
 'palau': 'PLW',
 'panama': 'PAN',
 'papua new guinea': 'PNG',
 'paraguay': 'PRY',
 'peru': 'PER',
 'philippines': 'PHL',
 'poland': 'POL',
 'portugal': 'PRT',
 'puerto rico': 'PRI',
 'qatar': 'QAT',
 'romania': 'ROU',
 'russia': 'RUS',
 'rwanda': 'RWA',
 'saint kitts and nevis': 'KNA',
 'saint lucia': 'LCA',
 'saint martin': 'MAF',
 'saint pierre and miquelon': 'SPM',
 'saint vincent and the grenadines': 'VCT',
 'samoa': 'WSM',
 'san marino': 'SMR',
 'sao tome and principe': 'STP',
 'saudi arabia': 'SAU',
 'senegal': 'SEN',
 'serbia': 'SRB',
 'seychelles': 'SYC',
 'sierra leone': 'SLE',
 'singapore': 'SGP',
 'sint maarten': 'SXM',
 'slovakia': 'SVK',
 'slovenia': 'SVN',
 'solomon islands': 'SLB',
 'somalia': 'SOM',
 'south africa': 'ZAF',
 'south sudan': 'SSD',
 'spain': 'ESP',
 'sri lanka': 'LKA',
 'sudan': 'SDN',
 'suriname': 'SUR',
 'swaziland': 'SWZ',
 'sweden': 'SWE',
 'switzerland': 'CHE',
 'syria': 'SYR',
 'taiwan': 'TWN',
 'tajikistan': 'TJK',
 'tanzania': 'TZA',
 'thailand': 'THA',
 'timor-leste': 'TLS',
 'togo': 'TGO',
 'tonga': 'TON',
 'trinidad and tobago': 'TTO',
 'tunisia': 'TUN',
 'turkey': 'TUR',
 'turkmenistan': 'TKM',
 'tuvalu': 'TUV',
 'uganda': 'UGA',
 'ukraine': 'UKR',
 'united arab emirates': 'ARE',
 'united kingdom': 'GBR',
 'united states': 'USA',
 'uruguay': 'URY',
 'uzbekistan': 'UZB',
 'vanuatu': 'VUT',
 'venezuela': 'VEN',
 'vietnam': 'VNM',
 'virgin islands': 'VGB',
 'west bank': 'WBG',
 'yemen': 'YEM',
 'zambia': 'ZMB',
 'zimbabwe': 'ZWE'}

## countries 
from collections import Counter
colorscale = ["#f7fbff", "#ebf3fb", "#deebf7", "#d2e3f3", "#c6dbef", "#b3d2e9", "#9ecae1",
    "#85bcdb", "#6baed6", "#57a0ce", "#4292c6", "#3082be", "#2171b5", "#1361a9",
    "#08519c", "#0b4083", "#08306b"
]
    
def geoplot(ddf):
    country_with_code, country = {}, {}
    shows_countries = ", ".join(ddf['country'].dropna()).split(", ")
    for c,v in dict(Counter(shows_countries)).items():
        code = ""
        if c.lower() in country_codes:
            code = country_codes[c.lower()]
        country_with_code[code] = v
        country[c] = v

    data = [dict(
            type = 'choropleth',
            locations = list(country_with_code.keys()),
            z = list(country_with_code.values()),
            colorscale = [[0,"rgb(5, 10, 172)"],[0.65,"rgb(40, 60, 190)"],[0.75,"rgb(70, 100, 245)"],\
                        [0.80,"rgb(90, 120, 245)"],[0.9,"rgb(106, 137, 247)"],[1,"rgb(220, 220, 220)"]],
            autocolorscale = False,
            reversescale = True,
            marker = dict(
                line = dict (
                    color = 'gray',
                    width = 0.5
                ) ),
            colorbar = dict(
                autotick = False,
                title = ''),
          ) ]

    layout = dict(
        title = '',
        geo = dict(
            showframe = False,
            showcoastlines = False,
            projection = dict(
                type = 'Mercator'
            )
        )
    )

    fig = dict( data=data, layout=layout )
    iplot( fig, validate=False, filename='d3-world-map' )
    return country

country_vals = geoplot(df)
tabs = Counter(country_vals).most_common(25)

labels = [_[0] for _ in tabs][::-1]
values = [_[1] for _ in tabs][::-1]
trace1 = go.Bar(y=labels, x=values, orientation="h", name="", marker=dict(color="#a678de"))

data = [trace1]
layout = go.Layout(title="Countries with most content", height=700, legend=dict(x=0.1, y=1.1, orientation="h"))
fig = go.Figure(data, layout=layout)
fig.show()
```


<div>                            <div id="36c3e403-de3b-439c-95c1-835760590c2e" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("36c3e403-de3b-439c-95c1-835760590c2e")) {                    Plotly.newPlot(                        "36c3e403-de3b-439c-95c1-835760590c2e",                        [{"type":"choropleth","locations":["IND","FRA","SEN","BEL","NGA","ZAF","USA","DEU","CAN","NOR","ARE","JPN","GBR","AUS","MEX","ITA","NZL","ESP","CHL","PER","COL","ARG","IRL","CHN","HKG","KOR","LIE","TWN","ISR","TUR","CHE","SGP","MYS","LUX","EGY","AUT","IDN","POL","DNK","NLD","","MLT","VNM","PAK","BRA","PRT","SWE","PHL","BGR","CZE","THA","DOM","URY","ROU","MAR","GHA","FIN","IRN","KHM","MWI","PRY","RUS","BGD","NPL","ISL","GRC","IRQ","SRB","CYM","HRV","ALB","QAT","JOR","HUN","GTM","SOM","KEN","SDN","SVN","NIC","GEO","VEN","SAU","LBN","PAN","LVA","SVK","LKA","AFG","MNE","BMU","ECU"],"z":[677,177,1,52,26,20,1538,97,176,11,20,64,297,63,72,35,11,116,19,7,12,48,21,79,90,46,1,10,14,65,13,16,13,7,43,6,47,19,23,25,3,1,4,20,37,4,18,44,7,9,36,2,4,5,5,1,2,4,3,1,1,5,2,2,5,4,1,4,1,3,1,6,4,3,1,1,1,1,3,1,1,1,1,2,1,1,1,1,1,1,1,1],"colorscale":[[0,"rgb(5, 10, 172)"],[0.65,"rgb(40, 60, 190)"],[0.75,"rgb(70, 100, 245)"],[0.8,"rgb(90, 120, 245)"],[0.9,"rgb(106, 137, 247)"],[1,"rgb(220, 220, 220)"]],"autocolorscale":false,"reversescale":true,"marker":{"line":{"color":"gray","width":0.5}},"colorbar":{"autotick":false,"title":""}}],                        {"title":"","geo":{"showframe":false,"showcoastlines":false,"projection":{"type":"Mercator"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('36c3e403-de3b-439c-95c1-835760590c2e');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="61ac049e-9748-4f1f-8a1e-b0088cc98c66" class="plotly-graph-div" style="height:700px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("61ac049e-9748-4f1f-8a1e-b0088cc98c66")) {                    Plotly.newPlot(                        "61ac049e-9748-4f1f-8a1e-b0088cc98c66",                        [{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[23,25,26,35,36,37,43,44,46,47,48,52,63,64,65,72,79,90,97,116,176,177,297,677,1538],"y":["Denmark","Netherlands","Nigeria","Italy","Thailand","Brazil","Egypt","Philippines","South Korea","Indonesia","Argentina","Belgium","Australia","Japan","Turkey","Mexico","China","Hong Kong","Germany","Spain","Canada","France","United Kingdom","India","United States"],"type":"bar"}],                        {"height":700,"legend":{"orientation":"h","x":0.1,"y":1.1},"title":{"text":"Countries with most content"},"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('61ac049e-9748-4f1f-8a1e-b0088cc98c66');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### Distribution of Movie Duration


```python
import plotly.figure_factory as ff
x1 = d2['duration'].fillna(0.0).astype(float)
fig = ff.create_distplot([x1], ['a'], bin_size=0.7, curve_type='normal', colors=["#6ad49b"])
fig.update_layout(title_text='Distplot with Normal Distribution')
fig.show()
```


<div>                            <div id="cd243f7b-88db-407d-a601-727d8d4c7295" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("cd243f7b-88db-407d-a601-727d8d4c7295")) {                    Plotly.newPlot(                        "cd243f7b-88db-407d-a601-727d8d4c7295",                        [{"autobinx":false,"histnorm":"probability density","legendgroup":"a","marker":{"color":"#6ad49b"},"name":"a","opacity":0.7,"x":[135.0,106.0,107.0,81.0,107.0,110.0,104.0,93.0,94.0,124.0,137.0,134.0,106.0,209.0,86.0,24.0,117.0,92.0,114.0,121.0,94.0,109.0,110.0,96.0,97.0,121.0,119.0,138.0,93.0,111.0,88.0,81.0,73.0,86.0,116.0,85.0,134.0,109.0,102.0,101.0,88.0,101.0,28.0,103.0,131.0,166.0,105.0,82.0,107.0,84.0,103.0,112.0,89.0,121.0,87.0,136.0,118.0,129.0,94.0,88.0,158.0,78.0,100.0,84.0,107.0,101.0,107.0,74.0,60.0,143.0,105.0,110.0,98.0,100.0,74.0,46.0,88.0,54.0,59.0,100.0,95.0,100.0,93.0,94.0,121.0,78.0,116.0,61.0,98.0,123.0,85.0,92.0,87.0,92.0,112.0,97.0,78.0,103.0,107.0,68.0,68.0,99.0,69.0,81.0,69.0,68.0,68.0,91.0,86.0,40.0,102.0,119.0,94.0,119.0,90.0,105.0,108.0,108.0,87.0,112.0,101.0,89.0,91.0,200.0,119.0,90.0,103.0,110.0,133.0,124.0,118.0,98.0,85.0,115.0,137.0,98.0,110.0,92.0,116.0,94.0,134.0,102.0,98.0,100.0,105.0,118.0,153.0,109.0,99.0,96.0,91.0,185.0,92.0,127.0,98.0,111.0,120.0,137.0,121.0,89.0,91.0,135.0,98.0,139.0,95.0,98.0,91.0,122.0,100.0,103.0,129.0,133.0,105.0,107.0,136.0,138.0,129.0,141.0,99.0,89.0,90.0,98.0,88.0,65.0,126.0,63.0,20.0,88.0,98.0,94.0,106.0,118.0,86.0,94.0,82.0,92.0,95.0,52.0,108.0,91.0,96.0,67.0,83.0,94.0,97.0,91.0,98.0,92.0,118.0,87.0,121.0,98.0,95.0,107.0,100.0,101.0,96.0,99.0,112.0,127.0,140.0,63.0,97.0,86.0,93.0,62.0,96.0,104.0,123.0,100.0,99.0,151.0,102.0,106.0,87.0,70.0,111.0,101.0,109.0,110.0,102.0,102.0,24.0,98.0,93.0,77.0,103.0,45.0,119.0,147.0,123.0,99.0,98.0,106.0,112.0,85.0,91.0,88.0,101.0,90.0,96.0,99.0,101.0,111.0,96.0,86.0,97.0,121.0,90.0,88.0,64.0,104.0,122.0,125.0,92.0,129.0,79.0,121.0,106.0,124.0,87.0,125.0,154.0,86.0,111.0,97.0,66.0,121.0,91.0,91.0,82.0,117.0,81.0,84.0,81.0,107.0,94.0,163.0,90.0,96.0,91.0,93.0,64.0,89.0,102.0,116.0,133.0,108.0,97.0,89.0,146.0,146.0,130.0,106.0,58.0,152.0,87.0,121.0,182.0,106.0,102.0,98.0,83.0,82.0,111.0,171.0,84.0,112.0,80.0,88.0,124.0,104.0,76.0,91.0,64.0,94.0,86.0,157.0,83.0,95.0,106.0,87.0,90.0,65.0,105.0,107.0,103.0,88.0,96.0,67.0,140.0,93.0,90.0,125.0,133.0,128.0,88.0,108.0,93.0,96.0,102.0,108.0,102.0,93.0,87.0,94.0,100.0,88.0,109.0,138.0,149.0,82.0,86.0,112.0,98.0,113.0,47.0,106.0,113.0,119.0,109.0,98.0,88.0,116.0,106.0,95.0,95.0,113.0,91.0,102.0,100.0,81.0,98.0,110.0,106.0,85.0,66.0,92.0,92.0,91.0,133.0,135.0,120.0,118.0,123.0,98.0,98.0,98.0,104.0,167.0,95.0,117.0,100.0,86.0,112.0,69.0,119.0,123.0,93.0,107.0,46.0,91.0,103.0,117.0,76.0,67.0,114.0,67.0,77.0,84.0,63.0,99.0,59.0,102.0,81.0,145.0,102.0,116.0,164.0,87.0,90.0,106.0,116.0,134.0,133.0,118.0,120.0,154.0,128.0,115.0,177.0,105.0,112.0,120.0,119.0,100.0,92.0,104.0,97.0,88.0,128.0,130.0,108.0,98.0,89.0,90.0,130.0,99.0,107.0,94.0,111.0,100.0,77.0,57.0,107.0,93.0,103.0,153.0,94.0,149.0,111.0,58.0,56.0,110.0,98.0,106.0,102.0,93.0,81.0,77.0,98.0,97.0,136.0,161.0,32.0,107.0,98.0,83.0,77.0,133.0,105.0,79.0,106.0,90.0,87.0,95.0,65.0,131.0,105.0,119.0,107.0,112.0,97.0,106.0,66.0,106.0,110.0,102.0,105.0,112.0,96.0,97.0,113.0,53.0,54.0,53.0,54.0,113.0,53.0,53.0,52.0,54.0,54.0,53.0,53.0,53.0,91.0,126.0,99.0,134.0,26.0,117.0,89.0,94.0,101.0,94.0,99.0,114.0,48.0,118.0,119.0,104.0,98.0,93.0,90.0,122.0,113.0,141.0,81.0,176.0,119.0,73.0,87.0,88.0,15.0,55.0,122.0,117.0,95.0,63.0,73.0,107.0,99.0,96.0,113.0,69.0,99.0,87.0,112.0,116.0,99.0,101.0,116.0,83.0,110.0,110.0,99.0,59.0,126.0,97.0,89.0,88.0,91.0,113.0,117.0,90.0,95.0,98.0,102.0,61.0,93.0,117.0,108.0,126.0,93.0,60.0,142.0,113.0,90.0,107.0,74.0,119.0,114.0,125.0,71.0,110.0,127.0,118.0,90.0,94.0,90.0,98.0,99.0,133.0,94.0,116.0,93.0,103.0,84.0,90.0,107.0,89.0,96.0,130.0,90.0,86.0,79.0,93.0,12.0,62.0,94.0,93.0,114.0,110.0,125.0,119.0,109.0,100.0,98.0,98.0,128.0,105.0,109.0,83.0,101.0,78.0,110.0,116.0,141.0,120.0,97.0,109.0,105.0,102.0,127.0,101.0,135.0,124.0,103.0,105.0,98.0,93.0,113.0,106.0,99.0,123.0,91.0,84.0,99.0,122.0,101.0,99.0,91.0,137.0,111.0,66.0,159.0,125.0,95.0,95.0,96.0,90.0,53.0,87.0,90.0,96.0,150.0,105.0,112.0,119.0,142.0,94.0,97.0,137.0,100.0,125.0,128.0,108.0,136.0,165.0,124.0,133.0,116.0,102.0,87.0,70.0,92.0,88.0,100.0,111.0,128.0,98.0,104.0,110.0,87.0,126.0,103.0,111.0,100.0,112.0,99.0,125.0,147.0,88.0,79.0,110.0,128.0,110.0,79.0,87.0,133.0,116.0,119.0,53.0,82.0,99.0,87.0,90.0,124.0,53.0,88.0,104.0,92.0,126.0,140.0,87.0,93.0,80.0,92.0,64.0,72.0,148.0,110.0,119.0,163.0,125.0,87.0,96.0,91.0,122.0,125.0,95.0,99.0,98.0,90.0,97.0,97.0,86.0,114.0,96.0,99.0,88.0,168.0,89.0,90.0,100.0,102.0,98.0,92.0,116.0,111.0,91.0,99.0,99.0,98.0,73.0,113.0,153.0,81.0,54.0,93.0,123.0,96.0,101.0,53.0,102.0,141.0,111.0,131.0,116.0,119.0,146.0,101.0,82.0,90.0,87.0,95.0,94.0,129.0,94.0,103.0,133.0,135.0,85.0,91.0,105.0,59.0,91.0,94.0,89.0,131.0,170.0,91.0,128.0,88.0,97.0,110.0,170.0,92.0,113.0,126.0,86.0,120.0,76.0,60.0,118.0,88.0,55.0,110.0,113.0,110.0,94.0,54.0,53.0,103.0,126.0,89.0,124.0,128.0,88.0,83.0,96.0,118.0,91.0,91.0,53.0,103.0,108.0,88.0,81.0,124.0,102.0,98.0,132.0,89.0,109.0,75.0,126.0,61.0,126.0,108.0,89.0,100.0,129.0,108.0,101.0,105.0,115.0,118.0,130.0,101.0,115.0,99.0,97.0,61.0,97.0,101.0,92.0,123.0,108.0,104.0,98.0,65.0,88.0,106.0,69.0,97.0,80.0,85.0,99.0,98.0,115.0,138.0,90.0,86.0,90.0,117.0,132.0,127.0,125.0,128.0,126.0,87.0,98.0,116.0,90.0,111.0,100.0,123.0,106.0,100.0,104.0,99.0,105.0,128.0,91.0,127.0,100.0,131.0,104.0,94.0,124.0,106.0,108.0,103.0,100.0,79.0,92.0,116.0,83.0,118.0,162.0,162.0,126.0,109.0,109.0,111.0,89.0,97.0,103.0,88.0,120.0,90.0,99.0,83.0,107.0,109.0,105.0,87.0,93.0,90.0,89.0,131.0,114.0,92.0,75.0,107.0,97.0,90.0,98.0,100.0,103.0,66.0,106.0,125.0,77.0,130.0,119.0,110.0,120.0,102.0,125.0,103.0,127.0,121.0,101.0,113.0,118.0,132.0,106.0,116.0,117.0,90.0,61.0,90.0,107.0,80.0,58.0,58.0,56.0,110.0,70.0,117.0,89.0,84.0,131.0,91.0,103.0,100.0,139.0,103.0,42.0,79.0,90.0,88.0,96.0,114.0,89.0,103.0,127.0,113.0,111.0,112.0,103.0,104.0,102.0,86.0,97.0,111.0,132.0,89.0,102.0,120.0,137.0,82.0,85.0,115.0,107.0,130.0,57.0,62.0,112.0,102.0,93.0,111.0,93.0,91.0,118.0,63.0,74.0,136.0,90.0,132.0,101.0,59.0,84.0,91.0,93.0,91.0,101.0,110.0,82.0,100.0,52.0,127.0,86.0,107.0,22.0,22.0,91.0,99.0,105.0,105.0,88.0,128.0,113.0,50.0,92.0,110.0,120.0,123.0,141.0,97.0,118.0,95.0,127.0,118.0,74.0,137.0,86.0,125.0,114.0,143.0,121.0,119.0,108.0,150.0,88.0,105.0,97.0,96.0,91.0,49.0,96.0,96.0,90.0,97.0,94.0,88.0,118.0,86.0,127.0,88.0,125.0,116.0,102.0,65.0,106.0,117.0,93.0,136.0,116.0,121.0,89.0,104.0,91.0,99.0,91.0,117.0,135.0,135.0,91.0,95.0,95.0,82.0,124.0,86.0,115.0,99.0,101.0,142.0,102.0,95.0,120.0,96.0,92.0,110.0,133.0,143.0,111.0,144.0,121.0,99.0,93.0,100.0,102.0,123.0,127.0,116.0,119.0,105.0,134.0,90.0,93.0,93.0,108.0,115.0,119.0,61.0,109.0,97.0,154.0,120.0,148.0,121.0,95.0,102.0,87.0,114.0,48.0,124.0,101.0,28.0,61.0,56.0,81.0,50.0,49.0,72.0,100.0,30.0,146.0,127.0,46.0,57.0,90.0,86.0,122.0,97.0,109.0,122.0,96.0,91.0,110.0,115.0,150.0,90.0,89.0,93.0,105.0,105.0,124.0,44.0,91.0,95.0,78.0,22.0,95.0,82.0,89.0,90.0,94.0,85.0,29.0,96.0,137.0,27.0,112.0,112.0,107.0,153.0,123.0,90.0,84.0,87.0,145.0,135.0,109.0,86.0,107.0,110.0,86.0,85.0,75.0,90.0,105.0,124.0,110.0,96.0,110.0,105.0,98.0,111.0,116.0,106.0,109.0,88.0,155.0,119.0,122.0,87.0,98.0,130.0,99.0,105.0,110.0,81.0,115.0,113.0,146.0,118.0,122.0,109.0,117.0,95.0,124.0,96.0,96.0,123.0,85.0,93.0,30.0,82.0,95.0,127.0,120.0,114.0,119.0,101.0,96.0,120.0,139.0,95.0,44.0,104.0,110.0,91.0,91.0,114.0,91.0,64.0,89.0,95.0,86.0,94.0,133.0,122.0,86.0,111.0,132.0,111.0,121.0,76.0,92.0,119.0,45.0,75.0,71.0,81.0,115.0,128.0,95.0,114.0,99.0,132.0,106.0,122.0,79.0,78.0,111.0,112.0,107.0,102.0,81.0,129.0,86.0,116.0,95.0,122.0,98.0,103.0,117.0,95.0,146.0,136.0,91.0,123.0,144.0,103.0,126.0,121.0,91.0,158.0,157.0,124.0,106.0,92.0,80.0,94.0,126.0,129.0,133.0,109.0,143.0,140.0,150.0,128.0,120.0,154.0,45.0,132.0,126.0,95.0,91.0,135.0,139.0,131.0,118.0,129.0,131.0,146.0,123.0,141.0,93.0,91.0,58.0,91.0,112.0,119.0,106.0,131.0,104.0,126.0,118.0,116.0,122.0,105.0,90.0,87.0,102.0,89.0,94.0,126.0,89.0,90.0,115.0,93.0,138.0,74.0,102.0,103.0,119.0,104.0,161.0,51.0,86.0,140.0,122.0,95.0,205.0,121.0,131.0,80.0,63.0,150.0,159.0,131.0,130.0,99.0,106.0,97.0,118.0,98.0,144.0,110.0,96.0,57.0,98.0,119.0,92.0,94.0,151.0,74.0,89.0,124.0,84.0,121.0,85.0,25.0,121.0,88.0,111.0,81.0,137.0,56.0,80.0,94.0,94.0,72.0,140.0,91.0,97.0,146.0,118.0,133.0,106.0,128.0,137.0,145.0,106.0,61.0,137.0,129.0,99.0,155.0,142.0,214.0,137.0,128.0,89.0,67.0,151.0,121.0,101.0,145.0,147.0,90.0,129.0,129.0,136.0,117.0,150.0,134.0,100.0,44.0,44.0,111.0,147.0,115.0,121.0,103.0,73.0,129.0,102.0,122.0,155.0,139.0,131.0,127.0,156.0,132.0,130.0,137.0,132.0,156.0,132.0,119.0,140.0,101.0,126.0,107.0,112.0,111.0,126.0,130.0,126.0,127.0,108.0,163.0,95.0,103.0,110.0,111.0,91.0,101.0,124.0,102.0,112.0,103.0,92.0,93.0,112.0,113.0,92.0,153.0,111.0,141.0,128.0,117.0,100.0,112.0,110.0,107.0,109.0,100.0,114.0,98.0,100.0,123.0,86.0,112.0,106.0,106.0,102.0,86.0,146.0,122.0,126.0,116.0,135.0,125.0,96.0,130.0,135.0,88.0,96.0,122.0,110.0,104.0,96.0,109.0,95.0,110.0,110.0,79.0,101.0,112.0,94.0,94.0,86.0,84.0,95.0,113.0,111.0,135.0,95.0,93.0,101.0,90.0,112.0,141.0,120.0,140.0,99.0,86.0,104.0,100.0,50.0,124.0,31.0,90.0,196.0,93.0,110.0,162.0,139.0,130.0,145.0,88.0,91.0,91.0,102.0,95.0,119.0,114.0,100.0,54.0,150.0,108.0,90.0,101.0,104.0,88.0,93.0,79.0,92.0,90.0,86.0,90.0,103.0,86.0,108.0,97.0,92.0,101.0,83.0,85.0,85.0,106.0,86.0,131.0,105.0,134.0,85.0,203.0,120.0,124.0,95.0,12.0,104.0,143.0,127.0,100.0,74.0,104.0,148.0,127.0,96.0,82.0,158.0,124.0,163.0,121.0,154.0,127.0,128.0,98.0,124.0,109.0,168.0,89.0,162.0,159.0,137.0,133.0,125.0,127.0,131.0,90.0,89.0,88.0,125.0,88.0,94.0,83.0,89.0,118.0,109.0,87.0,79.0,147.0,92.0,103.0,119.0,80.0,110.0,128.0,88.0,117.0,98.0,126.0,115.0,86.0,108.0,94.0,151.0,88.0,97.0,92.0,102.0,93.0,126.0,82.0,86.0,105.0,101.0,100.0,57.0,95.0,128.0,99.0,113.0,108.0,105.0,109.0,93.0,82.0,95.0,84.0,106.0,122.0,108.0,94.0,103.0,124.0,92.0,88.0,72.0,93.0,102.0,125.0,99.0,89.0,95.0,92.0,133.0,112.0,85.0,84.0,118.0,99.0,109.0,103.0,94.0,105.0,101.0,103.0,86.0,91.0,95.0,99.0,90.0,88.0,85.0,98.0,94.0,95.0,108.0,87.0,95.0,108.0,110.0,113.0,71.0,75.0,75.0,97.0,83.0,66.0,83.0,110.0,88.0,112.0,77.0,91.0,58.0,69.0,61.0,100.0,88.0,128.0,122.0,60.0,126.0,166.0,113.0,88.0,151.0,107.0,42.0,94.0,130.0,87.0,116.0,77.0,117.0,84.0,92.0,130.0,89.0,109.0,104.0,114.0,133.0,101.0,137.0,88.0,88.0,115.0,104.0,114.0,128.0,125.0,102.0,94.0,69.0,92.0,90.0,101.0,77.0,89.0,98.0,88.0,152.0,66.0,103.0,95.0,79.0,89.0,89.0,119.0,70.0,104.0,121.0,106.0,106.0,113.0,119.0,82.0,95.0,100.0,110.0,66.0,99.0,131.0,86.0,118.0,126.0,130.0,99.0,89.0,136.0,131.0,165.0,110.0,85.0,92.0,89.0,117.0,114.0,63.0,94.0,74.0,103.0,58.0,94.0,92.0,104.0,117.0,89.0,129.0,160.0,150.0,158.0,118.0,100.0,90.0,64.0,92.0,63.0,105.0,76.0,67.0,99.0,91.0,92.0,87.0,100.0,113.0,105.0,94.0,109.0,116.0,99.0,89.0,92.0,82.0,70.0,131.0,105.0,130.0,98.0,90.0,105.0,106.0,126.0,84.0,65.0,121.0,163.0,92.0,124.0,107.0,25.0,103.0,45.0,101.0,60.0,90.0,96.0,98.0,94.0,74.0,112.0,115.0,92.0,93.0,95.0,117.0,66.0,110.0,97.0,78.0,102.0,62.0,90.0,97.0,105.0,90.0,129.0,92.0,109.0,77.0,107.0,69.0,87.0,99.0,105.0,99.0,108.0,66.0,94.0,93.0,75.0,96.0,95.0,32.0,70.0,94.0,117.0,78.0,100.0,91.0,115.0,122.0,107.0,109.0,104.0,95.0,100.0,93.0,104.0,134.0,75.0,100.0,105.0,104.0,101.0,96.0,80.0,75.0,75.0,79.0,78.0,75.0,78.0,166.0,90.0,132.0,91.0,117.0,103.0,176.0,116.0,60.0,149.0,86.0,171.0,103.0,116.0,169.0,134.0,159.0,130.0,103.0,173.0,195.0,111.0,99.0,110.0,94.0,87.0,103.0,78.0,108.0,62.0,97.0,102.0,95.0,106.0,115.0,80.0,102.0,99.0,74.0,100.0,150.0,90.0,103.0,87.0,127.0,129.0,120.0,129.0,137.0,91.0,103.0,145.0,23.0,25.0,92.0,81.0,115.0,150.0,106.0,115.0,140.0,79.0,104.0,52.0,90.0,83.0,97.0,85.0,69.0,94.0,121.0,89.0,94.0,90.0,86.0,100.0,84.0,58.0,95.0,92.0,51.0,69.0,104.0,101.0,91.0,153.0,149.0,94.0,79.0,137.0,53.0,166.0,162.0,93.0,155.0,158.0,138.0,151.0,96.0,106.0,141.0,127.0,130.0,140.0,145.0,132.0,41.0,170.0,157.0,165.0,143.0,106.0,73.0,71.0,161.0,187.0,127.0,165.0,117.0,134.0,116.0,94.0,168.0,109.0,148.0,135.0,127.0,115.0,150.0,87.0,127.0,185.0,75.0,177.0,81.0,137.0,118.0,124.0,123.0,90.0,91.0,87.0,89.0,81.0,47.0,46.0,173.0,108.0,125.0,124.0,137.0,171.0,160.0,87.0,110.0,67.0,92.0,96.0,92.0,105.0,127.0,109.0,150.0,100.0,93.0,87.0,134.0,118.0,85.0,60.0,96.0,133.0,118.0,150.0,132.0,121.0,127.0,152.0,126.0,104.0,162.0,120.0,65.0,133.0,96.0,95.0,97.0,94.0,84.0,91.0,97.0,102.0,93.0,105.0,116.0,102.0,81.0,93.0,97.0,94.0,110.0,88.0,143.0,42.0,117.0,96.0,96.0,69.0,74.0,108.0,102.0,61.0,85.0,49.0,87.0,85.0,59.0,97.0,104.0,77.0,63.0,86.0,62.0,108.0,95.0,99.0,62.0,106.0,117.0,94.0,147.0,89.0,89.0,111.0,118.0,153.0,87.0,122.0,97.0,121.0,124.0,68.0,94.0,71.0,76.0,89.0,92.0,63.0,135.0,112.0,88.0,97.0,83.0,86.0,126.0,133.0,91.0,104.0,105.0,123.0,89.0,102.0,102.0,91.0,87.0,103.0,193.0,176.0,124.0,135.0,106.0,133.0,107.0,78.0,192.0,55.0,132.0,148.0,46.0,46.0,46.0,74.0,46.0,114.0,117.0,108.0,97.0,122.0,115.0,104.0,140.0,101.0,99.0,102.0,94.0,92.0,125.0,151.0,86.0,93.0,82.0,100.0,53.0,87.0,102.0,112.0,105.0,106.0,89.0,91.0,61.0,94.0,80.0,118.0,141.0,94.0,84.0,103.0,85.0,69.0,100.0,85.0,97.0,129.0,143.0,90.0,92.0,81.0,105.0,91.0,111.0,103.0,118.0,148.0,75.0,89.0,128.0,86.0,83.0,109.0,111.0,40.0,70.0,108.0,106.0,106.0,81.0,94.0,102.0,102.0,89.0,162.0,147.0,224.0,137.0,109.0,163.0,162.0,104.0,27.0,67.0,142.0,78.0,26.0,123.0,124.0,81.0,97.0,81.0,119.0,149.0,106.0,140.0,121.0,99.0,96.0,128.0,94.0,101.0,93.0,109.0,51.0,61.0,73.0,113.0,125.0,92.0,94.0,65.0,135.0,86.0,80.0,142.0,131.0,66.0,88.0,99.0,96.0,126.0,65.0,54.0,76.0,100.0,81.0,123.0,153.0,103.0,141.0,115.0,120.0,104.0,97.0,120.0,132.0,135.0,82.0,158.0,107.0,122.0,95.0,98.0,113.0,94.0,120.0,92.0,132.0,84.0,76.0,75.0,93.0,98.0,91.0,168.0,68.0,97.0,115.0,148.0,149.0,113.0,137.0,140.0,144.0,103.0,82.0,94.0,66.0,135.0,96.0,144.0,108.0,127.0,127.0,90.0,126.0,121.0,119.0,109.0,132.0,118.0,84.0,93.0,96.0,145.0,158.0,132.0,111.0,133.0,143.0,117.0,107.0,83.0,102.0,52.0,85.0,113.0,88.0,89.0,59.0,80.0,107.0,90.0,66.0,92.0,101.0,135.0,107.0,74.0,74.0,74.0,83.0,79.0,72.0,80.0,75.0,75.0,72.0,80.0,93.0,86.0,47.0,107.0,119.0,108.0,85.0,90.0,72.0,23.0,68.0,85.0,84.0,118.0,116.0,81.0,73.0,140.0,130.0,103.0,103.0,104.0,82.0,94.0,91.0,112.0,102.0,101.0,119.0,62.0,64.0,137.0,108.0,132.0,87.0,92.0,78.0,71.0,89.0,99.0,110.0,60.0,90.0,95.0,135.0,98.0,96.0,113.0,90.0,114.0,79.0,98.0,70.0,116.0,139.0,101.0,148.0,92.0,107.0,90.0,151.0,88.0,99.0,99.0,132.0,95.0,155.0,162.0,163.0,102.0,96.0,85.0,97.0,95.0,83.0,89.0,103.0,84.0,160.0,113.0,99.0,86.0,83.0,113.0,86.0,78.0,96.0,95.0,57.0,92.0,81.0,98.0,95.0,116.0,100.0,95.0,70.0,51.0,94.0,83.0,92.0,88.0,104.0,104.0,124.0,113.0,124.0,96.0,63.0,56.0,92.0,91.0,94.0,94.0,82.0,90.0,71.0,97.0,83.0,98.0,92.0,88.0,62.0,165.0,166.0,166.0,159.0,160.0,159.0,93.0,95.0,22.0,105.0,101.0,124.0,100.0,54.0,121.0,103.0,129.0,89.0,92.0,111.0,92.0,91.0,172.0,95.0,80.0,84.0,97.0,86.0,58.0,114.0,103.0,93.0,84.0,67.0,110.0,110.0,84.0,63.0,94.0,116.0,104.0,143.0,116.0,14.0,108.0,59.0,99.0,74.0,103.0,105.0,84.0,109.0,96.0,83.0,89.0,97.0,95.0,106.0,116.0,91.0,90.0,86.0,97.0,170.0,153.0,104.0,162.0,163.0,106.0,108.0,130.0,133.0,148.0,105.0,121.0,66.0,102.0,108.0,139.0,91.0,90.0,81.0,72.0,89.0,92.0,81.0,161.0,92.0,24.0,66.0,100.0,122.0,81.0,77.0,104.0,73.0,102.0,47.0,98.0,102.0,112.0,85.0,108.0,87.0,84.0,107.0,117.0,89.0,119.0,88.0,126.0,130.0,102.0,154.0,44.0,104.0,96.0,108.0,105.0,47.0,44.0,109.0,89.0,92.0,122.0,154.0,152.0,161.0,123.0,44.0,106.0,71.0,153.0,98.0,143.0,123.0,97.0,80.0,106.0,101.0,119.0,140.0,59.0,164.0,151.0,84.0,142.0,110.0,99.0,86.0,101.0,88.0,86.0,151.0,116.0,128.0,128.0,86.0,88.0,80.0,61.0,81.0,72.0,98.0,64.0,95.0,96.0,111.0,127.0,117.0,123.0,139.0,95.0,148.0,121.0,79.0,127.0,92.0,162.0,163.0,83.0,112.0,117.0,90.0,95.0,99.0,125.0,93.0,89.0,120.0,114.0,109.0,83.0,107.0,129.0,135.0,149.0,136.0,138.0,177.0,61.0,126.0,137.0,114.0,104.0,96.0,147.0,53.0,95.0,95.0,149.0,106.0,108.0,66.0,54.0,55.0,55.0,55.0,55.0,113.0,83.0,53.0,95.0,50.0,97.0,92.0,97.0,105.0,107.0,99.0,101.0,81.0,92.0,131.0,111.0,99.0,101.0,96.0,85.0,119.0,110.0,89.0,76.0,103.0,101.0,98.0,74.0,156.0,88.0,70.0,88.0,93.0,143.0,83.0,107.0,93.0,134.0,78.0,81.0,106.0,84.0,90.0,95.0,63.0,58.0,18.0,102.0,42.0,76.0,92.0,118.0,91.0,63.0,110.0,91.0,93.0,93.0,108.0,72.0,114.0,97.0,118.0,92.0,137.0,97.0,101.0,143.0,97.0,107.0,168.0,117.0,80.0,87.0,160.0,88.0,85.0,104.0,62.0,109.0,103.0,99.0,102.0,97.0,99.0,101.0,102.0,102.0,107.0,99.0,106.0,103.0,123.0,98.0,118.0,100.0,101.0,103.0,101.0,104.0,104.0,100.0,136.0,102.0,105.0,110.0,105.0,105.0,102.0,98.0,118.0,106.0,106.0,92.0,92.0,83.0,123.0,90.0,57.0,103.0,92.0,87.0,56.0,94.0,118.0,98.0,125.0,113.0,110.0,71.0,108.0,123.0,97.0,99.0,97.0,100.0,75.0,54.0,54.0,55.0,54.0,55.0,67.0,129.0,105.0,78.0,99.0,96.0,95.0,87.0,134.0,119.0,128.0,128.0,91.0,109.0,79.0,88.0,92.0,88.0,71.0,64.0,83.0,124.0,123.0,97.0,91.0,79.0,93.0,54.0,109.0,93.0,78.0,86.0,94.0,116.0,154.0,179.0,93.0,97.0,99.0,104.0,94.0,111.0,108.0,92.0,78.0,87.0,99.0,91.0,102.0,67.0,57.0,78.0,130.0,80.0,90.0,93.0,66.0,93.0,119.0,90.0,105.0,83.0,84.0,98.0,91.0,89.0,90.0,123.0,117.0,100.0,91.0,87.0,146.0,104.0,85.0,109.0,74.0,94.0,84.0,98.0,22.0,141.0,95.0,117.0,100.0,75.0,69.0,97.0,92.0,92.0,85.0,83.0,118.0,84.0,146.0,93.0,97.0,93.0,62.0,98.0,92.0,22.0,115.0,88.0,87.0,86.0,94.0,105.0,88.0,93.0,98.0,88.0,93.0,81.0,31.0,83.0,38.0,108.0,90.0,97.0,81.0,70.0,62.0,104.0,97.0,99.0,96.0,84.0,112.0,90.0,88.0,89.0,24.0,100.0,116.0,101.0,92.0,79.0,72.0,96.0,92.0,61.0,88.0,88.0,113.0,67.0,65.0,62.0,107.0,69.0,77.0,98.0,71.0,101.0,64.0,125.0,125.0,59.0,91.0,107.0,101.0,93.0,86.0,81.0,92.0,89.0,107.0,94.0,110.0,81.0,77.0,64.0,111.0,97.0,77.0,94.0,94.0,91.0,93.0,95.0,80.0,95.0,90.0,73.0,108.0,25.0,105.0,100.0,96.0,97.0,73.0,132.0,96.0,92.0,99.0,78.0,87.0,89.0,60.0,91.0,83.0,102.0,111.0,88.0,93.0,29.0,88.0,107.0,91.0,180.0,75.0,92.0,90.0,114.0,80.0,99.0,90.0,72.0,71.0,77.0,70.0,73.0,107.0,81.0,86.0,128.0,45.0,46.0,112.0,87.0,87.0,106.0,100.0,101.0,117.0,114.0,94.0,100.0,96.0,87.0,81.0,96.0,52.0,114.0,105.0,157.0,80.0,97.0,105.0,96.0,60.0,93.0,99.0,109.0,96.0,102.0,107.0,60.0,86.0,163.0,98.0,101.0,76.0,95.0,127.0,86.0,116.0,82.0,102.0,88.0,109.0,75.0,23.0,83.0,97.0,62.0,84.0,90.0,82.0,96.0,65.0,90.0,93.0,91.0,100.0,101.0,74.0,44.0,43.0,95.0,94.0,68.0,72.0,92.0,107.0,108.0,74.0,55.0,24.0,101.0,29.0,78.0,73.0,72.0,60.0,47.0,70.0,120.0,74.0,57.0,72.0,91.0,62.0,85.0,85.0,90.0,105.0,59.0,137.0,86.0,135.0,64.0,72.0,83.0,82.0,90.0,80.0,130.0,62.0,90.0,92.0,155.0,78.0,92.0,60.0,69.0,92.0,143.0,78.0,84.0,78.0,93.0,84.0,58.0,82.0,83.0,48.0,73.0,103.0,81.0,75.0,68.0,71.0,71.0,94.0,80.0,78.0,72.0,93.0,105.0,130.0,100.0,44.0,80.0,72.0,75.0,26.0,84.0,120.0,76.0,95.0,96.0,94.0,94.0,93.0,96.0,94.0,94.0,93.0,95.0,93.0,75.0,103.0,84.0,29.0,104.0,81.0],"xaxis":"x","xbins":{"end":224.0,"size":0.7,"start":12.0},"yaxis":"y","type":"histogram"},{"legendgroup":"a","marker":{"color":"#6ad49b"},"mode":"lines","name":"a","showlegend":false,"x":[12.0,12.424,12.848,13.272,13.696,14.120000000000001,14.544,14.968,15.392,15.815999999999999,16.240000000000002,16.664,17.088,17.512,17.936,18.36,18.784,19.208,19.631999999999998,20.055999999999997,20.48,20.904,21.328,21.752000000000002,22.176000000000002,22.6,23.024,23.448,23.872,24.296,24.72,25.144,25.567999999999998,25.992,26.416,26.84,27.264,27.688000000000002,28.112,28.536,28.96,29.384,29.808,30.232,30.656,31.08,31.504,31.928,32.352000000000004,32.775999999999996,33.2,33.623999999999995,34.048,34.472,34.896,35.32,35.744,36.168,36.592,37.016,37.44,37.864000000000004,38.288,38.712,39.135999999999996,39.56,39.984,40.408,40.832,41.256,41.68,42.104,42.528,42.952,43.376000000000005,43.8,44.224,44.648,45.072,45.496,45.92,46.344,46.768,47.192,47.616,48.04,48.464,48.888,49.312,49.736,50.16,50.584,51.008,51.432,51.856,52.28,52.704,53.128,53.552,53.976,54.4,54.824,55.248,55.672,56.096,56.52,56.944,57.368,57.792,58.216,58.64,59.064,59.488,59.912,60.336,60.76,61.184,61.608,62.032,62.456,62.88,63.304,63.728,64.152,64.576,65.0,65.424,65.848,66.27199999999999,66.696,67.12,67.544,67.968,68.392,68.816,69.24000000000001,69.664,70.088,70.512,70.936,71.36,71.78399999999999,72.208,72.632,73.056,73.47999999999999,73.904,74.328,74.75200000000001,75.176,75.6,76.024,76.448,76.872,77.296,77.72,78.144,78.568,78.992,79.416,79.84,80.264,80.688,81.112,81.536,81.96,82.384,82.808,83.232,83.656,84.08,84.504,84.928,85.352,85.776,86.2,86.624,87.048,87.472,87.896,88.32,88.744,89.168,89.592,90.016,90.44,90.864,91.288,91.712,92.136,92.56,92.984,93.408,93.832,94.256,94.68,95.104,95.528,95.952,96.376,96.8,97.224,97.648,98.072,98.496,98.92,99.344,99.768,100.192,100.616,101.04,101.464,101.888,102.312,102.736,103.16,103.584,104.008,104.432,104.856,105.28,105.704,106.128,106.552,106.976,107.4,107.824,108.248,108.672,109.096,109.52,109.944,110.368,110.792,111.216,111.64,112.064,112.488,112.912,113.336,113.76,114.184,114.608,115.032,115.456,115.88,116.304,116.728,117.152,117.576,118.0,118.424,118.848,119.272,119.696,120.12,120.544,120.968,121.392,121.816,122.24,122.664,123.088,123.512,123.936,124.36,124.784,125.208,125.632,126.056,126.48,126.904,127.328,127.752,128.176,128.6,129.024,129.44799999999998,129.872,130.296,130.72,131.144,131.56799999999998,131.99200000000002,132.416,132.84,133.264,133.688,134.112,134.536,134.95999999999998,135.38400000000001,135.808,136.232,136.656,137.07999999999998,137.50400000000002,137.928,138.352,138.776,139.2,139.624,140.048,140.472,140.896,141.32,141.744,142.168,142.592,143.016,143.44,143.864,144.288,144.712,145.136,145.56,145.984,146.408,146.832,147.256,147.68,148.104,148.528,148.952,149.376,149.8,150.224,150.648,151.072,151.496,151.92,152.344,152.768,153.192,153.616,154.04,154.464,154.888,155.312,155.736,156.16,156.584,157.008,157.432,157.856,158.28,158.704,159.128,159.552,159.976,160.4,160.824,161.248,161.672,162.096,162.52,162.944,163.368,163.792,164.216,164.64,165.064,165.488,165.912,166.336,166.76,167.184,167.608,168.032,168.456,168.88,169.304,169.728,170.152,170.576,171.0,171.424,171.848,172.272,172.696,173.12,173.544,173.968,174.392,174.816,175.24,175.664,176.088,176.512,176.936,177.36,177.784,178.208,178.632,179.056,179.48,179.904,180.328,180.752,181.176,181.6,182.024,182.448,182.872,183.296,183.72,184.144,184.568,184.992,185.416,185.84,186.264,186.688,187.112,187.536,187.96,188.384,188.808,189.232,189.656,190.08,190.504,190.928,191.352,191.776,192.2,192.624,193.048,193.472,193.896,194.32,194.744,195.168,195.592,196.016,196.44,196.864,197.288,197.712,198.136,198.56,198.984,199.408,199.832,200.256,200.68,201.104,201.528,201.952,202.376,202.8,203.224,203.648,204.072,204.496,204.92,205.344,205.768,206.192,206.616,207.04,207.464,207.888,208.312,208.736,209.16,209.584,210.008,210.432,210.856,211.28,211.704,212.128,212.552,212.976,213.4,213.824,214.248,214.672,215.096,215.52,215.944,216.368,216.792,217.216,217.64,218.064,218.488,218.912,219.336,219.76,220.184,220.608,221.032,221.456,221.88,222.304,222.728,223.152,223.576],"xaxis":"x","y":[4.071002276898301e-05,4.305988411101503e-05,4.553324127422504e-05,4.813583114497028e-05,5.0873613010423136e-05,5.3752774734756033e-05,5.677973897416064e-05,5.996116942269328e-05,6.330397708041756e-05,6.681532653479728e-05,7.050264224575515e-05,7.437361482427351e-05,7.843620729385877e-05,8.269866132363638e-05,8.71695034212808e-05,9.185755107340975e-05,9.677191882050587e-05,0.00010192202425284873,0.00010731759391336547,0.00011296866909272873,0.00011888561150145327,0.00012507910880317548,0.0001315601799927219,0.00013834018060201355,0.0001454308077163019,0.00015284410478267206,0.00016059246619223085,0.00016868864161686278,0.00017714574008093127,0.0001859772337478174,0.00019519696140071245,0.00020481913159663552,0.0002148583254722136,0.00022532949917936518,0.00023624798592865982,0.00024762949761777893,0.0002594901260221802,0.00027184634352482633,0.0002847150033615592,0.0002981133393585381,0.000312058965137974,0.0003265698727683088,0.0003416644308348949,0.00035736138190723673,0.00037367983937888093,0.0003906392836561246,0.00040825955767187267,0.00042656086170116673,0.00044556374745517653,0.00046528911143075993,0.0004857581874931176,0.0005069925386694881,0.0005290140481323865,0.0005518449093514668,0.0005755076153937705,0.0006000249473528514,0.0006254199618880874,0.0006517159778563984,0.0006789365620195237,0.0007071055138110957,0.0007362468491488434,0.0007663847832784919,0.0007975437126371695,0.0008297481957255564,0.0008630229329793678,0.000897392745632397,0.0009328825535648318,0.0009695173521323339,0.00100732218797306,0.001046322133791685,0.0010865422621213513,0.0011280076180665106,0.0011707431910315976,0.001214773885442673,0.0012601244904712958,0.001306819648772201,0.0013548838242485842,0.0014043412688612559,0.0014552159885002649,0.0015075317079401285,0.0015613118349022573,0.0016165794232507827,0.0016733571353505184,0.0017316672036184742,0.0017915313913029213,0.001852970952526752,0.001916006591634461,0.0019806584218848644,0.0020469459235342774,0.0021148879013576155,0.002184502441657533,0.0022558068688143594,0.002328817701432238,0.0024035506081394875,0.0024800203631036946,0.0025582408013246566,0.0026382247737706376,0.0027199841024258846,0.002803529535319609,0.0028888707016089064,0.0029760160667902224,0.003064972888116066,0.003155747170295567,0.003248343621559397,0.003342765610171211,0.003439015121469467,0.0035370927155248277,0.003636997485499815,0.0037387270167984097,0.0038422773470944533,0.003947642927328438,0.004054816583763085,0.004163789481188473,0.004274551087367989,0.004387089138816315,0.004501389608000786,0.004617436672057104,0.004735212683109951,0.004854698140288406,0.0049758716635252016,0.005098709969227705,0.005223187847907324,0.005349278143852325,0.005476951736927554,0.005606177526582296,0.005736922418145589,0.0058691513114856415,0.006002827092107577,0.0061379106247607205,0.006274360749623569,0.006412134281131399,0.006551186009507798,0.006691468705057803,0.006832933125276239,0.00697552802482097,0.007119200168396081,0.007263894346585848,0.007409553394675407,0.007556118214489201,0.007703527799273262,0.007851719261642211,0.008000627864606409,0.008150187055689198,0.008300328504138645,0.008450982141232355,0.008602076203668133,0.00875353728002722,0.008905290360290967,0.009057258888385603,0.009209364817723577,0.009361528669703925,0.009513669595127656,0.009665705438478146,0.009817552805010241,0.009969127130585403,0.010120342754184329,0.01027111299302219,0.010421350220185494,0.010570965944703898,0.010719870893964003,0.010867975098366817,0.011015187978124544,0.011161418432087199,0.01130657492848386,0.011450565597458584,0.011593298325275602,0.011734680850064067,0.011874620858967674,0.012013026086560439,0.01214980441438566,0.012284863971471272,0.012418113235671362,0.012549461135680149,0.012678817153561927,0.012806091427637606,0.012931194855566281,0.013054039197458027,0.013174537178852632,0.013292602593397247,0.013408150405055332,0.013521096849678123,0.013631359535769898,0.013738857544278062,0.013843511527239626,0.013945243805116317,0.014043978462651653,0.01413964144308494,0.014232160640558706,0.014321465990558651,0.014407489558227422,0.014490165624396706,0.014569430769185177,0.014645223953013719,0.014717486594893,0.014786162647843064,0.014851198671308973,0.01491254390044165,0.014970150312118196,0.015023972687581502,0.015073968671584866,0.015120098827933246,0.015162326691319285,0.015200618815358675,0.015234944816736349,0.01526527741538185,0.015291592470599608,0.015313869013087065,0.015332089272781235,0.015346238702481925,0.015356305997207675,0.015362283109248313,0.015364165258886134,0.015361950940765657,0.015355641925900108,0.015345243259310856,0.015330763253304243,0.015312213476398322,0.015289608737920162,0.015262967068302423,0.015232309695115926,0.015197661014882754,0.015159048560722367,0.0151165029658907,0.015070057923279916,0.015019750140953721,0.014965619293800372,0.014907707971392582,0.014846061622150132,0.01478072849390777,0.014711759570997083,0.014639208507957219,0.014563131559995104,0.014483587510321244,0.014400637594492435,0.014314345421897709,0.014224776894528229,0.014132000123176348,0.014036085341212784,0.013937104816094566,0.013835132758759633,0.013730245231066815,0.013622520051442535,0.013512036698897658,0.013398876215579855,0.013283121108028087,0.013164855247297186,0.013044163768120894,0.012921132967282424,0.012795850201361248,0.012668403784024748,0.012538882883032452,0.012407377417119614,0.01227397795292539,0.012138775602129193,0.012001861918956754,0.0118633287982148,0.011723268374010921,0.011581772919311789,0.01143893474648989,0.01129484610900513,0.011149599104364034,0.011003285578494894,0.010855997031673148,0.010707824526126406,0.010558858595444049,0.010409189155911016,0.010258905419880447,0.010108095811294415,0.009956847883456382,0.009805248239153536,0.009653382453221195,0.009501334997635724,0.009349189169216324,0.009197027020010077,0.00904492929042837,0.008892975345196823,0.008741243112174488,0.008589809024091904,0.008438747963251376,0.008288133209226795,0.00813803638959357,0.007988527433714,0.00783967452959643,0.007691544083841027,0.0075442006846791025,0.007397707068106542,0.007252124087106993,0.007107510683954023,0.006963923865576452,0.006821418681965725,0.006680048207598651,0.006539863525844088,0.006400913716317382,0.0062632458451411,0.006126904958066943,0.005991934076408586,0.0058583741957315815,0.0057262642872424815,0.005595641301815216,0.005466540176590007,0.005338993844076015,0.005213033243686389,0.005088687335631407,0.004965983117092613,0.004844945640598662,0.0047255980345212905,0.0046079615256079684,0.0044920554634659176,0.004377897346910809,0.004265502852092116,0.004154885862305971,0.00404605849940569,0.003939031156719205,0.0038338125333824873,0.003730409669997627,0.003628827985524329,0.003529071315313568,0.003431141950192675,0.0033350406765115265,0.003240766817060316,0.0031483182727700927,0.003057691565108484,0.0029688818790840573,0.0028818831067741572,0.0027966878912926135,0.0027132876711151474,0.0026316727246822264,0.002551832215200817,0.002473754235568633,0.0023974258533463,0.0023228331557052367,0.0022499612942811654,0.0021787945298656053,0.002109316276869934,0.002041509147499216,0.001975354995575453,0.0019108349599524146,0.001847929507466945,0.0017866184753741012,0.0017268811132162736,0.0016686961240790401,0.0016120417051892564,0.0015568955878134663,0.001503235076417525,0.001451037087050911,0.001400278184921974,0.0013509346211328592,0.0013029823685456274,0.0012563971567535468,0.001211154506134216,0.0011672297609635277,0.0011245981215720945,0.0010832346755281009,0.0010431144278328953,0.0010042123301180301,0.0009665033088345583,0.0009299622924277262,0.0008945642374922041,0.0008602841539051378,0.0008270971289361693,0.0007949783503355892,0.000763903128403524,0.0007338469170448871,0.0007047853338164164,0.0006766941789737854,0.0006495494535282571,0.0006233273763237967,0.0005980044001469198,0.000573557226882809,0.0005499628217324728,0.0005271984265067853,0.0005052415720143489,0.0004840700895610117,0.00046366212157983235,0.0004439961314110266,0.0004250509122522286,0.0004068055952999997,0.0003892396571041658,0.00037233292615703936,0.0003560655887400581,0.0003404181940507671,0.0003253716586333534,0.0003109072701362471,0.00029700669042046727,0.00028365195804256173,0.0002708254901360354,0.0002585100837152343,0.00024668891642559904,0.00023534554676416303,0.00022446391379401671,0.00021402833637633808,0.00020402351194336146,0.00019443451483542448,0.00018524679422496145,0.0001764461716499802,0.00016801883817923503,0.0001599513512309143,0.0001522306310662809,0.0001448439569792454,0.0001377789632024288,0.00013102363454977793,0.00012456630181532458,0.00011839563694714641,0.00011250064801508769,0.00010687067399024794,0.00010149537935370203,9.636474855136347e-05,9.146908031132923e-05,8.679898183948397e-05,8.234536290856251e-05,7.8099429855296e-05,7.40526794996839e-05,7.01968929998658e-05,6.652412965548069e-05,6.302672067184065e-05,5.969726289666434e-05,5.652861254056353e-05,5.351387889191051e-05,5.064641803617134e-05,4.791982658923955e-05,4.532793545378149e-05,4.2864803607073485e-05,4.0524711928302084e-05,3.830215707279831e-05,3.619184540018137e-05,3.418868696291582e-05,3.22877895613156e-05,3.0484452870581125e-05,2.877416264501086e-05,2.7152585004108612e-05,2.561556080489317e-05,2.4159100104322316e-05,2.2779376715355853e-05,2.147272285981774e-05,2.0235623920860416e-05,1.906471329749407e-05,1.7956767363319777e-05,1.690870053129067e-05,1.591756042603044e-05,1.4980523164953757e-05,1.4094888749164646e-05,1.3258076564851146e-05,1.2467620995654684e-05,1.1721167146261416e-05,1.1016466677248817e-05,1.0351373751015107e-05,9.723841088430689e-06,9.131916135670482e-06,8.57373734052022e-06,8.047530537294062e-06,7.551605439356859e-06,7.084352238111438e-06,6.6442383071883784e-06,6.2298050104634035e-06,5.839664612424182e-06,5.472497289315806e-06,5.127048239408935e-06,4.802124890659919e-06,4.496594203963844e-06,4.209380070143012e-06,3.939460798760792e-06,3.6858666968066088e-06,3.447677735260247e-06,3.224021301511807e-06,3.014070035589323e-06,2.8170397481264913e-06,2.6321874179894446e-06,2.458809267472808e-06,2.296238912971803e-06,2.143845589037985e-06,2.0010324437315663e-06,1.8672349031919935e-06,1.7419191033616695e-06,1.6245803868137203e-06,1.514741862654153e-06,1.411953027490843e-06,1.3157884454868625e-06,1.2258464855428972e-06,1.1417481136828396e-06,1.0631357387483344e-06,9.89672109541029e-07,9.210392615861649e-07,8.56937511727251e-07,7.970844987989256e-07,7.412142686634058e-07,6.890764019352201e-07,6.404351827588339e-07,5.95068807044332e-07,5.52768628607231e-07,5.133384416998098e-07,4.765937984628269e-07,4.4236135986799744e-07,4.1047827876325584e-07,3.807916136741224e-07,3.531577720558488e-07,3.274419817319281e-07,3.0351778929515735e-07,2.8126658428762743e-07,2.605771480157178e-07,2.413452258953206e-07],"yaxis":"y","type":"scatter"},{"legendgroup":"a","marker":{"color":"#6ad49b","symbol":"line-ns-open"},"mode":"markers","name":"a","showlegend":false,"x":[135.0,106.0,107.0,81.0,107.0,110.0,104.0,93.0,94.0,124.0,137.0,134.0,106.0,209.0,86.0,24.0,117.0,92.0,114.0,121.0,94.0,109.0,110.0,96.0,97.0,121.0,119.0,138.0,93.0,111.0,88.0,81.0,73.0,86.0,116.0,85.0,134.0,109.0,102.0,101.0,88.0,101.0,28.0,103.0,131.0,166.0,105.0,82.0,107.0,84.0,103.0,112.0,89.0,121.0,87.0,136.0,118.0,129.0,94.0,88.0,158.0,78.0,100.0,84.0,107.0,101.0,107.0,74.0,60.0,143.0,105.0,110.0,98.0,100.0,74.0,46.0,88.0,54.0,59.0,100.0,95.0,100.0,93.0,94.0,121.0,78.0,116.0,61.0,98.0,123.0,85.0,92.0,87.0,92.0,112.0,97.0,78.0,103.0,107.0,68.0,68.0,99.0,69.0,81.0,69.0,68.0,68.0,91.0,86.0,40.0,102.0,119.0,94.0,119.0,90.0,105.0,108.0,108.0,87.0,112.0,101.0,89.0,91.0,200.0,119.0,90.0,103.0,110.0,133.0,124.0,118.0,98.0,85.0,115.0,137.0,98.0,110.0,92.0,116.0,94.0,134.0,102.0,98.0,100.0,105.0,118.0,153.0,109.0,99.0,96.0,91.0,185.0,92.0,127.0,98.0,111.0,120.0,137.0,121.0,89.0,91.0,135.0,98.0,139.0,95.0,98.0,91.0,122.0,100.0,103.0,129.0,133.0,105.0,107.0,136.0,138.0,129.0,141.0,99.0,89.0,90.0,98.0,88.0,65.0,126.0,63.0,20.0,88.0,98.0,94.0,106.0,118.0,86.0,94.0,82.0,92.0,95.0,52.0,108.0,91.0,96.0,67.0,83.0,94.0,97.0,91.0,98.0,92.0,118.0,87.0,121.0,98.0,95.0,107.0,100.0,101.0,96.0,99.0,112.0,127.0,140.0,63.0,97.0,86.0,93.0,62.0,96.0,104.0,123.0,100.0,99.0,151.0,102.0,106.0,87.0,70.0,111.0,101.0,109.0,110.0,102.0,102.0,24.0,98.0,93.0,77.0,103.0,45.0,119.0,147.0,123.0,99.0,98.0,106.0,112.0,85.0,91.0,88.0,101.0,90.0,96.0,99.0,101.0,111.0,96.0,86.0,97.0,121.0,90.0,88.0,64.0,104.0,122.0,125.0,92.0,129.0,79.0,121.0,106.0,124.0,87.0,125.0,154.0,86.0,111.0,97.0,66.0,121.0,91.0,91.0,82.0,117.0,81.0,84.0,81.0,107.0,94.0,163.0,90.0,96.0,91.0,93.0,64.0,89.0,102.0,116.0,133.0,108.0,97.0,89.0,146.0,146.0,130.0,106.0,58.0,152.0,87.0,121.0,182.0,106.0,102.0,98.0,83.0,82.0,111.0,171.0,84.0,112.0,80.0,88.0,124.0,104.0,76.0,91.0,64.0,94.0,86.0,157.0,83.0,95.0,106.0,87.0,90.0,65.0,105.0,107.0,103.0,88.0,96.0,67.0,140.0,93.0,90.0,125.0,133.0,128.0,88.0,108.0,93.0,96.0,102.0,108.0,102.0,93.0,87.0,94.0,100.0,88.0,109.0,138.0,149.0,82.0,86.0,112.0,98.0,113.0,47.0,106.0,113.0,119.0,109.0,98.0,88.0,116.0,106.0,95.0,95.0,113.0,91.0,102.0,100.0,81.0,98.0,110.0,106.0,85.0,66.0,92.0,92.0,91.0,133.0,135.0,120.0,118.0,123.0,98.0,98.0,98.0,104.0,167.0,95.0,117.0,100.0,86.0,112.0,69.0,119.0,123.0,93.0,107.0,46.0,91.0,103.0,117.0,76.0,67.0,114.0,67.0,77.0,84.0,63.0,99.0,59.0,102.0,81.0,145.0,102.0,116.0,164.0,87.0,90.0,106.0,116.0,134.0,133.0,118.0,120.0,154.0,128.0,115.0,177.0,105.0,112.0,120.0,119.0,100.0,92.0,104.0,97.0,88.0,128.0,130.0,108.0,98.0,89.0,90.0,130.0,99.0,107.0,94.0,111.0,100.0,77.0,57.0,107.0,93.0,103.0,153.0,94.0,149.0,111.0,58.0,56.0,110.0,98.0,106.0,102.0,93.0,81.0,77.0,98.0,97.0,136.0,161.0,32.0,107.0,98.0,83.0,77.0,133.0,105.0,79.0,106.0,90.0,87.0,95.0,65.0,131.0,105.0,119.0,107.0,112.0,97.0,106.0,66.0,106.0,110.0,102.0,105.0,112.0,96.0,97.0,113.0,53.0,54.0,53.0,54.0,113.0,53.0,53.0,52.0,54.0,54.0,53.0,53.0,53.0,91.0,126.0,99.0,134.0,26.0,117.0,89.0,94.0,101.0,94.0,99.0,114.0,48.0,118.0,119.0,104.0,98.0,93.0,90.0,122.0,113.0,141.0,81.0,176.0,119.0,73.0,87.0,88.0,15.0,55.0,122.0,117.0,95.0,63.0,73.0,107.0,99.0,96.0,113.0,69.0,99.0,87.0,112.0,116.0,99.0,101.0,116.0,83.0,110.0,110.0,99.0,59.0,126.0,97.0,89.0,88.0,91.0,113.0,117.0,90.0,95.0,98.0,102.0,61.0,93.0,117.0,108.0,126.0,93.0,60.0,142.0,113.0,90.0,107.0,74.0,119.0,114.0,125.0,71.0,110.0,127.0,118.0,90.0,94.0,90.0,98.0,99.0,133.0,94.0,116.0,93.0,103.0,84.0,90.0,107.0,89.0,96.0,130.0,90.0,86.0,79.0,93.0,12.0,62.0,94.0,93.0,114.0,110.0,125.0,119.0,109.0,100.0,98.0,98.0,128.0,105.0,109.0,83.0,101.0,78.0,110.0,116.0,141.0,120.0,97.0,109.0,105.0,102.0,127.0,101.0,135.0,124.0,103.0,105.0,98.0,93.0,113.0,106.0,99.0,123.0,91.0,84.0,99.0,122.0,101.0,99.0,91.0,137.0,111.0,66.0,159.0,125.0,95.0,95.0,96.0,90.0,53.0,87.0,90.0,96.0,150.0,105.0,112.0,119.0,142.0,94.0,97.0,137.0,100.0,125.0,128.0,108.0,136.0,165.0,124.0,133.0,116.0,102.0,87.0,70.0,92.0,88.0,100.0,111.0,128.0,98.0,104.0,110.0,87.0,126.0,103.0,111.0,100.0,112.0,99.0,125.0,147.0,88.0,79.0,110.0,128.0,110.0,79.0,87.0,133.0,116.0,119.0,53.0,82.0,99.0,87.0,90.0,124.0,53.0,88.0,104.0,92.0,126.0,140.0,87.0,93.0,80.0,92.0,64.0,72.0,148.0,110.0,119.0,163.0,125.0,87.0,96.0,91.0,122.0,125.0,95.0,99.0,98.0,90.0,97.0,97.0,86.0,114.0,96.0,99.0,88.0,168.0,89.0,90.0,100.0,102.0,98.0,92.0,116.0,111.0,91.0,99.0,99.0,98.0,73.0,113.0,153.0,81.0,54.0,93.0,123.0,96.0,101.0,53.0,102.0,141.0,111.0,131.0,116.0,119.0,146.0,101.0,82.0,90.0,87.0,95.0,94.0,129.0,94.0,103.0,133.0,135.0,85.0,91.0,105.0,59.0,91.0,94.0,89.0,131.0,170.0,91.0,128.0,88.0,97.0,110.0,170.0,92.0,113.0,126.0,86.0,120.0,76.0,60.0,118.0,88.0,55.0,110.0,113.0,110.0,94.0,54.0,53.0,103.0,126.0,89.0,124.0,128.0,88.0,83.0,96.0,118.0,91.0,91.0,53.0,103.0,108.0,88.0,81.0,124.0,102.0,98.0,132.0,89.0,109.0,75.0,126.0,61.0,126.0,108.0,89.0,100.0,129.0,108.0,101.0,105.0,115.0,118.0,130.0,101.0,115.0,99.0,97.0,61.0,97.0,101.0,92.0,123.0,108.0,104.0,98.0,65.0,88.0,106.0,69.0,97.0,80.0,85.0,99.0,98.0,115.0,138.0,90.0,86.0,90.0,117.0,132.0,127.0,125.0,128.0,126.0,87.0,98.0,116.0,90.0,111.0,100.0,123.0,106.0,100.0,104.0,99.0,105.0,128.0,91.0,127.0,100.0,131.0,104.0,94.0,124.0,106.0,108.0,103.0,100.0,79.0,92.0,116.0,83.0,118.0,162.0,162.0,126.0,109.0,109.0,111.0,89.0,97.0,103.0,88.0,120.0,90.0,99.0,83.0,107.0,109.0,105.0,87.0,93.0,90.0,89.0,131.0,114.0,92.0,75.0,107.0,97.0,90.0,98.0,100.0,103.0,66.0,106.0,125.0,77.0,130.0,119.0,110.0,120.0,102.0,125.0,103.0,127.0,121.0,101.0,113.0,118.0,132.0,106.0,116.0,117.0,90.0,61.0,90.0,107.0,80.0,58.0,58.0,56.0,110.0,70.0,117.0,89.0,84.0,131.0,91.0,103.0,100.0,139.0,103.0,42.0,79.0,90.0,88.0,96.0,114.0,89.0,103.0,127.0,113.0,111.0,112.0,103.0,104.0,102.0,86.0,97.0,111.0,132.0,89.0,102.0,120.0,137.0,82.0,85.0,115.0,107.0,130.0,57.0,62.0,112.0,102.0,93.0,111.0,93.0,91.0,118.0,63.0,74.0,136.0,90.0,132.0,101.0,59.0,84.0,91.0,93.0,91.0,101.0,110.0,82.0,100.0,52.0,127.0,86.0,107.0,22.0,22.0,91.0,99.0,105.0,105.0,88.0,128.0,113.0,50.0,92.0,110.0,120.0,123.0,141.0,97.0,118.0,95.0,127.0,118.0,74.0,137.0,86.0,125.0,114.0,143.0,121.0,119.0,108.0,150.0,88.0,105.0,97.0,96.0,91.0,49.0,96.0,96.0,90.0,97.0,94.0,88.0,118.0,86.0,127.0,88.0,125.0,116.0,102.0,65.0,106.0,117.0,93.0,136.0,116.0,121.0,89.0,104.0,91.0,99.0,91.0,117.0,135.0,135.0,91.0,95.0,95.0,82.0,124.0,86.0,115.0,99.0,101.0,142.0,102.0,95.0,120.0,96.0,92.0,110.0,133.0,143.0,111.0,144.0,121.0,99.0,93.0,100.0,102.0,123.0,127.0,116.0,119.0,105.0,134.0,90.0,93.0,93.0,108.0,115.0,119.0,61.0,109.0,97.0,154.0,120.0,148.0,121.0,95.0,102.0,87.0,114.0,48.0,124.0,101.0,28.0,61.0,56.0,81.0,50.0,49.0,72.0,100.0,30.0,146.0,127.0,46.0,57.0,90.0,86.0,122.0,97.0,109.0,122.0,96.0,91.0,110.0,115.0,150.0,90.0,89.0,93.0,105.0,105.0,124.0,44.0,91.0,95.0,78.0,22.0,95.0,82.0,89.0,90.0,94.0,85.0,29.0,96.0,137.0,27.0,112.0,112.0,107.0,153.0,123.0,90.0,84.0,87.0,145.0,135.0,109.0,86.0,107.0,110.0,86.0,85.0,75.0,90.0,105.0,124.0,110.0,96.0,110.0,105.0,98.0,111.0,116.0,106.0,109.0,88.0,155.0,119.0,122.0,87.0,98.0,130.0,99.0,105.0,110.0,81.0,115.0,113.0,146.0,118.0,122.0,109.0,117.0,95.0,124.0,96.0,96.0,123.0,85.0,93.0,30.0,82.0,95.0,127.0,120.0,114.0,119.0,101.0,96.0,120.0,139.0,95.0,44.0,104.0,110.0,91.0,91.0,114.0,91.0,64.0,89.0,95.0,86.0,94.0,133.0,122.0,86.0,111.0,132.0,111.0,121.0,76.0,92.0,119.0,45.0,75.0,71.0,81.0,115.0,128.0,95.0,114.0,99.0,132.0,106.0,122.0,79.0,78.0,111.0,112.0,107.0,102.0,81.0,129.0,86.0,116.0,95.0,122.0,98.0,103.0,117.0,95.0,146.0,136.0,91.0,123.0,144.0,103.0,126.0,121.0,91.0,158.0,157.0,124.0,106.0,92.0,80.0,94.0,126.0,129.0,133.0,109.0,143.0,140.0,150.0,128.0,120.0,154.0,45.0,132.0,126.0,95.0,91.0,135.0,139.0,131.0,118.0,129.0,131.0,146.0,123.0,141.0,93.0,91.0,58.0,91.0,112.0,119.0,106.0,131.0,104.0,126.0,118.0,116.0,122.0,105.0,90.0,87.0,102.0,89.0,94.0,126.0,89.0,90.0,115.0,93.0,138.0,74.0,102.0,103.0,119.0,104.0,161.0,51.0,86.0,140.0,122.0,95.0,205.0,121.0,131.0,80.0,63.0,150.0,159.0,131.0,130.0,99.0,106.0,97.0,118.0,98.0,144.0,110.0,96.0,57.0,98.0,119.0,92.0,94.0,151.0,74.0,89.0,124.0,84.0,121.0,85.0,25.0,121.0,88.0,111.0,81.0,137.0,56.0,80.0,94.0,94.0,72.0,140.0,91.0,97.0,146.0,118.0,133.0,106.0,128.0,137.0,145.0,106.0,61.0,137.0,129.0,99.0,155.0,142.0,214.0,137.0,128.0,89.0,67.0,151.0,121.0,101.0,145.0,147.0,90.0,129.0,129.0,136.0,117.0,150.0,134.0,100.0,44.0,44.0,111.0,147.0,115.0,121.0,103.0,73.0,129.0,102.0,122.0,155.0,139.0,131.0,127.0,156.0,132.0,130.0,137.0,132.0,156.0,132.0,119.0,140.0,101.0,126.0,107.0,112.0,111.0,126.0,130.0,126.0,127.0,108.0,163.0,95.0,103.0,110.0,111.0,91.0,101.0,124.0,102.0,112.0,103.0,92.0,93.0,112.0,113.0,92.0,153.0,111.0,141.0,128.0,117.0,100.0,112.0,110.0,107.0,109.0,100.0,114.0,98.0,100.0,123.0,86.0,112.0,106.0,106.0,102.0,86.0,146.0,122.0,126.0,116.0,135.0,125.0,96.0,130.0,135.0,88.0,96.0,122.0,110.0,104.0,96.0,109.0,95.0,110.0,110.0,79.0,101.0,112.0,94.0,94.0,86.0,84.0,95.0,113.0,111.0,135.0,95.0,93.0,101.0,90.0,112.0,141.0,120.0,140.0,99.0,86.0,104.0,100.0,50.0,124.0,31.0,90.0,196.0,93.0,110.0,162.0,139.0,130.0,145.0,88.0,91.0,91.0,102.0,95.0,119.0,114.0,100.0,54.0,150.0,108.0,90.0,101.0,104.0,88.0,93.0,79.0,92.0,90.0,86.0,90.0,103.0,86.0,108.0,97.0,92.0,101.0,83.0,85.0,85.0,106.0,86.0,131.0,105.0,134.0,85.0,203.0,120.0,124.0,95.0,12.0,104.0,143.0,127.0,100.0,74.0,104.0,148.0,127.0,96.0,82.0,158.0,124.0,163.0,121.0,154.0,127.0,128.0,98.0,124.0,109.0,168.0,89.0,162.0,159.0,137.0,133.0,125.0,127.0,131.0,90.0,89.0,88.0,125.0,88.0,94.0,83.0,89.0,118.0,109.0,87.0,79.0,147.0,92.0,103.0,119.0,80.0,110.0,128.0,88.0,117.0,98.0,126.0,115.0,86.0,108.0,94.0,151.0,88.0,97.0,92.0,102.0,93.0,126.0,82.0,86.0,105.0,101.0,100.0,57.0,95.0,128.0,99.0,113.0,108.0,105.0,109.0,93.0,82.0,95.0,84.0,106.0,122.0,108.0,94.0,103.0,124.0,92.0,88.0,72.0,93.0,102.0,125.0,99.0,89.0,95.0,92.0,133.0,112.0,85.0,84.0,118.0,99.0,109.0,103.0,94.0,105.0,101.0,103.0,86.0,91.0,95.0,99.0,90.0,88.0,85.0,98.0,94.0,95.0,108.0,87.0,95.0,108.0,110.0,113.0,71.0,75.0,75.0,97.0,83.0,66.0,83.0,110.0,88.0,112.0,77.0,91.0,58.0,69.0,61.0,100.0,88.0,128.0,122.0,60.0,126.0,166.0,113.0,88.0,151.0,107.0,42.0,94.0,130.0,87.0,116.0,77.0,117.0,84.0,92.0,130.0,89.0,109.0,104.0,114.0,133.0,101.0,137.0,88.0,88.0,115.0,104.0,114.0,128.0,125.0,102.0,94.0,69.0,92.0,90.0,101.0,77.0,89.0,98.0,88.0,152.0,66.0,103.0,95.0,79.0,89.0,89.0,119.0,70.0,104.0,121.0,106.0,106.0,113.0,119.0,82.0,95.0,100.0,110.0,66.0,99.0,131.0,86.0,118.0,126.0,130.0,99.0,89.0,136.0,131.0,165.0,110.0,85.0,92.0,89.0,117.0,114.0,63.0,94.0,74.0,103.0,58.0,94.0,92.0,104.0,117.0,89.0,129.0,160.0,150.0,158.0,118.0,100.0,90.0,64.0,92.0,63.0,105.0,76.0,67.0,99.0,91.0,92.0,87.0,100.0,113.0,105.0,94.0,109.0,116.0,99.0,89.0,92.0,82.0,70.0,131.0,105.0,130.0,98.0,90.0,105.0,106.0,126.0,84.0,65.0,121.0,163.0,92.0,124.0,107.0,25.0,103.0,45.0,101.0,60.0,90.0,96.0,98.0,94.0,74.0,112.0,115.0,92.0,93.0,95.0,117.0,66.0,110.0,97.0,78.0,102.0,62.0,90.0,97.0,105.0,90.0,129.0,92.0,109.0,77.0,107.0,69.0,87.0,99.0,105.0,99.0,108.0,66.0,94.0,93.0,75.0,96.0,95.0,32.0,70.0,94.0,117.0,78.0,100.0,91.0,115.0,122.0,107.0,109.0,104.0,95.0,100.0,93.0,104.0,134.0,75.0,100.0,105.0,104.0,101.0,96.0,80.0,75.0,75.0,79.0,78.0,75.0,78.0,166.0,90.0,132.0,91.0,117.0,103.0,176.0,116.0,60.0,149.0,86.0,171.0,103.0,116.0,169.0,134.0,159.0,130.0,103.0,173.0,195.0,111.0,99.0,110.0,94.0,87.0,103.0,78.0,108.0,62.0,97.0,102.0,95.0,106.0,115.0,80.0,102.0,99.0,74.0,100.0,150.0,90.0,103.0,87.0,127.0,129.0,120.0,129.0,137.0,91.0,103.0,145.0,23.0,25.0,92.0,81.0,115.0,150.0,106.0,115.0,140.0,79.0,104.0,52.0,90.0,83.0,97.0,85.0,69.0,94.0,121.0,89.0,94.0,90.0,86.0,100.0,84.0,58.0,95.0,92.0,51.0,69.0,104.0,101.0,91.0,153.0,149.0,94.0,79.0,137.0,53.0,166.0,162.0,93.0,155.0,158.0,138.0,151.0,96.0,106.0,141.0,127.0,130.0,140.0,145.0,132.0,41.0,170.0,157.0,165.0,143.0,106.0,73.0,71.0,161.0,187.0,127.0,165.0,117.0,134.0,116.0,94.0,168.0,109.0,148.0,135.0,127.0,115.0,150.0,87.0,127.0,185.0,75.0,177.0,81.0,137.0,118.0,124.0,123.0,90.0,91.0,87.0,89.0,81.0,47.0,46.0,173.0,108.0,125.0,124.0,137.0,171.0,160.0,87.0,110.0,67.0,92.0,96.0,92.0,105.0,127.0,109.0,150.0,100.0,93.0,87.0,134.0,118.0,85.0,60.0,96.0,133.0,118.0,150.0,132.0,121.0,127.0,152.0,126.0,104.0,162.0,120.0,65.0,133.0,96.0,95.0,97.0,94.0,84.0,91.0,97.0,102.0,93.0,105.0,116.0,102.0,81.0,93.0,97.0,94.0,110.0,88.0,143.0,42.0,117.0,96.0,96.0,69.0,74.0,108.0,102.0,61.0,85.0,49.0,87.0,85.0,59.0,97.0,104.0,77.0,63.0,86.0,62.0,108.0,95.0,99.0,62.0,106.0,117.0,94.0,147.0,89.0,89.0,111.0,118.0,153.0,87.0,122.0,97.0,121.0,124.0,68.0,94.0,71.0,76.0,89.0,92.0,63.0,135.0,112.0,88.0,97.0,83.0,86.0,126.0,133.0,91.0,104.0,105.0,123.0,89.0,102.0,102.0,91.0,87.0,103.0,193.0,176.0,124.0,135.0,106.0,133.0,107.0,78.0,192.0,55.0,132.0,148.0,46.0,46.0,46.0,74.0,46.0,114.0,117.0,108.0,97.0,122.0,115.0,104.0,140.0,101.0,99.0,102.0,94.0,92.0,125.0,151.0,86.0,93.0,82.0,100.0,53.0,87.0,102.0,112.0,105.0,106.0,89.0,91.0,61.0,94.0,80.0,118.0,141.0,94.0,84.0,103.0,85.0,69.0,100.0,85.0,97.0,129.0,143.0,90.0,92.0,81.0,105.0,91.0,111.0,103.0,118.0,148.0,75.0,89.0,128.0,86.0,83.0,109.0,111.0,40.0,70.0,108.0,106.0,106.0,81.0,94.0,102.0,102.0,89.0,162.0,147.0,224.0,137.0,109.0,163.0,162.0,104.0,27.0,67.0,142.0,78.0,26.0,123.0,124.0,81.0,97.0,81.0,119.0,149.0,106.0,140.0,121.0,99.0,96.0,128.0,94.0,101.0,93.0,109.0,51.0,61.0,73.0,113.0,125.0,92.0,94.0,65.0,135.0,86.0,80.0,142.0,131.0,66.0,88.0,99.0,96.0,126.0,65.0,54.0,76.0,100.0,81.0,123.0,153.0,103.0,141.0,115.0,120.0,104.0,97.0,120.0,132.0,135.0,82.0,158.0,107.0,122.0,95.0,98.0,113.0,94.0,120.0,92.0,132.0,84.0,76.0,75.0,93.0,98.0,91.0,168.0,68.0,97.0,115.0,148.0,149.0,113.0,137.0,140.0,144.0,103.0,82.0,94.0,66.0,135.0,96.0,144.0,108.0,127.0,127.0,90.0,126.0,121.0,119.0,109.0,132.0,118.0,84.0,93.0,96.0,145.0,158.0,132.0,111.0,133.0,143.0,117.0,107.0,83.0,102.0,52.0,85.0,113.0,88.0,89.0,59.0,80.0,107.0,90.0,66.0,92.0,101.0,135.0,107.0,74.0,74.0,74.0,83.0,79.0,72.0,80.0,75.0,75.0,72.0,80.0,93.0,86.0,47.0,107.0,119.0,108.0,85.0,90.0,72.0,23.0,68.0,85.0,84.0,118.0,116.0,81.0,73.0,140.0,130.0,103.0,103.0,104.0,82.0,94.0,91.0,112.0,102.0,101.0,119.0,62.0,64.0,137.0,108.0,132.0,87.0,92.0,78.0,71.0,89.0,99.0,110.0,60.0,90.0,95.0,135.0,98.0,96.0,113.0,90.0,114.0,79.0,98.0,70.0,116.0,139.0,101.0,148.0,92.0,107.0,90.0,151.0,88.0,99.0,99.0,132.0,95.0,155.0,162.0,163.0,102.0,96.0,85.0,97.0,95.0,83.0,89.0,103.0,84.0,160.0,113.0,99.0,86.0,83.0,113.0,86.0,78.0,96.0,95.0,57.0,92.0,81.0,98.0,95.0,116.0,100.0,95.0,70.0,51.0,94.0,83.0,92.0,88.0,104.0,104.0,124.0,113.0,124.0,96.0,63.0,56.0,92.0,91.0,94.0,94.0,82.0,90.0,71.0,97.0,83.0,98.0,92.0,88.0,62.0,165.0,166.0,166.0,159.0,160.0,159.0,93.0,95.0,22.0,105.0,101.0,124.0,100.0,54.0,121.0,103.0,129.0,89.0,92.0,111.0,92.0,91.0,172.0,95.0,80.0,84.0,97.0,86.0,58.0,114.0,103.0,93.0,84.0,67.0,110.0,110.0,84.0,63.0,94.0,116.0,104.0,143.0,116.0,14.0,108.0,59.0,99.0,74.0,103.0,105.0,84.0,109.0,96.0,83.0,89.0,97.0,95.0,106.0,116.0,91.0,90.0,86.0,97.0,170.0,153.0,104.0,162.0,163.0,106.0,108.0,130.0,133.0,148.0,105.0,121.0,66.0,102.0,108.0,139.0,91.0,90.0,81.0,72.0,89.0,92.0,81.0,161.0,92.0,24.0,66.0,100.0,122.0,81.0,77.0,104.0,73.0,102.0,47.0,98.0,102.0,112.0,85.0,108.0,87.0,84.0,107.0,117.0,89.0,119.0,88.0,126.0,130.0,102.0,154.0,44.0,104.0,96.0,108.0,105.0,47.0,44.0,109.0,89.0,92.0,122.0,154.0,152.0,161.0,123.0,44.0,106.0,71.0,153.0,98.0,143.0,123.0,97.0,80.0,106.0,101.0,119.0,140.0,59.0,164.0,151.0,84.0,142.0,110.0,99.0,86.0,101.0,88.0,86.0,151.0,116.0,128.0,128.0,86.0,88.0,80.0,61.0,81.0,72.0,98.0,64.0,95.0,96.0,111.0,127.0,117.0,123.0,139.0,95.0,148.0,121.0,79.0,127.0,92.0,162.0,163.0,83.0,112.0,117.0,90.0,95.0,99.0,125.0,93.0,89.0,120.0,114.0,109.0,83.0,107.0,129.0,135.0,149.0,136.0,138.0,177.0,61.0,126.0,137.0,114.0,104.0,96.0,147.0,53.0,95.0,95.0,149.0,106.0,108.0,66.0,54.0,55.0,55.0,55.0,55.0,113.0,83.0,53.0,95.0,50.0,97.0,92.0,97.0,105.0,107.0,99.0,101.0,81.0,92.0,131.0,111.0,99.0,101.0,96.0,85.0,119.0,110.0,89.0,76.0,103.0,101.0,98.0,74.0,156.0,88.0,70.0,88.0,93.0,143.0,83.0,107.0,93.0,134.0,78.0,81.0,106.0,84.0,90.0,95.0,63.0,58.0,18.0,102.0,42.0,76.0,92.0,118.0,91.0,63.0,110.0,91.0,93.0,93.0,108.0,72.0,114.0,97.0,118.0,92.0,137.0,97.0,101.0,143.0,97.0,107.0,168.0,117.0,80.0,87.0,160.0,88.0,85.0,104.0,62.0,109.0,103.0,99.0,102.0,97.0,99.0,101.0,102.0,102.0,107.0,99.0,106.0,103.0,123.0,98.0,118.0,100.0,101.0,103.0,101.0,104.0,104.0,100.0,136.0,102.0,105.0,110.0,105.0,105.0,102.0,98.0,118.0,106.0,106.0,92.0,92.0,83.0,123.0,90.0,57.0,103.0,92.0,87.0,56.0,94.0,118.0,98.0,125.0,113.0,110.0,71.0,108.0,123.0,97.0,99.0,97.0,100.0,75.0,54.0,54.0,55.0,54.0,55.0,67.0,129.0,105.0,78.0,99.0,96.0,95.0,87.0,134.0,119.0,128.0,128.0,91.0,109.0,79.0,88.0,92.0,88.0,71.0,64.0,83.0,124.0,123.0,97.0,91.0,79.0,93.0,54.0,109.0,93.0,78.0,86.0,94.0,116.0,154.0,179.0,93.0,97.0,99.0,104.0,94.0,111.0,108.0,92.0,78.0,87.0,99.0,91.0,102.0,67.0,57.0,78.0,130.0,80.0,90.0,93.0,66.0,93.0,119.0,90.0,105.0,83.0,84.0,98.0,91.0,89.0,90.0,123.0,117.0,100.0,91.0,87.0,146.0,104.0,85.0,109.0,74.0,94.0,84.0,98.0,22.0,141.0,95.0,117.0,100.0,75.0,69.0,97.0,92.0,92.0,85.0,83.0,118.0,84.0,146.0,93.0,97.0,93.0,62.0,98.0,92.0,22.0,115.0,88.0,87.0,86.0,94.0,105.0,88.0,93.0,98.0,88.0,93.0,81.0,31.0,83.0,38.0,108.0,90.0,97.0,81.0,70.0,62.0,104.0,97.0,99.0,96.0,84.0,112.0,90.0,88.0,89.0,24.0,100.0,116.0,101.0,92.0,79.0,72.0,96.0,92.0,61.0,88.0,88.0,113.0,67.0,65.0,62.0,107.0,69.0,77.0,98.0,71.0,101.0,64.0,125.0,125.0,59.0,91.0,107.0,101.0,93.0,86.0,81.0,92.0,89.0,107.0,94.0,110.0,81.0,77.0,64.0,111.0,97.0,77.0,94.0,94.0,91.0,93.0,95.0,80.0,95.0,90.0,73.0,108.0,25.0,105.0,100.0,96.0,97.0,73.0,132.0,96.0,92.0,99.0,78.0,87.0,89.0,60.0,91.0,83.0,102.0,111.0,88.0,93.0,29.0,88.0,107.0,91.0,180.0,75.0,92.0,90.0,114.0,80.0,99.0,90.0,72.0,71.0,77.0,70.0,73.0,107.0,81.0,86.0,128.0,45.0,46.0,112.0,87.0,87.0,106.0,100.0,101.0,117.0,114.0,94.0,100.0,96.0,87.0,81.0,96.0,52.0,114.0,105.0,157.0,80.0,97.0,105.0,96.0,60.0,93.0,99.0,109.0,96.0,102.0,107.0,60.0,86.0,163.0,98.0,101.0,76.0,95.0,127.0,86.0,116.0,82.0,102.0,88.0,109.0,75.0,23.0,83.0,97.0,62.0,84.0,90.0,82.0,96.0,65.0,90.0,93.0,91.0,100.0,101.0,74.0,44.0,43.0,95.0,94.0,68.0,72.0,92.0,107.0,108.0,74.0,55.0,24.0,101.0,29.0,78.0,73.0,72.0,60.0,47.0,70.0,120.0,74.0,57.0,72.0,91.0,62.0,85.0,85.0,90.0,105.0,59.0,137.0,86.0,135.0,64.0,72.0,83.0,82.0,90.0,80.0,130.0,62.0,90.0,92.0,155.0,78.0,92.0,60.0,69.0,92.0,143.0,78.0,84.0,78.0,93.0,84.0,58.0,82.0,83.0,48.0,73.0,103.0,81.0,75.0,68.0,71.0,71.0,94.0,80.0,78.0,72.0,93.0,105.0,130.0,100.0,44.0,80.0,72.0,75.0,26.0,84.0,120.0,76.0,95.0,96.0,94.0,94.0,93.0,96.0,94.0,94.0,93.0,95.0,93.0,75.0,103.0,84.0,29.0,104.0,81.0],"xaxis":"x","y":["a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a","a"],"yaxis":"y2","type":"scatter"}],                        {"barmode":"overlay","hovermode":"closest","legend":{"traceorder":"reversed"},"xaxis":{"anchor":"y2","domain":[0.0,1.0],"zeroline":false},"yaxis":{"anchor":"free","domain":[0.35,1],"position":0.0},"yaxis2":{"anchor":"x","domain":[0,0.25],"dtick":1,"showticklabels":false},"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"title":{"text":"Distplot with Normal Distribution"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('cd243f7b-88db-407d-a601-727d8d4c7295');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### What are the Top Categories?


```python
col = "listed_in"
categories = ", ".join(d2['listed_in']).split(", ")
counter_list = Counter(categories).most_common(50)
labels = [_[0] for _ in counter_list][::-1]
values = [_[1] for _ in counter_list][::-1]
trace1 = go.Bar(y=labels, x=values, orientation="h", name="TV Shows", marker=dict(color="#a678de"))

data = [trace1]
layout = go.Layout(title="Content added over the years", legend=dict(x=0.1, y=1.1, orientation="h"))
fig = go.Figure(data, layout=layout)
fig.show()
```


<div>                            <div id="c835a23a-8031-4119-994b-2339b7ef54bf" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c835a23a-8031-4119-994b-2339b7ef54bf")) {                    Plotly.newPlot(                        "c835a23a-8031-4119-994b-2339b7ef54bf",                        [{"marker":{"color":"#a678de"},"name":"TV Shows","orientation":"h","x":[19,40,40,41,50,57,110,158,181,223,233,292,301,329,348,509,521,948,1449,1610],"y":["Movies","Faith & Spirituality","Anime Features","Cult Movies","LGBTQ Movies","Classic Movies","Sports Movies","Sci-Fi & Fantasy","Music & Musicals","Horror Movies","Stand-Up Comedy","Children & Family Movies","Documentaries","Romantic Movies","Thrillers","Independent Movies","Action & Adventure","Comedies","Dramas","International Movies"],"type":"bar"}],                        {"legend":{"orientation":"h","x":0.1,"y":1.1},"title":{"text":"Content added over the years"},"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c835a23a-8031-4119-994b-2339b7ef54bf');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### Top Actors on Netflix with Most Movies


```python
def country_trace(country, flag = "movie"):
    df["from_us"] = df['country'].fillna("").apply(lambda x : 1 if country.lower() in x.lower() else 0)
    small = df[df["from_us"] == 1]
    if flag == "movie":
        small = small[small["duration"] != ""]
    else:
        small = small[small["season_count"] != ""]
    cast = ", ".join(small['cast'].fillna("")).split(", ")
    tags = Counter(cast).most_common(25)
    tags = [_ for _ in tags if "" != _[0]]

    labels, values = [_[0]+"  " for _ in tags], [_[1] for _ in tags]
    trace = go.Bar(y=labels[::-1], x=values[::-1], orientation="h", name="", marker=dict(color="#a678de"))
    return trace

from plotly.subplots import make_subplots
traces = []
titles = ["United States", "","India","", "United Kingdom", "Canada","", "Spain","", "Japan"]
for title in titles:
    if title != "":
        traces.append(country_trace(title))

fig = make_subplots(rows=2, cols=5, subplot_titles=titles)
fig.add_trace(traces[0], 1,1)
fig.add_trace(traces[1], 1,3)
fig.add_trace(traces[2], 1,5)
fig.add_trace(traces[3], 2,1)
fig.add_trace(traces[4], 2,3)
fig.add_trace(traces[5], 2,5)

fig.update_layout(height=1200, showlegend=False)
fig.show()
```


<div>                            <div id="523ab68a-1a49-41d8-984b-45e6799aee92" class="plotly-graph-div" style="height:1200px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("523ab68a-1a49-41d8-984b-45e6799aee92")) {                    Plotly.newPlot(                        "523ab68a-1a49-41d8-984b-45e6799aee92",                        [{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[8,9,9,9,9,9,9,9,9,9,10,10,10,10,10,11,11,12,12,12,12,12,13,13,13],"y":["Robert De Niro  ","Debi Derryberry  ","Tabitha St. Germain  ","Jim Gaffigan  ","Alfred Molina  ","John C. Reilly  ","Kathryn Hahn  ","Bruce Willis  ","Don Cheadle  ","Harvey Keitel  ","Danny Trejo  ","Tara Strong  ","Morgan Freeman  ","Halle Berry  ","Ron Perlman  ","Keanu Reeves  ","Samuel L. Jackson  ","Erin Fitzgerald  ","Fred Tatasciore  ","Kate Higgins  ","Adam Sandler  ","Molly Shannon  ","Laura Bailey  ","James Franco  ","Nicolas Cage  "],"type":"bar","xaxis":"x","yaxis":"y"},{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[11,11,11,11,12,12,12,12,12,12,12,12,12,13,14,14,15,17,17,18,19,20,22,27,28],"y":["Vijay Raaz  ","Rajpal Yadav  ","Aamir Khan  ","Nassar  ","Johnny Lever  ","Sachin Khedekar  ","Ajay Devgn  ","Nawazuddin Siddiqui  ","Jackie Shroff  ","Manoj Bajpayee  ","Saif Ali Khan  ","Raghuvir Yadav  ","Adil Hussain  ","Anil Kapoor  ","Kay Kay Menon  ","Amitabh Bachchan  ","Gulshan Grover  ","Kareena Kapoor  ","Naseeruddin Shah  ","Boman Irani  ","Paresh Rawal  ","Om Puri  ","Akshay Kumar  ","Shah Rukh Khan  ","Anupam Kher  "],"type":"bar","xaxis":"x3","yaxis":"y3"},{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[3,4,4,4,4,4,4,4,4,4,4,4,4,4,4,5,5,5,6,7,7,7,7,7,8],"y":["Andy Serkis  ","David Morrissey  ","Carol Cleveland  ","Colin Farrell  ","Jim Broadbent  ","Julie Walters  ","Ben Mendelsohn  ","Eric Bana  ","Ewan McGregor  ","Emily Watson  ","Kristin Scott Thomas  ","Michael Gambon  ","Ben Whishaw  ","Jamie Bell  ","Sam Spruell  ","Eddie Marsan  ","Jason Flemyng  ","James Cosmo  ","Graham Chapman  ","Michael Palin  ","Terry Jones  ","Terry Gilliam  ","Eric Idle  ","Samuel West  ","John Cleese  "],"type":"bar","xaxis":"x5","yaxis":"y5"},{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[3,3,3,3,3,3,3,3,3,3,3,4,4,4,4,4,4,4,4,4,5,7,8,10,10],"y":["Marc-Andr\u00e9 Grondin  ","Sarah Dunsworth  ","David DeLuise  ","Rebecca Shoichet  ","Andrea Libman  ","Kenneth Welsh  ","Jay Baruchel  ","Robert Forster  ","Sarah Fisher  ","Ryan Reynolds  ","Michael Kopsa  ","Cathy Weseluck  ","Pat Roach  ","Barrie Dunn  ","Lucy Decoutere  ","Alison Pill  ","Tabitha St. Germain  ","Ashleigh Ball  ","Tara Strong  ","Colm Feore  ","Patrick Roach  ","Mike Smith  ","John Dunsworth  ","John Paul Tremblay  ","Robb Wells  "],"type":"bar","xaxis":"x6","yaxis":"y6"},{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[3,3,3,4,4,4,4,4,4,4,4,4,4,5,5,5,5,5,5,6,6,6,7,7,8],"y":["Natalia de Molina  ","Kandido Uranga  ","Mariana Cordero  ","Carlos Areces  ","Luis Callejo  ","Jos\u00e9 Coronado  ","Pedro Casablanc  ","Joaqu\u00edn Climent  ","Macarena Garc\u00eda  ","Blanca Su\u00e1rez  ","Clara Lago  ","Elvira M\u00ednguez  ","B\u00e1rbara Lennie  ","Macarena G\u00f3mez  ","Javier Guti\u00e9rrez  ","Dani Rovira  ","Bel\u00e9n Cuesta  ","Alexandra Jim\u00e9nez  ","Secun de la Rosa  ","Emilio Guti\u00e9rrez Caba  ","Karra Elejalde  ","Alain Hern\u00e1ndez  ","Luis Tosar  ","Mario Casas  ","Carmen Machi  "],"type":"bar","xaxis":"x8","yaxis":"y8"},{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[3,3,3,4,4,4,4,4,4,4,4,4,4,4,4,5,5,5,5,5,6,6,7,7,8],"y":["Ikkyu Juku  ","Akira Ishida  ","Megumi Han  ","Showtaro Morikubo  ","Fumiko Orikasa  ","Kumiko Watanabe  ","Houko Kuwashima  ","Koji Tsujitani  ","Kappei Yamaguchi  ","Akio Otsuka  ","Kenta Miyake  ","Daisuke Ono  ","Maaya Sakamoto  ","Saori Hayami  ","Kana Hanazawa  ","Rikiya Koyama  ","Satsuki Yukino  ","Kazuya Nakai  ","Minako Kotobuki  ","Aki Toyosaki  ","Kazuhiko Inoue  ","Takahiro Sakurai  ","Chie Nakamura  ","Junko Takeuchi  ","Yuki Kaji  "],"type":"bar","xaxis":"x10","yaxis":"y10"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,0.16799999999999998]},"yaxis":{"anchor":"x","domain":[0.625,1.0]},"xaxis2":{"anchor":"y2","domain":[0.208,0.376]},"yaxis2":{"anchor":"x2","domain":[0.625,1.0]},"xaxis3":{"anchor":"y3","domain":[0.416,0.584]},"yaxis3":{"anchor":"x3","domain":[0.625,1.0]},"xaxis4":{"anchor":"y4","domain":[0.624,0.792]},"yaxis4":{"anchor":"x4","domain":[0.625,1.0]},"xaxis5":{"anchor":"y5","domain":[0.832,1.0]},"yaxis5":{"anchor":"x5","domain":[0.625,1.0]},"xaxis6":{"anchor":"y6","domain":[0.0,0.16799999999999998]},"yaxis6":{"anchor":"x6","domain":[0.0,0.375]},"xaxis7":{"anchor":"y7","domain":[0.208,0.376]},"yaxis7":{"anchor":"x7","domain":[0.0,0.375]},"xaxis8":{"anchor":"y8","domain":[0.416,0.584]},"yaxis8":{"anchor":"x8","domain":[0.0,0.375]},"xaxis9":{"anchor":"y9","domain":[0.624,0.792]},"yaxis9":{"anchor":"x9","domain":[0.0,0.375]},"xaxis10":{"anchor":"y10","domain":[0.832,1.0]},"yaxis10":{"anchor":"x10","domain":[0.0,0.375]},"annotations":[{"font":{"size":16},"showarrow":false,"text":"United States","x":0.08399999999999999,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"India","x":0.5,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"United Kingdom","x":0.9159999999999999,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"Canada","x":0.08399999999999999,"xanchor":"center","xref":"paper","y":0.375,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"Spain","x":0.5,"xanchor":"center","xref":"paper","y":0.375,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"Japan","x":0.9159999999999999,"xanchor":"center","xref":"paper","y":0.375,"yanchor":"bottom","yref":"paper"}],"height":1200,"showlegend":false},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('523ab68a-1a49-41d8-984b-45e6799aee92');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### Top Actors on Netflix with Most TV Shows


```python
traces = []
titles = ["United States","", "United Kingdom"]
for title in titles:
    if title != "":
        traces.append(country_trace(title, flag="tv_shows"))

fig = make_subplots(rows=1, cols=3, subplot_titles=titles)
fig.add_trace(traces[0], 1,1)
fig.add_trace(traces[1], 1,3)

fig.update_layout(height=600, showlegend=False)
fig.show()
```


<div>                            <div id="c192990d-c163-402e-9c45-5ebfbea908bd" class="plotly-graph-div" style="height:600px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c192990d-c163-402e-9c45-5ebfbea908bd")) {                    Plotly.newPlot(                        "c192990d-c163-402e-9c45-5ebfbea908bd",                        [{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,2],"y":["Eileen Moreno  ","Juan Pablo Gamboa  ","Ramiro Meneses  ","Juli\u00e1n Rom\u00e1n  ","Ana Serradilla  ","Kim Jong-soo  ","Jung Suk-won  ","Heo Jun-ho  ","Kim Hye-jun  ","Jeon Seok-ho  ","Kim Sung-kyu  ","Kim Sang-ho  ","Bae Doona  ","Ryu Seung-ryong  ","Ju Ji-hoon  ","James Parks  ","Bruce Dern  ","Michael Madsen  ","Tim Roth  ","Demi\u00e1n Bichir  ","Walton Goggins  ","Jennifer Jason Leigh  ","Kurt Russell  ","Samuel L. Jackson  ","Dave Chappelle  "],"type":"bar","xaxis":"x","yaxis":"y"},{"marker":{"color":"#a678de"},"name":"","orientation":"h","x":[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,2,2,3],"y":["Rob Brydon  ","James Acaster  ","Jason Watkins  ","Daniel Rigby  ","Rosamund Pike  ","Craig Parkinson  ","Rory Kinnear  ","Daniel Kaluuya  ","Miles Jupp  ","Freddie Fox  ","James Faulkner  ","Taron Egerton  ","Anne-Marie Duff  ","Mackenzie Crook  ","Olivia Colman  ","Peter Capaldi  ","Gemma Arterton  ","Tom Wilkinson  ","Ben Kingsley  ","John Boyega  ","Nicholas Hoult  ","James McAvoy  ","Bertie Carvel  ","Lee Ingleby  ","David Attenborough  "],"type":"bar","xaxis":"x3","yaxis":"y3"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,0.2888888888888889]},"yaxis":{"anchor":"x","domain":[0.0,1.0]},"xaxis2":{"anchor":"y2","domain":[0.35555555555555557,0.6444444444444445]},"yaxis2":{"anchor":"x2","domain":[0.0,1.0]},"xaxis3":{"anchor":"y3","domain":[0.7111111111111111,1.0]},"yaxis3":{"anchor":"x3","domain":[0.0,1.0]},"annotations":[{"font":{"size":16},"showarrow":false,"text":"United States","x":0.14444444444444446,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"United Kingdom","x":0.8555555555555556,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"}],"height":600,"showlegend":false},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c192990d-c163-402e-9c45-5ebfbea908bd');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### US Directors with the most content


```python
small = df[df["type"] == "Movie"]
small = small[small["country"] == "United States"]

col = "director"
categories = ", ".join(small[col].fillna("")).split(", ")
counter_list = Counter(categories).most_common(12)
counter_list = [_ for _ in counter_list if _[0] != ""]
labels = [_[0] for _ in counter_list][::-1]
values = [_[1] for _ in counter_list][::-1]
trace1 = go.Bar(y=labels, x=values, orientation="h", name="TV Shows", marker=dict(color="orange"))

data = [trace1]
layout = go.Layout(title="Movie Directors from the United States with the most content", legend=dict(x=0.1, y=1.1, orientation="h"))
fig = go.Figure(data, layout=layout)
fig.show()
```


<div>                            <div id="f4da922a-1d5c-41c1-974e-3a009bb12515" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("f4da922a-1d5c-41c1-974e-3a009bb12515")) {                    Plotly.newPlot(                        "f4da922a-1d5c-41c1-974e-3a009bb12515",                        [{"marker":{"color":"orange"},"name":"TV Shows","orientation":"h","x":[5,5,5,5,6,6,7,7,8,12,12,14],"y":["Robert Rodriguez","Troy Miller","Steven Soderbergh","Noah Baumbach","Lance Bangs","Leslie Small","Ryan Polito","Martin Scorsese","Shannon Hartman","Marcus Raboy","Jay Chapman","Jay Karas"],"type":"bar"}],                        {"legend":{"orientation":"h","x":0.1,"y":1.1},"title":{"text":"Movie Directors from the United States with the most content"},"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('f4da922a-1d5c-41c1-974e-3a009bb12515');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### Stand Up Comedy in US


```python
tag = "Stand-Up Comedy"
df["relevant"] = df['listed_in'].fillna("").apply(lambda x : 1 if tag.lower() in x.lower() else 0)
small = df[df["relevant"] == 1]
small[small["country"] == "United States"][["title", "country","release_year"]].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>country</th>
      <th>release_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28</th>
      <td>Mike Birbiglia: The New One</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Iliza Shlesinger: Unveiled</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Jeff Dunham: All Over the Map</td>
      <td>United States</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>136</th>
      <td>Jeff Garlin: Our Man In Chicago</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Seth Meyers: Lobby Baby</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>304</th>
      <td>Arsenio Hall: Smart &amp; Classy</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>342</th>
      <td>Jenny Slate: Stage Fright</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>411</th>
      <td>Deon Cole: Cole Hearted</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>480</th>
      <td>Nikki Glaser: Bangin’</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>545</th>
      <td>Jeff Dunham: Beside Himself</td>
      <td>United States</td>
      <td>2019</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```


```python

```
