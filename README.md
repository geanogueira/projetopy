# Introdução

É comum que as organizações utilizem planilhas do Microsoft Excel como meio para orçar/precificar seus produtos e/ou serviços a seus clientes. E isto é devido, em grande medida, à notória flexibilidade dessa ferramenta, que lhe permite atender aos mais diversos tipos de negócio, com diferentes especificidades. 
Entretanto, embora o Excel seja, de fato, um poderoso aliado nessa tarefa, ele é um fator limitante no que diz respeito à integração de dados, pois, levando ainda em conta a complexidade que planilhas de orçamentação costumam carregar, torna-se praticamente impossível, por meio dele próprio, unificar esses dados e analisá-los sob a ponto de vista gerencial, onde se avaliariam aspectos fundamentais da construção de preços, na figura de métricas e indicadores, com o objetivo final de tornar a orçamentação mais assertiva, para que aumente o potencial de venda da organização, bem como eleve as margens de resultado. 
Desse modo, a própria ferramenta acaba por criar pequenos silos de dados, haja vista que os dados de um determinado orçamento ficam restritos ao arquivo “.xlsx” que o contém, por exemplo.

# Cenário do projeto

No cenário fictício que dá base a este projeto, existe uma determinada organização, cuja formação de preços de seus produtos e serviços é realizada por meio de uma planilha padrão no Microsoft Excel, e, do ponto de vista gerencial, incorre no mesmo problema exposto na introdução.
Segue um exemplo de como é estruturada essa planilha:

![image](https://i.ibb.co/rmzcbQM/planilha.png)

# Objetivos do projeto

Pensando na resolução do problema supramencionado, o presente projeto, primeiramente, se utilizou de ferramentas para ler, limpar e organizar o conteúdo dos arquivos Excel de orçamentação, para então os disponibilizou num Data Warehouse no Microsoft Azure.
Por fim, para responder às perguntas de cunho gerencial, tais dados foram compilados num dashboard de 3 páginas, o qual, por conseguinte, contempla métricas e indicadores relevantes ao processo de formação de preços da organização.

# Serviços e ferramentas utilizadas

- **Python**: linguagem na qual foram escritos os scripts que executaram os processos de ETL (Extract, Transform and Load) dos arquivos Excel de orçamentação;
- **Azure SQL Server**: plataforma que serviu como o ambiente de Data Warehouse;
- **Power BI (Desktop e Online)**: serviu como a ferramenta de visualização de dados, na qual, portanto, foram construídos os relatórios analíticos.

### Scripts Python

*De antemão, cabe esclarecer que os arquivos Excel de orçamentação também foram gerados randomicamente por meio de um script Python, o qual está presente no arquivo “00 Criador de Orçamentos.ipynb”. Ao todo, foram criados cerca de 6.300 arquivos cujas datas variam de 01/01/2023 a 31/05/2024.*

Para iniciar o consumo desses dados, foi preciso realizar uma análise exploratória de toda base de dados, ou seja, de todos os arquivos Excel. Para isso, criou-se um script Python, intitulado de “01 Análise Exploratória Arquivos de Orçamentos.ipynb”, por meio do qual foram avaliadas todas as variáveis necessárias para a correta extração dos dados presentes nesses arquivos.
Por fim, o script que efetivamente contém a execução dos processos de ETL está escrito no arquivo “02 Extrai Dados de Orçamentos (Histórico).ipynb”. Nele, portanto, cada arquivo de orçamentação teve seus dados extraídos, tratados e carregados num banco de dados SQL Server na Azure. E estas foram as principais bibliotecas envolvidas nas tarefas acima:

- OS e RE: utilizadas para interagir com o local, no Microsoft Windows, onde estavam salvos os arquivos de orçamentação, e relacioná-los numa lista de arquivos;
- PANDAS: seus métodos e propriedades foram responsáveis pela maioria das tarefas relacionadas à extração e transformação dos dados oriundos de cada arquivo Excel;
- PYODBC: foi responsável pela integração do Python com o banco de dados Azure SQL Server (por meio de conexão ODBC), permitindo a busca de dados de tabelas dimensões, também utilizados nas tarefas de tratamento dos dados das orçamentações, e possibilitando a inserção dos dados finais (tratados) na tabela fato, que já estava devidamente parametrizada para recebê-los.

### Azure SQL Server

Este banco de dados serviu como o ambiente de Data Warehouse, no qual foram inseridos, portanto, os dados tratados pelo script Python supracitado.
A modelagem de dados deste projeto é ligeiramente simples, contemplando apenas 4 entidades: 1 tabela fato e 3 dimensões, sendo uma delas a tabela calendário, conforme diagrama ER abaixo:

![image](https://i.ibb.co/p2DMfP0/diagrama-er.png)

![image](https://i.ibb.co/6DDdngk/tabela-fato.png)

![image](https://i.ibb.co/6DDdngk/tabela-fato.png)

![image](https://i.ibb.co/S74SxRB/tabela-calendario.png)

### Power BI

Software utilizado para a construção do relatório, que contemplou 3 páginas:

#### Página 1 – Panorama

Nela constam as principais métricas e indicadores do processo de orçamentação, bem como visões de evolução histórica e de segmentação por cliente dessas métricas.

![Página 1 - Panorama](https://i.ibb.co/PD8tYw6/pagina1-panorama.png)

#### Página 2 – Demonstrativo

Nesta página constam sobretudo as tabelas de detalhamento da orçamentação. A primeira visão, inclusive, agrega os montantes orçados no período temporal escolhido, utilizando um formato similar a um DRE, da mesma forma que se encontra nos arquivos individuais de orçamentação.

![Página 2 - Demonstrativo](https://i.ibb.co/qYs5Q7b/pagina2-demonstrativo.png)

#### Página 3 – Distribuição Geográfica

Por fim, são trazidas visões relacionadas à distribuição geográfica dos orçamentos, baseadas nas principais métricas do negócio.

![Página 3 - Distribuição Geográfica](https://i.ibb.co/c8KM7kS/pagina3-distribuicao-geografica.png)
