# Grupo A: Covid Data Analytics
Este repositório é um trabalho desenvolvido pelo Grupo A do projeto Covid Data Analytics (veja mais em covid.dcc.ufmg.br). Aqui, são desenvolvidas visualizações a partir de bases de dados relativos à evolução temporal e localizada da pandemia de COVID-19 no Brasil. No momento, os trabalhos desenvolvidos consistem na extração, tratamento e projeção gráfica de dados estruturados. 
## Bases de Dados
Aqui, trabalhamos incialmente com dois arquivos extraídos em formato CSV: "caso_full.csv", disponível no site [brasil.io](https://brasil.io/dataset/covid19/caso_full/), e "pop2020.csv". Nessas bases, há informações sobre indicadores básicos acerca da pandemia situados por município e semana epidemiológica.
## Tratamento
O tratamento dos dados é realizado em um caderno Jupyter, que pode ser acessado para uma compreensão passo a passo do processo. A partir dos indicadores originais disponíveis nas bases de dados (casos e óbitos por semana e acumulados), são calculados: prevalência; mortalidade; letalidade; incidência de novos casos e óbitos; fator de crescimento de casos e óbitos.

Nesse momento, vale ponderar que diversos dados podem ser inconsistentes com a realidade, tendo em vista o número de casos subnotificados, bem como dados que podem não ser devidamente encaminhados, registrados ou atualizados pelas Secretarias Estaduais de Saúde.

Como a maioria das visualizações a serem produzidas vão trabalhar com semanas epidemiológias, os dados atribuídos a cada semana são do último dia. Após serem reorganizadas e acrescidas de colunas contendo os novos indicadores, as bases de dados são exportadas para o arquivo **indicadores.db**.
## Visualização
As visualizações são projetadas utilizando a biblioteca MatPlotLib, também em um caderno Jupyter disponível nesse repositório, com base nos dados já tratados. Foram projetados gráficos e mapas que distinguem a evolução da pandemia no país conforme cada região ao longo do tempo.

![novos_casos](https://user-images.githubusercontent.com/58361234/92538919-640d0a80-f216-11ea-9fb9-70508a728745.png)
![letalidade](https://user-images.githubusercontent.com/58361234/92538918-63747400-f216-11ea-940f-911e9e7925fc.png)

Com base nos gráficos obtidos, é possível abstrair tendências e deduzir motivos para, por exemplo: um maior índice de letalidade na macrorregião Norte em contraste com a baixa incidência da região; os elevados números de casos acumulados nas regiões Nordeste e Sudeste; a maior taxa de óbitos no Sudeste com relação ao Nordeste, enquanto ambas apresentam índices semelhantes de casos acumulados. Os demais gráficos gerados estão na pasta "Gráficos", subpasta de "Visualização dos Indicadores".

A disparidade da pandemia em cada região fica ainda mais visível nos mapas, produzidos utilizando a biblioteca GeoPandas.

![mapa_letalidade_macrorregioes](https://user-images.githubusercontent.com/58361234/92549600-071e4e00-f230-11ea-8c57-6939b2a910a7.gif)

Mapas semelhantes a esse para demais indicadores e níveis de localidade estão armazenados na pasta "Mapas", subpasta de "Visualização dos Indicadores"

Todas essas análises podem levar a conclusões relevantes que ajudam a entender a pandemia enquanto um problema de relevância nacional. A compreensão de fenômenos como a interiorização da doença, por exemplo, é fundamental para agir estrategicamente na contenção da pandemia.
