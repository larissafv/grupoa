# Tutorial API Pytrends

Esse tutorial tem a finalidade de instruir os leitores quanto a utilização da API Pytrends. O Pytrends promove uma interface simples que busca auxiliar e automatizar a coleta de grandes volumes de dados do Google Trends. 

Introduzimos então de forma didática as nuances da utilização da plataforma Google Trends e da API Pytrends para o nosso projeto. 
Esse tutorial tem como base a documentação da [Pseudo API para o  Google Trends](https://pypi.org/project/pytrends/#related-queries).

### Instalação e Importação

É necessário instalar a biblioteca pytrends diretamente com o código
```
pip install pytrends
```

Além das bibliotecas Pandas e Requests.

A importação da biblioteca python se faz por

```
from pytrends.request import TrendReq
```

### Se Conectando ao Google

Para nos conectar ao server da Google utilizamos a função

```
pytrends = TrendReq(hl='pt-br', tz=3)
```

Parêmetro | Descrição | Default Value | Definição 
:-------: | :-------: | :-------: | :-------:
hl | Refere-se à "host language" de acesso ao Google Trends | **Deve ser especificado** | Portugês do Brasil
tz | Refere-se à "time zone" de acesso| **Deve ser especificado** | UTC-3 como fuso horário de Brasília

_Nota: Os limites de taxa definem o número de solicitações que podem ser feitas para o Google. É possível exceder essa taxa ao solicitar muitas pesquisas e existe uma função complementar à citada que estabelece conexões de forma automática. Porém se o limite exceder, um tempo de espera (backoff factor) de 1 minuto já permite realizar outras pesquisas. Portanto, essa função complementar não será apresentada aqui._

### Métodos da API

Antes de utilizar qualquer um dos métodos da API, precisamos criar o que chamaremos aqui de solicitação de pesquisa.

```
pytrends.build_payload(kw_list, cat=0, timeframe='today 12-m', geo='', gprop='') 
```

Essa função é análoga à pesquisa direta na plataforma Google Trends.

Parêmetro | Descrição | Default Value | Definição 
:-------: | :-------: | :-------: | :-------:
kw_list* | Lista de termos para pesquisa | **Deve ser especificado** | -
cat | Categoria para limitar resultados | Sem categoria | All categories: 0
timeframe | Período de pesquisa | Last 5 years (today 5-y) | Por Mês, Dia ou Hora ou data específica '2019-12-01 2020-01-01'
geo | Define pesquisa para Países e Estados específicos | World | BR, BR-MG etc.
gprop | Propriedade do Google | Web Searches | images, news, youtube etc.


_* Deve especificar os termos de pesquisa, porém existem considerações._

   _O Google Trends pode ser usado para a realização de pesquisa de interesse de palavras em conjunto. Porém os resultados não serão apresentados na mesma escala da pesquisa de interesse de palavras individuais._ 
   
   _Em uma pesquisa de termo individual, os resultados se encontram numa escala de 0 a 100 sendo que 0 corresponde ao período de menos interesse e 100 corresponde a período de mais interesse do termo, dentro do intervalo de tempo especificado. Ao realizar uma pesquisa conjunta de termos, a escala se altera. O termo de menor interesse no período de menor procura tomará o valor de 0 e o termo de maior interesse no período de maior procura tomará o valor de 100 na escala. Portanto, os resultados numéricos serão diferentes em cada caso._
   
   _Avaliamos preferencialmente os termos individuais pelo fato de que assim, podemos observar melhor o comportamento do interesse temporal. Portanto a lista do parâmetro 'kw_list' deve conter preferencialmente apenas um elemento._
   

#### 1. Interesse ao longo do tempo

Retorna dados históricos indexados de quando a palavra-chave foi mais pesquisada, conforme mostrado na seção Interesse ao longo do tempo do Google Trends.

Para retornarmos o interesse ao longo do tempo dos dados recebidos pela função da solicitação de pesquisa, entraremos com a seguinte função:

```
pytrends.interest_over_time()
```

O retorno do método é um DataFrame indexado pelas datas e com o valor numérico de interesse de cada palavra contida em 'kw_list'.

#### 2. Histórico de interesse por hora

Retorna dados históricos, indexados por data e por hora para quando a palavra-chave foi mais pesquisada, conforme mostrado na seção Interesse ao longo do tempo do Google Trends. Ele envia várias solicitações ao Google, cada uma recuperando uma semana de dados por hora. Parece que essa seria a única maneira de obter dados históricos por hora.


O interesse histórico por hora pode ser adquirido com a função:

```
pytrends.get_historical_interest(kw_list, year_start=2020, month_start=9, day_start=1, hour_start=0, year_end=2020, month_end=9, day_end=2, hour_end=0, cat=0, geo='BR', gprop='', sleep=0)
```


#### 3. Interesse por região 

Retorna os dados de onde a palavra-chave é mais pesquisada, conforme mostrado na seção Interesse por região do Google Trends.

Os dados do interesse por região é dado pela função:

```
pytrends.interest_by_region(resolution='COUNTRY', inc_low_vol=True, inc_geo_code=True)
```

É importante notar que a escala de valores numéricos por região segue a mesma lógica escalar de 0 a 100 do interesse ao longo do tempo. Porém são consideradas as pesquisas por termos durante todo o período de tempo especificado pelo parâmetro 'timeframe' na solicitação pesquisa.

#### 4. Tópicos relacionados

Retorna dados para as palavras-chave relacionadas a uma palavra-chave fornecida mostrada na seção Tópicos relacionados do Google Trends.

#### 5. Consultas relacionadas

Retorna dados para as palavras-chave relacionadas a uma palavra-chave fornecida mostrada na seção Consultas relacionadas do Google Trends.

#### 6. Tendências de pesquisas

Retorna os dados das últimas tendências de pesquisas mostradas na seção Tendências de pesquisas do Google Trends

#### 7. Sugestões

Retorna uma lista de palavras-chave sugeridas adicionais que podem ser usadas para refinar uma pesquisa de tendência. 

