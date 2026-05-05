# 🚚 Logistics-Data-Analysis-Power-BI--PT

> Análise de dados de logística com foco no desempenho de entregas, pontualidade, canais de entrega e avaliação por vendedor, abrangendo os anos de 2019 e 2020.

---

## 📌 Índice

- Sobre o Projeto
- Objetivos da Análise
- Ferramentas Utilizadas
- Estrutura do Dataset
- Dashboard
- Medidas DAX
- Principais Conclusões
- Como Visualizar)

---

## 📁 Sobre o Projeto

Este projeto foi desenvolvido com base num dataset de logística contendo registos de pedidos, entregas e avaliações de vendedores referentes aos anos de **2019 e 2020**.

O objetivo principal é compreender o desempenho operacional das entregas — identificando atrasos, canais mais eficientes e as equipas com melhor desempenho.

---

## 🎯 Objetivos da Análise

- Monitorizar o total de entregas e a taxa de entregas no prazo
- Identificar os canais de entrega com maior volume e eficiência
- Analisar o desempenho por equipa de entrega
- Identificar as cidades com mais entregas em atraso
- Avaliar o desempenho dos vendedores por rating

---

## 🛠️ Ferramentas Utilizadas

| Ferramenta | Utilização |
|---|---|
| **Power BI Desktop** | Construção do dashboard e visualizações |
| **DAX** | Criação de medidas calculadas |
| **Excel (.xlsx)** | Fonte dos dados de logística |

---

## 📂 Estrutura do Dataset

O ficheiro `dataset.xlsx` contém uma tabela chamada **Logistica** com as seguintes colunas:

| Variável | Tipo | Descrição |
|---|---|---|
| `ID_Pedido` | Numérica | Identificador único do pedido |
| `ID_Vendedor` | Numérica | Identificador do vendedor |
| `ID_Cliente` | Numérica | Identificador do cliente |
| `Equipe_Entrega` | Categórica | Equipa responsável pela entrega (Norte, Sul, etc.) |
| `Cliente` | Categórica | Nome do cliente |
| `Canal_Entrega` | Categórica | Canal utilizado para a entrega |
| `ID_Cidade` | Numérica | Identificador da cidade de entrega |
| `Data_Pedido` | Data | Data em que o pedido foi realizado |
| `Data_Entrega_Prevista` | Data | Data prevista para a entrega |
| `Data_Entrega_Realizada` | Data | Data em que a entrega foi efectivamente realizada |
| `Status_Entrega` | Categórica | Estado da entrega — Antecipado, No Prazo ou Atrasado |

---

## 📊 Dashboard

![Dashboard Preview](https://github.com/diogovent/Logistics-Data-Analysis-Power-BI--PT/blob/main/Dashboard.png)

**KPIs no topo:**
- **Total de Entregas:** 54K
- **Total de Entregas no Prazo:** 47K

**Visuais incluídos:**
- **Total de Entregas no Prazo por Canal de Entrega** — Gráfico de área mostrando a evolução por canal
- **Percentual de Entregas por Equipa** — Gráfico de barras horizontais; a equipa Norte lidera com 27,44%
- **Total de Entregas por Mês** — Gráfico de linhas com evolução mensal ao longo do ano
- **Percentual de Entregas por Status** — Gráfico de barras com Antecipado (70,71%), No Prazo (16,31%) e Atrasado (12,98%)
- **Tabela: Top Vendedores** — Ranking por ID de vendedor com total de entregas e rating em estrelas
- **Tabela: Entregas com Atraso por Cidade** — Cidades com maior número de entregas em atraso

**Filtro disponível:**
- Seleção por ano (2019 / 2020)

---

## 🧮 Medidas DAX

Total de Entregas:
```dax
Total de Entregas = COUNTROWS(Logistica)
```

Total de Entregas no Prazo:
```dax
Total de Entregas no Prazo = CALCULATE([Total de Entregas], FILTER(Logistica, Logistica[Status_Entrega] = "Antecipado" || Logistica[Status_Entrega]= "No Prazo"))
```

Rating:
```dax
Rating = 
VAR __MAX_NUMBER_OF_STARS = 5
VAR __MIN_RATED_VALUE = 1500
VAR __MAX_RATED_VALUE = 2500
VAR __BASE_VALUE = [Total de Entregas]
VAR __NORMALIZED_BASE_VALUE =
	MIN(
		MAX(
			DIVIDE(
				__BASE_VALUE - __MIN_RATED_VALUE,
				__MAX_RATED_VALUE - __MIN_RATED_VALUE
			),
			0
		),
		1
	)
VAR __STAR_RATING = ROUND(__NORMALIZED_BASE_VALUE * __MAX_NUMBER_OF_STARS, 0)
RETURN
	IF(
		NOT ISBLANK(__BASE_VALUE),
		REPT(UNICHAR(9733), __STAR_RATING)
			& REPT(UNICHAR(9734), __MAX_NUMBER_OF_STARS - __STAR_RATING)
	)
```

---

## 🔍 Principais Conclusões

1. **A Maioria das Entregas é Antecipada:**  70,71% das entregas foram realizadas antes da data prevista, o que é um excelente indicador de desempenho

2. **A Taxa de Atraso é de 12,98%:**  Cerca de 1 em cada 8 entregas chega atrasada, um valor a monitorizar

3. **A Equipa Norte Domina o Volume:**  Responsável por 27,44% de todas as entregas, seguida pelo Sudeste com 24,87%

4. **A Cidade 79 Concentra o Maior Número de Atrasos:**  Com 589 entregas em atraso, destaca-se claramente das restantes

5. **O Vendedor 3894 é o mais Produtivo:**  Com 2.208 entregas e avaliação de 4 estrelas

6. **Os Canais Distribuidores, Venda Direta e Centro-Oeste têm Desempenho Muito Baixo:**  Com percentuais próximos de 0%, podem indicar problemas operacionais nesses canais

---

## 💡 Recomendações

1. **Investigar os Atrasos na Cidade 79:** Concentra quase o dobro dos atrasos da segunda cidade mais problemática

2. **Analisar os Canais com Baixo Desempenho:** Distribuidores (0,33%), Venda Direta (0,16%) e Centro-Oeste (0,00%) precisam de atenção

3. **Replicar as Práticas da Equipa Norte:** Sendo a equipa com maior volume, perceber o que fazem bem e aplicar às restantes equipas

4. **Monitorizar a Queda de Entregas a Partir de Setembro:** O gráfico mensal mostra uma descida acentuada no segundo semestre

---

## 📌 Como Visualizar

1. Faz o download do ficheiro `.pbix` disponível neste repositório
2. Abre no **Power BI Desktop** (gratuito)
3. O ficheiro de dados `dataset.xlsx` está incluído no repositório
4. Usa os botões **2019 / 2020** para filtrar por ano
5. Ou então se preferir pode aceder por este link: https://app.powerbi.com/groups/me/reports/dd0c01aa-ab22-4d45-885e-546b96c2c640?pbi_source=desktop
---

## 👤 Autor

Desenvolvido como projecto de análise de dados para portfólio pessoal.
