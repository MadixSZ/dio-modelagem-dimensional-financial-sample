# 📊 Modelagem Dimensional - Star Schema no Power BI (Desafio DIO)

<p align="center">
  <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" alt="Power BI">
  <img src="https://img.shields.io/badge/DAX-00758F?style=for-the-badge&logo=dax&logoColor=white" alt="DAX">
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge" alt="Status Concluído">
</p>

## 🚀 Sobre o Projeto
Este repositório contém a resolução do desafio de projeto da **DIO (Digital Innovation One)** centrado na criação de um modelo dimensional em **Star Schema (Esquema em Estrela)** utilizando o **Power BI**. 

O objetivo principal foi transformar uma tabela única de transações (*Financial Sample*) em um modelo relacional otimizado, aplicando boas práticas de engenharia de dados, duplicação e isolamento de fontes, enriquecimento de tabelas e funções DAX.

---

## 🏗️ Arquitetura do Modelo (Star Schema)

O modelo foi estruturado com uma **Tabela Fato central** cercada por **Tabelas Dimensão**, garantindo melhor performance nas consultas e clareza analítica:

- **`F_Vendas` (Tabela Fato):** Contém as transações de vendas, métricas numéricas e a chave estrangeira criada por coluna condicional (`ID_Produto`).
- **`D_Produtos` (Dimensão):** Agrupamento por produto contendo métricas estatísticas (médias, mínimos, máximos e medianas de preços e unidades).
- **`D_Descontos` (Dimensão):** Isola as faixas de desconto e regras associadas.
- **`D_Produtos_Detalhes` (Dimensão):** Detalhes complementares de fabricação, preços de venda e unidades.
- **`D_Calendário` (Dimensão):** Tabela de datas gerada por DAX para análise temporal robusta.
- **`financials`:** Tabela de backup original mantida fora do carregamento ativo do modelo (*Desabilitar Carga*).

---

## 🛠️ Etapas de Desenvolvimento

1. **Gestão de Origem e Backup:** 
   - Duplicação da tabela original para criação de uma base de segurança (`financials`) com a opção de carga desativada.
2. **Transformação de Dados (Power Query):**
   - Criação de uma coluna condicional na tabela fato para gerar o **`ID_Produto`** com base nos nomes dos produtos (*Carretera*, *Montana*, *Paseo*, etc.).
   - Utilização da ferramenta de **Agrupar Por (Group By)** no modo avançado para gerar a tabela `D_Produtos` consolidando somas e médias estatísticas.
3. **Modelagem Temporal (DAX):**
   - Criação da tabela de calendário dinâmica utilizando a função `CALENDAR`:
     ```dax
     D_Calendário = CALENDAR(DATE(2013, 1, 1), DATE(2014, 12, 31))
     ```
4. **Relacionamentos (Model View):**
   - Estabelecimento de conexões do tipo **Um para Muitos (1:\*)** entre as chaves primárias das tabelas dimensão e as chaves estrangeiras da tabela fato `F_Vendas`.

