# Sobre a proposta e objetivo do Web Scraping

O presente Web Scraping tem o objetivo de obter os dados da Folha de Pessoal dos Municípios da Bahia, por meio do site do Tribunal de Contas dos Municípios do Estado da Bahia (TCM-Ba), visto que os dados ainda não são disponibilizados no formato de Dados Abertos, conforme impõe a Lei de Acesso à Informação (art. 8º, §3°, III, da LAI). Em vista disso, foi necessário produzir um Web Scraping para obter os dados disponibilizados no site do TCM-Ba em formato HTML (Ver [link do TCM-Ba](http://www.tcm.ba.gov.br/portal-da-cidadania/pessoal/)) de forma sistêmica e automatizada, reduzindo significativamente o tempo que se levaria para obter os dados caso as consultas fossem feitas manualmente.


## Sobre o Código do Web Scraping em Linguagem R

O código do *Web Scraping - Folha de Pessoal dos Municípios da Bahia via TCM-Ba* apesar de estar operacional, ainda faltam implementações (as quais serão mais detalhadas abaixo), bem como aperfeiçoar os códigos inicialmente utilizados; transferir as estratégias de armazenamento inicialmente adotadas (arquivos CSV) para um Sistema de Gestão de Banco de Dados (SGBD), a exemplo do SQLite; adotar estratégias mais eficientes na fase de tratamento de dados (Data Wrangling), tornando essa etapa mais rápida; e implantar um sistema de tratamento de erros durante a execução do Web Scraping.

O código foi desenvolvido para ambiente **Windows 10**, com a versão *3.4.2* do R. Apesar do código ter sido desenvolvido para funcionar em **Linux**, ainda não foram realizados testes.


## Etapas e Estratégias do Web Scraping

Para realizar com sucesso o presente Web Scraping, foi preciso mapear as etapas que serão pecorridas pelo Web Scraping, as quais podem ser sintetizadas da seguinte forma:

1° - Realizar uma requisição POST no [link do TCM-Ba](http://www.tcm.ba.gov.br/portal-da-cidadania/pessoal/) por meio da qual é possível consultar manualmente a folha de pessoal dos municípios do Estado da Bahia;
2° - Após realizada a requisição POST com os dados do Município, Entidade e Ano, procedemos a coleta dos dados da tabela que contém os dados da Folha de Pessoal, na qual são registradas informações sobre Nome do Servidor Público; N° matrícula; Tipo Servidor; Cargo; Salário Base; Salário Vantagens e Salário Gratificação. Ainda nessa etapa, precisamos incluir algumas informações adicionais para faciliar o armanezamento e futura análise dos dados, a exemplo das seguintes colunas: Ano, Mês, Código do Município, Nome do Município, Código da Entidade e Nome da Entidade.
3º - Obtido os dados, realizamos o seu tratamento, com o objetivo de auxiliar as futuras análises dos dados. Nessa fase, todas as letras foram transformadas em maiúsculas e retirado a acentuação das palavras.

Como estratégia na obtenção dos dados, optamos por armazenar os arquivos HTML das requisições, com vista a poder reproduzir a etapa de tratamento e limpeza dos dados, caso fosse necessário, visto o número de requisições que seriam realizas (cerca de 30.000).

## Estrutura do Código em Linguagem R do Web Scraping

O presente código foi dividido inicialmente no que poderíamos classificar em 10 estruturas:

Configuração:
1 - Carregar os pacotes e a pasta de trabalho (diretório);

Pré-Scraping Principal:
2 - Criar a Tabela com dimensão Calendário;
3 - Obter o Código e Nome dos Municípios no site do TCM-Ba e Criar uma Tabela com esses dados;
4 - Obter o Código e Nome das Entidades Municipais (Prefeitura, Câmara de Vereadores...) no Web Service do TCM-Ba e, por fim, consolidar todos os dados (código e nomes) dos Municípios e Entidades Municipais em uma só Tabela
5 - Gerar uma Tabela de Requisições para relizar as consultar no site do TCM-Ba via método POST;

Scraping:
6 - Scraping dos Dados de cada entidade municipal em determinado ano e mês;

Tratamento dos Dados:
7 - Data Wrangling (tratamento) dos dados obtidos via Web Scraping do site do TCM-Ba;

Disponibilizar os dados:
8 - API disponibilizar os dados via consulta GET;
9 - Exportar dados para o Banco de Dados do CKAN;

Automatizar a execução do Scraping:
10 - Timer para disparar o Web Scraping em diárias e horários determinados;

### Código do Web Scraping em Linguagem R 

O código do Web Scraping está no arquivo "TCM - Folha de Pessoal dos Municipios.Rmd".