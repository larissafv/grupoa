# Tratamento de dados

## Resumo
O arquivo de tratamento de dados foi feito utilizando o Jupyter Notebook para o Python 3.8.2 64-bit e a biblioteca Pandas. Os dados tratados foram dois arquivos, o caso_full e pop2020, ambos salvos em csv. O primeiro arquivo é um banco de dados, do site “https://brasil.io/”, contendo os dados relativos aos casos e às mortes relacionados à COVID-19, separados por cidades e estados brasileiros. O segundo arquivo, pop2020, informa a população atual de todas as cidades brasileiras e foi cedido pela professora ………….

O objetivo desse Notebook foi filtrar os dados desses arquivos pelos itens de interesse para calcular os indicadores descritos abaixo. Para isso foi utilizado um período semanal, coincidindo com as semanas epidemiológicas, e os cálculos foram feitos para os níveis municipal, estadual, regional e nacional.

Para que o notebook execute as análises é preciso que os arquivos caso_full.csv e pop2020.csv estejam contidos na pasta "case_csv".

## Indicadores
Os indicadores foram estabelecidos pelos mentores do projeto e são eles:
  
  • Casos acumulados:
      Soma dos casos.
      
  • Óbitos acumulados:
      Soma dos óbitos.
  
  • Prevalência:
      Número de casos acumulados por 100 mil habitantes.
  
  • Mortalidade:
      Número de óbitos acumulados por 100 mil habitantes.
  
  • Letalidade:
      Número de óbitos acumulados dividido por número de casos acumulados.
  
  • Novos casos:
      Número de novos casos em uma determinada semana epidemiológica.
  
  • Novos óbitos:
      Número de novos óbitos em uma determinada semana epidemiológica.
  
  • Incidência de casos:
      Número de novos casos por 100 mil habitantes.
  
  • Incidência de óbitos:
      Número de novos casos por 100 mil habitantes.
  
  • Fator de crescimento dos casos:
      Número de novos casos em um período dividido pelo número do período anterior. 
  
  • Fator de crescimento dos óbitos:
      Número de novos óbitos em um período dividido pelo número do período anterior. 

## Metodologia de tratamento

O arquivo caso_full possui diversos dados sobre a situação das cidades e dos estados e até mesmo alguns indicadores já calculados, apesar de estarem um uma base diária e não semanal como a de interesse do estudo. Buscando uma maior precisão dos cálculos e para adequá-los às necessidades do projeto ficou decidido que seria aproveitado do arquivo apenas os dados referentes aos casos e óbitos acumulados, a identificação dos locais por meio do código IBGE para cidades e estados, o nome da localidade, a semana epidemiológica e a data do registro. Esses dados são suficientes para o cálculo de todos os indicadores desejados.

**Foram seguidos os seguintes passos para chegar aos indicadores:**

    1. Leitura dos dois arquivos mencionados como DataFrames (DF) do Pandas.
    2. Correção dos nomes das colunas para que ambos tenham a coluna do código IBGE com o mesmo cabeçalho, que será utilizado para juntar os dois DF, e para padronizar os nomes.
    3. Seleção apenas dos registros referentes ao último dia de cada semana epidemiológica. 
    4. Classificação dos estados por macro regiões, que será utilizado posteriormente para agrupar os dados e calcular os indicadores, para a análise a nível regional. 
    5. Correção dos nomes dos estados no DF gerado pelo arquivo pop2020 para ficar somente com siglas, como o arquivo caso_full. Essa correção é necessária para juntar os dados dos dois DF e para somar a população de cada estado, já que o arquivo contém apenas valores referentes às cidades. 
    6. Criação de dois DF diferentes, um com a população das cidades e outro com a população dos estados.
    7. Criação de outros dois DF, um para cidades e outro para estados. Cada DF seleciona apenas os registros referentes ao seu nível de análise e inclui os valores referentes às respectivas populações. 
    8. Aplicação de duas funções que calculam o valor de novos casos e novos óbitos em cada semana, subtraindo o valor acumulado da semana atual pela última semana. Foi utilizado esse método, pois se mostrou mais eficiente do que somar todos os novos casos de cada dia da semana, que são disponibilizados no arquivo caso_full.
    9. Criação de 4 DF contendo os valores de casos e óbitos acumulados e novos por semana epidemiológica. Cada DF representa um nível de análise: municipal, estadual, regional e nacional.
       Os 4 indicadores desses DF serão base para os demais.  
    10. Aplicação de uma função para cada DF calculando os demais indicadores.
    
    
## Resultado

O resultado contido nos dataframes foi armazenado no arquivo "indicadores.db", na pasta "resultados". Nesse arquivo, cada tabela corresponde a um dataframe criado, sendo mantida a mesma estrutura.


## Acesso

O acesso ao banco de dados pode ser feito diretamente por meio da aplicação SqLite Browser, que pode ser baixado gratuitamente [neste link](https://sqlitebrowser.org/dl/). Após a instalação, basta abrir o arquivo "indicadores.db" e navegar pela tabela desejada.

Para acessar pelo python, basta seguir as instruções contidas no final do notebook de tratamento de dados. No geral, as tabelas podem ser revertidas para Pandas Dataframes utilizando o seguinte código:
```python
import pandas as pd
import sqlite3

con = sqlite3.connect("resultados/indicadores.db")  

novo_dataframe = pd.read_sql('select * from cities_df', con)
```


Caso você não possua a biblioteca 'sqlite3' instalada em seu computador, basta executar o comando abaixo no prompt de comando (em muitos casos não é necessário). A documentação da biblioteca pode ser consultada [nesse link](https://docs.python.org/3/library/sqlite3.html).

```bash
pip install pysqlite3
```

       
