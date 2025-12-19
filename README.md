# ⚽🏆 Sistema de Banco de Dados: Copa do Mundo FIFA

Este projeto apresenta a **especificação e implementação de um Banco de Dados Relacional** voltado ao registro histórico das edições da **Copa do Mundo da FIFA**.
O sistema gerencia dados de **edições, seleções, jogadores e estatísticas detalhadas de cada partida**, permitindo consultas e análises de desempenho ao longo dos torneios.

---

## 👥 Autores

* **João Gabriel Abreu Soares** – 202302553
* **Ícaro Pereira Rosa Alves de Sá** – 202302547

**Professor:** Cássio Leonardo Rodrigues
**Disciplina:** Banco de Dados

---

## 📌 Sumário

1. [Introdução](#1-introdução)
2. [Requisitos do Sistema](#2-requisitos-do-sistema)
3. [Projeto Conceitual e Lógico](#3-projeto-conceitual-e-lógico)
4. [Normalização](#4-normalização)
5. [Implementação (DDL)](#5-implementação-ddl)
6. [População e Consultas (DML/DQL)](#6-população-e-consultas-dmldql)
7. [Referências](#7-referências)

---

## 1. Introdução

Desenvolvido utilizando o **SGBD PostgreSQL**, este sistema tem como objetivo organizar e armazenar informações históricas da **Copa do Mundo da FIFA**, possibilitando **análises de desempenho coletivo e individual** ao longo das edições do torneio.

O escopo do banco de dados contempla:

* Cadastro de **países-sede**;
* Registro de **edições do torneio**;
* Informações de **seleções nacionais e jogadores**;
* Estatísticas técnicas, como **gols, assistências e cartões por partida**.

---

## 2. Requisitos do Sistema

### 2.1 Requisitos Funcionais

* **Edições:** gerenciamento do ano da edição, país-sede e seleção campeã;
* **Seleções:** cadastro das nações participantes e suas respectivas confederações;
* **Jogadores:** registro de dados biográficos, posição, nacionalidade e vínculo com a seleção;
* **Partidas:** controle de placar, fase do torneio (grupos, oitavas, quartas, etc.), local e data;
* **Convocações:** histórico de clube do jogador e número da camisa em cada edição específica.

### 2.2 Requisitos Não Funcionais (Integridade)

* **Integridade de Entidade:**
  Todas as tabelas possuem chaves primárias (PK) únicas.

* **Integridade Referencial:**
  Uso de chaves estrangeiras (FK) com `ON DELETE CASCADE`, garantindo consistência entre edições e registros dependentes.

* **Integridade de Domínio:**
  Restrições para assegurar:

  * valores de placar não negativos;
  * datas de nascimento válidas e coerentes.

---

## 3. Projeto Conceitual e Lógico

O projeto foi desenvolvido com base em um **Modelo Entidade-Relacionamento (MER)**, conectando os principais elementos do torneio: edições, seleções, jogadores, partidas e estatísticas individuais.

### 📘 Dicionário de Dados (Resumo)

| Tabela       | Descrição                                        | Identificador  |
| ------------ | ------------------------------------------------ | -------------- |
| Edição       | Dados do torneio por ano                         | `ano`          |
| Seleção      | Cadastro das nações participantes                | `id_selecao`   |
| Jogador      | Dados biográficos dos atletas                    | `id_jogador`   |
| Jogos        | Confrontos entre seleções                        | `id_jogo`      |
| Convocação   | Relação N:N entre Jogador e Edição               | Chave composta |
| Participação | Estatísticas de um jogador em um jogo específico | Chave composta |

---

## 4. Normalização

O esquema do banco de dados foi normalizado para reduzir redundâncias e evitar inconsistências:

* **Primeira Forma Normal (1FN):**
  Atributos atômicos e criação de tabelas associativas para relações muitos-para-muitos (N:N).

* **Segunda Forma Normal (2FN):**
  Eliminação de dependências parciais; estatísticas de desempenho dependem integralmente da chave composta (**jogador + jogo**).

* **Terceira Forma Normal (3FN):**
  Remoção de dependências transitivas. Informações temporais, como clube e número da camisa, foram isoladas da entidade **Jogador**.

---

## 5. Implementação (DDL)

```sql
-- Criação da tabela de jogadores
CREATE TABLE jogador (
    id_jogador SERIAL NOT NULL,
    nome VARCHAR(150) NOT NULL,
    data_nascimento DATE,
    posicao VARCHAR(50),
    nacionalidade VARCHAR(100),
    id_selecao INTEGER,
    CONSTRAINT jogador_pkey PRIMARY KEY (id_jogador),
    CONSTRAINT jogador_id_selecao_fkey FOREIGN KEY (id_selecao)
        REFERENCES public.selecao (id_selecao)
);

-- Tabela de estatísticas por partida (Participação)
CREATE TABLE participacao (
    id_jogo INTEGER NOT NULL,
    id_jogador INTEGER NOT NULL,
    minutos_jogados INTEGER,
    gols INTEGER DEFAULT 0,
    assistencias INTEGER DEFAULT 0,
    cartoes VARCHAR(50),
    CONSTRAINT participacao_pkey PRIMARY KEY (id_jogo, id_jogador),
    CONSTRAINT participacao_id_jogo_fkey FOREIGN KEY (id_jogo)
        REFERENCES public.jogos (id_jogo),
    CONSTRAINT participacao_id_jogador_fkey FOREIGN KEY (id_jogador)
        REFERENCES public.jogador (id_jogador)
);
```

---

## 6. População e Consultas (DML/DQL)

### 📊 Média de Gols por Jogo (Copa de 2022)

```sql
SELECT AVG(gols_selecao1 + gols_selecao2) AS media_gols
FROM public.jogos
WHERE ano_edicao = 2022;
```

### 🏆 Top 10 Jogadores (Gols + Assistências – Copa de 2022)

```sql
SELECT j.nome,
       SUM(p.gols) AS total_gols,
       SUM(p.assistencias) AS total_assistencias
FROM public.participacao p
JOIN public.jogador j ON p.id_jogador = j.id_jogador
JOIN public.jogos g ON p.id_jogo = g.id_jogo
WHERE g.ano_edicao = 2022
GROUP BY j.nome
ORDER BY total_gols DESC, total_assistencias DESC
LIMIT 10;
```

---

## 7. Referências

* **Transfermarkt** – Dados técnicos de jogadores e seleções
* **FIFA Official** – Histórico oficial da Copa do Mundo
* **Globo Esporte** – Estatísticas e informações complementares
