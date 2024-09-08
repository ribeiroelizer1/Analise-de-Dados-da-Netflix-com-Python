# Analise-de-Dados-da-Netflix-com-Python

Nesse estudo irei analisar uma base de dados da Netflix contendo informações sobre Series e filmes no período de 2010 a 2021 para responder as seguintes perguntas:

1 - Quais os tipos de conteúdos disponíveis?   
2 - Diretores com maior quantidade de conteúdos publicados  
3 - Atores com maior participação em filmes e series  
4 - Análise de conteúdo criado pela Netflix ao longo do tempo  


<h6>Preparando a base de dados</h6>

```
import pandas as pd
import numpy as np
import plotly.express as px
from textblob import TextBlob
dff = pd.read_csv('netflixanalysis/netflix_titles.csv')
dff.shape
```
``` (8807, 12) ```


<h6>Visualizando o tipo de conteúdo que mais é distribuído na plataforma</h6>

``` 
z = dff.groupby(['rating']).size().reset_index(name='counts')
pieChart = px.pie(z, values='counts', names='rating', 
                  title='Distribution of Content Ratings on Netflix',
                  color_discrete_sequence=px.colors.qualitative.Set3)
pieChart.show()
```
![newplot (1)](https://github.com/user-attachments/assets/e482fc0a-bbed-4f56-beec-0309e1e07ac5)

Aqui, vêmos que no período estudado, o o conteúdo com maior distribuição foi o TV-MA, que significa que o conteúdo da netflix é majoritáriamente destinado ao público adulto.

<h6>Conhecendo os Diretores que mais publicaram</h6>
