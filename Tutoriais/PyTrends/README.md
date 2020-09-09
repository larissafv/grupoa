# Tutorial API Pytrends

Esse tutorial tem a finalidade de instruir os leitores quanto a utilização da API Pytrends. O Pytrends promove uma interface simples que busca auxiliar e automatizar a coleta de grandes volumes de dados do Google Trends. 

Introduzimos então de forma didática as nuances da utilização da plataforma Google Trends e da API Pytrends para o nosso projeto. 
Esse tutorial tem como base a documentação da [Pseudo API para o  Google Trends](https://pypi.org/project/pytrends/).

### Instalação e Importação

É necessário instalar a biblioteca pytrends diretamente com o código a seguir. O código deve ser executado no terminal do seu sistema operacional (exemplo: CMD do Windows), ou em uma célula de um arquivo .ipynb (Jupyter Notebook), com um ponto de exclamação no início. 
```
pip install pytrends
```

Para evitar erros, indicamos instalar também as bibliotecas Pandas e Requests. Estas são instaladas de maneira análoga, substituindo o termo "pytrends" pelo nome da biblioteca (com todos os caracteres minúsculos).

A importação da biblioteca python se faz por:

```
from pytrends.request import TrendReq
```

### Se Conectando ao Google

Para nos conectar ao server da Google utilizamos a função:

```
pytrends = TrendReq(hl='pt-br', tz=3)
```

Parâmetro | Descrição | Default Value | Definição 
:-------: | :-------: | :-------: | :-------:
hl | Refere-se à "host language" de acesso ao Google Trends | **Deve ser especificado** | Português do Brasil
tz | Refere-se à "time zone" de acesso| **Deve ser especificado** | UTC-3 como fuso horário de Brasília

_Nota: Os limites de taxa definem o número de solicitações máximas que podem ser feitas ao Google. É possível exceder essa taxa ao solicitar muitas pesquisas e existe uma função complementar à citada que estabelece conexões de forma automática. Porém, se o limite for excedido, um tempo de espera (backoff factor) de 1 minuto entre novas chamadas da API permite realizar outras pesquisas. Portanto, essa função adicional não será apresentada aqui._

### Métodos da API

Antes de utilizar qualquer um dos métodos da API, precisamos criar o que chamaremos aqui de **solicitação de pesquisa**.

```
pytrends.build_payload(kw_list, cat=0, timeframe='today 12-m', geo='', gprop='') 
```

Essa função é análoga à pesquisa direta na plataforma Google Trends.

Parâmetro | Descrição | Default Value | Definição 
:-------: | :-------: | :-------: | :-------:
kw_list* | Lista de termos para pesquisa | **Deve ser especificado** | -
cat | Categoria para limitar resultados | Sem categoria | All categories: 0
timeframe | Período de pesquisa | Last 5 years (today 5-y) | Por Mês, Dia ou Hora ou data específica '2019-12-01 2020-01-01'
geo | Define pesquisa para Países e Estados específicos | World | BR, BR-MG etc.
gprop | Propriedade do Google | Web Searches | images, news, youtube etc.


_* Devem ser especificados os termos de pesquisa, porém existem considerações._

   _O Google Trends pode ser usado para a realização de pesquisa de interesse de palavras em conjunto. Porém, os resultados númericos serão diferentes dos apresentados na pesquisa de interesse de palavras individuais._ 
   
   _Em uma pesquisa de termo único, os resultados se encontram numa escala de 0 a 100 sendo que 0 corresponde ao período de menos interesse e 100 corresponde a período de mais interesse no termo, dentro do intervalo de tempo especificado. Ao realizar uma pesquisa conjunta de termos, a escala se altera. O termo de menor interesse no período de menor procura tomará o valor de 0 e o termo de maior interesse no período de maior procura tomará o valor de 100 na escala._
   
   _Avaliamos preferencialmente os termos individuais pelo fato de que assim, podemos observar melhor o comportamento do interesse temporal. Portanto, a lista do parâmetro 'kw_list' deve conter preferencialmente apenas um elemento._
   

#### 1. Interesse ao longo do tempo

Retorna dados históricos indexados de quando a palavra-chave foi mais pesquisada, conforme mostrado na seção Interesse ao longo do tempo do Google Trends.

```
pytrends.interest_over_time()
```

O método retorna um DataFrame indexado pelas datas e com o valor numérico de interesse de cada termo contida em 'kw_list'.

#### 2. Histórico de interesse por hora

Retorna dados históricos, indexados por data e por hora para quando a palavra-chave foi mais pesquisada, conforme mostrado na seção Interesse ao longo do tempo do Google Trends. Ele envia várias solicitações ao Google, cada uma recuperando uma semana de dados por hora. Parece que essa seria a única maneira de obter dados históricos por hora.

```
pytrends.get_historical_interest(kw_list, year_start=2020, month_start=9, day_start=1, hour_start=0, year_end=2020, month_end=9, day_end=2, hour_end=0, cat=0, geo='BR', gprop='', sleep=0)
```

O método retorna um DataFrame indexado por data e hora e com o valor numérico de interesse de cada termo contido em 'kw_list'.

Parâmetro | Descrição | Default Value | Definição 
:-------: | :-------: | :-------: | :-------:
kw_list | Lista de termos para pesquisa | **Deve ser especificado** | -
(year/month/day/hour)_start | data e hora do ponto de inicio período de pequisa | **Deve ser especificado** | -
(year/month/day/hour)_end | data e hora do ponto de término do período de pesquisa | **Deve ser especificado** | -
cat | Categoria para limitar resultados | Sem categoria | All categories: 0 | 
geo | Define pesquisa para Países e Estados específicos | World | BR, BR-MG etc.
gprop | Propriedade do Google | Web Searches | images, news, youtube etc.
sleep | backoff factor - deve ser especificado se for excedida a taxa limite de pesquisas | 0 | segundos: 0 ou 60


#### 3. Interesse por região 

Retorna os dados de onde a palavra-chave é mais pesquisada, conforme mostrado na seção Interesse por região do Google Trends.


```
pytrends.interest_by_region(resolution='COUNTRY', inc_low_vol=True, inc_geo_code=True)
```

O metodo retorna um DataFrame indexado pelos países, estados, ou cidades solicitados referenciandando o valor numérico do interesse de cada termo.

Parâmetro | Descrição | Default Value | Definição 
:-------: | :-------: | :-------: | :-------:
resolution | Define o nível da localidade geográfica | COUNTRY | COUNTRY ou CITY ou REGION
inc_low_vol | Inclui região com baixo número de pesquisas | False | True ou False
inc_geo_code | Inclui os códigos de referência geográfica usados pela Google | False | True ou False


#### 4. Tópicos relacionados

Retorna dados para as palavras-chave relacionadas a uma palavra-chave fornecida mostrada na seção Assuntos relacionados do Google Trends.

```
pytrends.related_topics()
```

O método retorna um dicionário de DataFrames.

#### 5. Consultas relacionadas

Retorna dados para as palavras-chave relacionadas a uma palavra-chave fornecida mostrada na seção Consultas relacionadas do Google Trends.

```
pytrends.related_queries()
```
O método retorna um dicionário de DataFrames.

#### 6. Tendências de pesquisas

Retorna os dados das últimas tendências em tempo real de pesquisas mostradas na seção Tendências de pesquisas do Google Trends.

```
pytrends.trending_searches(pn='brazil')
```

O método retorna um  DataFrame.

Parâmetro | Descrição | Default Value | Definição 
:-------: | :-------: | :-------: | :-------:
pn | Define um país para pesquisa | **Deve ser especificado** | brazil, united_states, japan

#### 7. Sugestões

Retorna uma lista de palavras-chave sugeridas adicionais que podem ser usadas para refinar uma pesquisa de tendência. 

```
pytrends.suggestions(keyword)
```

O método retorna um dicionário.

O parâmetro keyword é uma string qualquer. Esse termo não precisa ter qualquer relação com a lista de termos espcificada na solicitação de pesquisa. 
