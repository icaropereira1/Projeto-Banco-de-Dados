# Sistema de Futebol da Copa do Mundo ⚽🏆

Este repositório contém o projeto de banco de dados desenvolvido para a disciplina de **Banco de Dados**, sob a orientação do **Professor Cássio Leonardo Rodrigues**. O objetivo central do sistema é organizar de forma estruturada as informações históricas das edições da Copa do Mundo da FIFA, focando em jogadores, seleções e estatísticas de partidas.

## 👥 Autores

* 
**João Gabriel Abreu Soares** (202302553) 


* 
**Ícaro Pereira Rosa Alves de Sá** (202302547) 



## 🛠️ Tecnologias Utilizadas

* 
**SGBD:** PostgreSQL 


* 
**Linguagem:** SQL (DDL, DML, DQL) 


* 
**Modelagem:** Diagrama Entidade-Relacionamento (DER) e Mapeamento Relacional 



## 📋 Escopo e Funcionalidades

O sistema abrange o registro histórico detalhado de competições passadas:

* 
**Edições e Seleções:** Cadastro de anos, países-sede e seleções participantes por confederação.


* 
**Jogadores:** Registro de dados biográficos e histórico de clubes/camisas através de convocações.


* 
**Partidas:** Dados de fase, local, data e placar.


* 
**Desempenho:** Registro de gols, assistências, minutos jogados e cartões por partida.



## 📐 Modelagem de Dados

### Modelo Entidade-Relacionamento (MER)

O projeto conceitual define as entidades principais e seus relacionamentos, como a relação entre jogadores e jogos através da entidade associativa "Participação".

### Normalização

O esquema relacional foi refinado para atender às três primeiras formas normais:

1. 
**1FN (Atomicidade):** Uso de tabelas associativas para evitar grupos repetidos.


2. 
**2FN (Dependência Total):** Atributos como gols e assistências dependem da chave primária composta (jogador + jogo).


3. 
**3FN (Dependência Transitiva):** Criação da tabela `convocacao` para armazenar dados voláteis (clube e número), mantendo a tabela `jogador` apenas com dados imutáveis.



## 🚀 Implementação

### Scripts DDL (Criação)

O banco de dados é composto pelas seguintes tabelas principais:

* `edicao`
* `selecao`
* `jogador`
* `jogos`
* `participacao`
* `convocacao`
* `participacao_selecao`

### Relatórios SQL (Exemplos)

O sistema permite a geração de relatórios complexos, tais como:

* Média de gols por edição específica.


* Top 10 jogadores com mais gols e assistências em uma edição.


* Total de cartões amarelos e vermelhos por confederação.


* Jogos de mata-mata que terminaram em empate no tempo normal.



## 📑 Referências

* 
[FIFA Official Website](https://www.fifa.com/pt) 


* 
[Transfermarkt](https://www.transfermarkt.com.br/) 


* 
[Globo Esporte (GE)](http://ge.globo.com/) 
