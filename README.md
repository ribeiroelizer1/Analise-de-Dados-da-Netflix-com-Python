# Analise de Dados da Netflix com Python

Neste estudo, exploraremos uma base de dados da Netflix, contendo informações sobre séries e filmes de 2010 a 2021. O objetivo é responder a algumas perguntas-chave sobre o conteúdo da plataforma:

1 - Quais os tipos de conteúdos disponíveis?   
2 - Diretores com maior quantidade de conteúdos publicados  
3 - Atores com maior participação em filmes e series  
4 - Análise de conteúdo criado pela Netflix ao longo do tempo  


<h6>Preparando a base de dados</h6>

```
import pandas as pd
import plotly.express as px
from textblob import TextBlob
dff = pd.read_csv('netflixanalysis/netflix_titles.csv')
dff.shape
```
``` (8807, 12) ```


<h6>Analisando o Tipo de Conteúdo Mais Distribuído</h6>

``` 
z = dff.groupby(['rating']).size().reset_index(name='counts')
dados_ordenados = z.sort_values(by='counts', ascending=True)
barras = px.bar(dados_ordenados, x='counts', y='rating')
barras.show()
```
![newplot (2)](https://github.com/user-attachments/assets/9198c2ad-e96f-4fc8-95f4-89e00eb70ff4)


A análise revela que o conteúdo mais distribuído é o TV-MA, indicando uma predominância de material destinado ao público adulto.

<h6>Identificando os Diretores com Mais Publicações</h6>

```
dff['director']= dff['director'].fillna('No Director Specified') #Trocando os dados vazios da coluna
filtered_directors = pd.DataFrame() 
filtered_directors = dff['director'].str.split(',', expand=True).stack()
filtered_directors = filtered_directors.to_frame()
filtered_directors.columns= ['Director']
directors = filtered_directors.groupby(['Director']).size().reset_index(name = 'Total Content')
directors = directors[directors.Director != 'No Director Specified']
directors = directors.sort_values(by = ['Total Content'], ascending = False)
directorsTop5 = directors.head()
directorsTop5 = directorsTop5.sort_values(by = ['Total Content'])
fig1 = px.bar(directorsTop5, x='Total Content', y='Director', title= 'Top 5 Diretores na Netflix')
fig1.show()
```
![newplot (3)](https://github.com/user-attachments/assets/15c54979-6b79-41e9-8c57-ca4d621d8ae5)

<h6>Descobrindo os Atores com Maior Participação</h6>

```
dff['cast']=dff['cast'].fillna('No Cast Specified')
filtered_cast=pd.DataFrame()
filtered_cast=dff['cast'].str.split(',',expand=True).stack()
filtered_cast=filtered_cast.to_frame()
filtered_cast.columns=['Actor']
actors=filtered_cast.groupby(['Actor']).size().reset_index(name='Total Content')
actors=actors[actors.Actor !='No Cast Specified']
actors=actors.sort_values(by=['Total Content'],ascending=False)
actorsTop5=actors.head()
actorsTop5=actorsTop5.sort_values(by=['Total Content'])
fig2=px.bar(actorsTop5,x='Total Content',y='Actor', title='Top 5 Atores na Netflix')
fig2.show()
```
![newplot (5)](https://github.com/user-attachments/assets/2ded33c8-f535-4d84-9bcd-8580838b1c7b)

Por fim, podemos também ver qual a tendência de conteúdo ao longo dos anos.

```
df1=dff[['type','release_year']]
df1=df1.rename(columns={"release_year": "Release Year"})
df2=df1.groupby(['Release Year','type']).size().reset_index(name='Total Content')
df2=df2[df2['Release Year']>=2010]
fig3 = px.line(df2, x="Release Year", y="Total Content", color='type',title='Tendência de conteúdo produzido ao longo dos anos na Netflix')
fig3.show()
```
![newplot (6)](https://github.com/user-attachments/assets/db1d1d48-1634-4285-a98c-14bbd666d482)

