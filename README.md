<h1 align="center"> Análise de Dados: COVID-19 Dashboard</h1> 

<p>O intuito desse projeto é a criação de um dashboard interativo, com base em uma análise exploratória de dados de casos e mortes de COVID-19 entre 2021 e 2022, assim como o processo de vacinação no Brasil no mesmo periodo. O processamento de dados está neste <a href="https://www.kaggle.com/caiomarquesribeiro/covid-dashboard">link</a> e o dashboard, neste <a href="https://lookerstudio.google.com/reporting/13a73b4c-ee10-4774-8746-a22608a8839c">link</a>.</p>

<h2>Etapas do projeto</h2>

<h3>1. Introdução</h3>

<h4>1.1. Resumo</h4>

- Dashboard:
    - Google Data Studio <a href="https://lookerstudio.google.com/reporting/13a73b4c-ee10-4774-8746-a22608a8839c">(link)</a>.
- Processamento:
    - Kaggle Notebook <a href="https://www.kaggle.com/caiomarquesribeiro/covid-dashboard">(link)</a>.
- Fontes: 
    - Casos pela universidade John Hopkins <a href="https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_daily_reports">(link)</a>; 
    - Vacinação pela universidade de Oxford <a href="https://covid.ourworldindata.org/data/owid-covid-data.csv">(link)</a>.

<h4>1.2. Dados</h4>

<p>Os dados sobre <b>casos da COVID-19</b> são compilados pelo centro de ciência de sistemas e engenharia da universidade americana <b>John Hopkins</b> <a href="https://www.jhu.edu/">(link)</a>. Os dados são atualizados diariamente deste janeiro de 2020 com uma granularidade temporal de dias e geográfica de regiões de países (estados, condados, etc.). O website do projeto pode ser acessado neste <a href="https://systems.jhu.edu/research/public-health/ncov/">link</a> enquanto os dados, neste <a href="https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_daily_reports">link</a>. Abaixo estão descritos os dados derivados do seu processamento.</p>

- <b> date</b>: data de referência;
- <b> state</b>: estado;
- <b>country</b>: país;
- <b>population</b>: população estimada;
- <b>confirmed</b>: número acumulado de infectados;
- <b>confirmed_1d</b>: número diário de infectados;
- <b>confirmed_moving_avg_7d</b>: média móvel de 7 dias do número diário de infectados;
- <b>confirmed_moving_avg_7d_rate_14d</b>: média móvel de 7 dias dividido pela média móvel de 7 dias de 14 dias atrás;
- <b>deaths</b>: número acumulado de mortos;
- <b>deaths_1d</b>: número diário de mortos;
- <b>deaths_moving_avg_7d</b>: média móvel de 7 dias do número diário de mortos;
- <b>deaths_moving_avg_7d</b>: média móvel de 7 dias dividido pela média móvel de 7 dias de 14 dias atrás;
- <b>case_fatality_ratio</b>: taxa de fatalidade por número de casos;
- <b>month</b>: mês de referência;
- <b>year</b>: ano de referência.

<p>Os dados sobre <b>vacinação da COVID-19</b> são compilados pelo projeto Nosso Mundo em Dados (Our World in Data ou OWID) da universidade britânica de <b>Oxford</b> <a href="https://www.ox.ac.uk/">(link)</a>. Os dados são <b>atualizados diariamente</b> deste janeiro de 2020 com uma <b>granularidade temporal de dias e geográfica de países</b>. O website do projeto pode ser acessado neste link enquanto os dados, neste <a href="https://covid.ourworldindata.org/data/owid-covid-data.csv">link</a>. Abaixo estão descritos os dados derivados do seu processamento.</p>

- <b>date</b>: data de referência;
- <b>country</b>: país;
- <b>population</b>: população estimada;
- <b>total</b>: número acumulado de doses administradas;
- <b>one_shot</b>: número acumulado de pessoas com uma dose;
- <b>one_shot_perc</b>: número acumulado relativo de pessoas com uma dose;
- <b>two_shots</b>: número acumulado de pessoas com duas doses;
- <b>two_shot_perc</b>: número acumulado relativo de pessoas com duas doses;
- <b>three_shots</b>: número acumulado de pessoas com três doses;
- <b>three_shot_perc</b>: número acumulado relativo de pessoas com três doses;
- <b>month</b>: mês de referência;
- <b>year</b>: ano de referência;

<h3>2. Análise exploratória de dados</h3>

<h4>2.1. Casos</h4>

Vamos processar os dados de <b>casos</b> da universidade John Hopkins.

<h5>2.1.1. Extração</h5>

Como os dados estão compilados em um arquivo por dia (exemplo: 2021/12/01), foi preciso fazer uma série de etapas para extrair todos os dias.

- Foi criada a função date_range como meta para a iteração a cada data entre 2 periodos, start_date e end_date, ambos informados manualmente;
- Como o endereço do GitHub <a href="https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports/01-12-2021.csv">(link)</a> pode ser alterado para acessar cada dia, foi utilizado o date_range para alterar a data de acesso a cada iteração;
- A cada vez, foi selecionado as colunas de interesse descritas, a região do Brasil e criada uma coluna data já formatada para o Data Studio.


<h5>2.1.2. Wrangling</h5>

Os dados foram  manipulados com o foco em garantir uma boa granularidade e qualidade da base de dados. 

- Foram alteradas os nomes das colunas para facilitação;
- Na coluna state, os nomes de estados com acento foram atualizados para posterior utilização no Data Studio, assim como foram criadas chaves temporais de mês e ano para o mesmo propósito;
- Os métodos Diff, Rolling e Shift foram utilizados para encontrar os valores dos casos e mortes em 24h, média móvel de 7 dias e estabilidade de 14 dias, respectivamente;
- Por último, os dados foram convertidos para os tipos respectivos e as colunas reorganizadas para seguir o padrão estipulado.

<h5>2.1.3. Carregamento</h5>

Com os dados manipulados, vamos persisti-lo em disco, fazer o seu download e carrega-lo no Google Data Studio.

<h4>2.2. Vacinação</h4>

Vamos processar os dados de vacinação da universidade de Oxford.

<h5>2.2.1. Extração</h5>

Ao contrário dos casos e mortes, os dados de vacinação estão consolidados em uma única planilha, facilitando o acesso. Contudo, como eles estão dispostos por país e não por estado, só será possivel analisar a vacinação no Brasil inteiro de uma vez.

- Foi selecionado o Brasil apenas como pais de interesse, assim como já reorganizado as colunas.

<h5>2.2.2. Wrangling</h5>

Segue a manipulação dos dados para garantia da granulidade.

- Os dados faltantes foram alimentados pelos dados do dia anterior com o método fillna;
- Foi separado a data de vacinação de acordo com o interesse, de 01-01-2021 até 31-12-2022;
- Renomeadas as colunas da 1º, 2º e 3º dose como proposto;
- Criados chaves temporais de mês e ano, criado um percentual dos valores das colunas alterada e corrigido para numérico essas colunas.
- Reorganização das colunas por fim.

<h5>2.2.3. Carregamento</h5>

Com os dados manipulados, vamos persisti-lo em disco, fazer o seu download e carrega-lo no Google Data Studio.

<h3>3. Exploração interativa dos dados</h3>

<h4>3.1. KPIs</h4>

O dashboard de dados contem os seguintes indicadores chaves de desempenho (key performance indicator ou KPI) consolidados:

1. Casos e mortes nas 24 horas;
2. Média móvel (7 dias) de casos e mortes;
3. Tendência de casos e mortes;
4. Proporção de vacinados com 1ª, 2ª e 3ª doses.

<h4>3.2. EDA</h4>

O dashboard de dados contem os seguintes gráficos para a análise exploratória de dados (exploratory data analysis ou EDA) interativa:

1. Distribuição do números de casos e mortes ao longo do tempo;
2. Distribuição da média móvel (7 dias) do números de casos e mortes ao longo do tempo;
3. Distribuição de risco de mortalidade média;
4. Distribuição geográfica dos casos por estado por dia.


<h3>4. Resultado</h3>

<p>A partir da análise do dashboard, é possivel retirar algumas conclusões:

- O estado com a maior média de fatalidade (quantidade de mortes por casos) é o RJ com 4,45%, seguindo de São Paulo. Então, apesar do grande surto, o número de mortes está dentro da média mundial.
- Apesar de haver uma queda no 2º semestre de 2021, houve um aumento gigantesco no número de casos confirmados em Janeiro e fevereiro de 2022. Isso pode ter váriados motivos, como a subnotificação e o aumento de testagem. Mas, a Organização Pan-Americana de Saúde <a href="https://www.paho.org/pt/noticias/16-2-2022-relaxamento-medidas-saude-publica-contribuiu-para-aumento-mortes-por-covid-19">(link)</a> informa que esse aumento é devido as flexibilizações da população enquanto as politicas de lockdown. Seguindo o dashboard, isso faz sentido, pois a partir de Janeiro de 2022 já havia uma taxa de vacinação acima de 75%, o que levou as pessoas a tomarem maior liberdade para sairem de casa.
- A taxa de vacinação foi relativamente boa, considerando que em menos de 1 ano, mais de 75% da população já tinha tomado a 1º dose e mais de 65% a 2º dose.</p>

<h2>Tecnologia, Pacotes e Bibliotecas</h2>

- Python
- Jupyter Notebook
- Google Data Studio
- Pacotes e Bibliotecas
  - math
  - typing, Iterator
  - datetime, timedelta
  - numpy
  - pandas
  


<h2>Contato</h2>
<p>Caio Marques - <a href="https://www.linkedin.com/in/caiombr/">LinkedIn</a></p>
