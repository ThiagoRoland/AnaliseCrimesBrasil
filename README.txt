Projeto realizado por Thiago Limeira Mendes Roland, com o intuito de servir como primeira aplicação de Machine Learning na prática, inicialmente utilizando o algoritmo de regressão linear, por se tratar de um algoritmo tanto "simples" quanto de amplas possibilidades de uso. Também possui como intuito a apresentação dos dados em formato de dashboards, com informações e insights úteis obtidos através da análise exploratória dos dados feita em Power BI.

Projeto iniciado em 09/02/2026.

Para iniciar, tive de pensar em quais seriam os dados que utilizaria para este projeto. Decidi que utilizaria dados de segurança pública dos municípios brasileiros, e para tanto utilizei os dados achados em https://www.kaggle.com/datasets/jadsonchagas/raw-data-on-brazilian-violence-indicators , que por sua vez vieram de https://dados.mj.gov.br/dataset/sistema-nacional-de-estatisticas-de-seguranca-publica

Os dados compreendem o número total de vítimas brasileiras de diversos crimes não especificados (mas dentre eles, homicídios, estupros, roubos, latrocínios, feminicídio, etc), em todos os municípios brasileiros. 

Meu plano consistiu em dividir a análise em duas partes: Uma voltada para o passado, para os dados históricos, feita e demonstrada no Power BI, e uma voltada para o futuro, para previsões que possam ser feitas sobre o futuro próximo (2 anos após o término dos dados), feita com Python e bibliotecas tais como pandas, numpy, sklearn, matplotlib, etc.

Primeiro, realizei uma análise dos dados históricos criando diversos gráficos com diversas informações que podem ser extraídas através dos dados, tais como: Números de vítimas totais, estados mais violentos, número de vítimas por região brasileira, número de vítimas por mês e ano, etc. Tudo se encontra no arquivo AnaliseCrimes.pbix que pode ser aberto com o Power BI.

Em seguida, parti para o Python para tentar fazer previsões a partir dos dados. Como este é meu primeiro projeto em que utilizo Machine Learning, quis começar por um algoritmo mais "básico" e, em tese, mais fácil de ser utilizado, o algoritmo de regressão linear.

Após alguns testes com ele, ficou demonstrado, no arquivo PrevVitimasPython, que o algoritmo de regressão linear não explica e nem prevê muito bem os dados, visto que estes possuem um grande sazonalidade/curvas bruscas que não permitem o estabelecimento de identificação de "padrões" nos dados. Seu R2 ficou muito baixo, indicando pouca confiabilidade nas suas previsões. Então, com a ajuda da IA, pesquisei por outros algoritmos que poderiam funcionar melhor para o meu caso, e esta me sugeriu o algoritmo Prophet, desenvolvido pela Meta.

O algoritmo Prophet é bem mais robusto para análise de séries temporais irregulares do que o algoritmo de regressão linear. Seus resultados imediatos já foram bem melhores, à primeira vista, do que os resultados anteriores, porém ainda insatisfatórios. Conversei mais um pouco com a IA e esta me sugeriu investigar melhor os dados, pois havia ali uma alta sazonalidade que dificultava a identificação de padrões.

Após análises tanto como Python quanto com o Power BI, percebi algo: Haviam muitas cidades, no total 459 (fiz cálculos posteriores a este momento), em que o número total de vítimas, de 2018 a 2022, era 0, e isto para os vários crimes mencionados no começo deste documento. Como a probabilidade disto ser verídico é muito baixa (ainda que existam de fato cidades com pouquíssimas pessoas no Brasil, é improvável que absolutamente nenhum crime tenha ocorrido em 459 cidades no período de 5 anos), decidi averiguar com a IA e busquei informações sobre crimes ocorridos em algumas das cidades mencionadas como "perfeitas". Em pouco tempo de pesquisa, já descobri que:

Cidade,Total de Ocorrências (Estimativa SSP),Principal Natureza do Crime
Águas da Prata,~480 a 550,Furtos e Lesão Corporal
Águas de São Pedro,~320 a 380,Furtos (especialmente em alta temporada)
Alambari,~250 a 310,Furtos e Crimes de Trânsito

Utilizando somente estas três cidades como exemplo, é evidente que é impossível que todas estas cidades de fato tenham sofrido de 0 crimes em todo este tempo, portanto só pude chegar a uma conclusão: Há dados faltantes nesta base de dados, o que demonstra o desafio constante da qualidade dos dados e que me foi útil visto que nunca antes havia trabalhado antes dados ausentes.

Ao averiguar tal, criei funções DAX no Power BI tanto para descobrir o número total de municípios com prováveis dados faltantes como também averiguar quais são os estados com mais dados faltantes. A partir desta descoberta, algo tornou-se evidente: Eu precisaria tratar/fatiar os dados caso realmente quisesse obter uma maior taxa de confiabilidade em minhas previsões, visto os outliers (de valores 0, por causa dos dados faltantes) serem extremos.

A partir disso já comecei a trabalhar com o tratamento e remoção de alguns dados. De imediato os resultados obtidos com o algoritmo Prophet melhoraram muito: De 35% de erro percentual, despencou para apenas cerca de 3%. A previsão se tornou muito mais confiável e com valores muito mais realistas, levando em conta os dados históricos.

Além do algoritmo Prophet, ainda utilizei o algoritmo XGBoost, por sugestão da IA, para tentar gerar mais previsões confiáveis, e este algoritmo teve um desempenho quase tão bom quanto o Prophet, ambos conseguiram atingir escalas de confiabilidade excelentes. Gerei gráficos em todos os algoritmos utilizados e, para finalizar, calculei os números brutos com o melhor algoritmo para este caso, o Prophet, de previsão para os próximos dois anos. No fim das contas, ao invés de utilizar apenas um algoritmo, que era meu plano inicial, para tentar resolver o problema acabei utilizando três, e me encontrei obrigado a tratar os dados, coisa que nunca tinha feito antes.

Para finalizar, todos os resultados e informações se encontram nos arquivos presentes, sendo indicadoressegurancapublica o arquivo inicial obtido no Kaggle, em formato do Excel e não tratado, e dados_seguranca_final o mesmo arquivo, porém pronto para análises e edições tanto via Power Bi quanto Python.

## 🛠️ Tecnologias Utilizadas
* Power BI: BI, DAX e Análise Exploratória (EDA).
* Python: Pandas, NumPy, Scikit-Learn, XGBoost e Prophet.
* Matplotlib: Visualização de tendências e previsões.

## 📈 Resultados do Modelo (Prophet)
* R²: 0.86
* MAE: 107 vítimas (Erro médio mensal)
* MAPE: 3.11% (Erro percentual)