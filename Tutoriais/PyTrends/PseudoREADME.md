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

### Métodos da API

Antes de utlizar qualquer um dos métodos da API, precisamos criar o que chamaremos aqui de solicitação de pesquisa.

```
pytrends.build_payload(kw_list, cat=0, timeframe='today 12-m', geo='', gprop='') #construimos a solicitação de pesquisa 
```

#### 1. Interesse ao longo do tempo

Retorna dados históricos indexados de quando a palavra-chave foi mais pesquisada, conforme mostrado na seção Interesse ao longo do tempo do Google Trends.



#### 2. Histórico de interesse por hora

Retorna dados históricos, indexados e por hora para quando a palavra-chave foi mais pesquisada, conforme mostrado na seção Interesse ao longo do tempo do Google Trends. Ele envia várias solicitações ao Google, cada uma recuperando uma semana de dados por hora. Parece que essa seria a única maneira de obter dados históricos por hora

#### 3. Interesse por região 

Retorna os dados de onde a palavra-chave é mais pesquisada, conforme mostrado na seção Interesse por região do Google Trends.

#### 4. Tópicos relacionados

Retorna dados para as palavras-chave relacionadas a uma palavra-chave fornecida mostrada na seção Tópicos relacionados do Google Trends.

#### 5. Consultas relacionadas

Retorna dados para as palavras-chave relacionadas a uma palavra-chave fornecida mostrada na seção Consultas relacionadas do Google Trends.

#### 6. Tendências de pesquisas

Retorna os dados das últimas tendências de pesquisas mostradas na seção Tendências de pesquisas do Google Trends

#### 7. Sugestões

Retorna uma lista de palavras-chave sugeridas adicionais que podem ser usadas para refinar uma pesquisa de tendência. 

