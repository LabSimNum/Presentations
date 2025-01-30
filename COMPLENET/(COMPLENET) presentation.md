---
marp: true
math: katex
paginate: true
theme: gaia
style: |
    .columns {
        display: flex;
        justify-content: center; /* Centraliza as divs dentro do container */
        gap: 20px; /* Espaço entre as colunas */
    }

    /* Estilizando cada bloco de texto + imagem */
    .text-center {
        display: flex;
        flex-direction: column; /* Organiza os elementos em coluna */
        align-items: center; /* Centraliza na horizontal */
        text-align: center; /* Centraliza o texto */
        width: 50%; /* Cada bloco ocupa metade da tela */
    }

    /* Ajuste opcional para responsividade */
    img {
        max-width: 100%; /* Garante que as imagens não ultrapassem o contêiner */
        height: auto;
    }
    .rounded-frame {
        border-radius: 10px;
        border: 1px solid #ccc;
        padding: 10px;        
    }

    .table-center {
        text-align: center;
        margin-left: auto;
        margin-right: auto;
    }

    .caption {
        text-align: center;
    }

    table {
        margin-left: auto;
        margin-right: auto;
    }
    .text-center {
        text-align: center;
    }
---


## Análise de estratégias de vacinação para COVID-19 baseada em redes complexas


![bottom-center width:400px](img/ufc-logo-universidade-14.png)



---
# CONTEXTO DO COVID

- Disseminação do vírus e alta taxa de mortalidade
- Custos elevados e tempo para desenvolvimento de vacinas
    - Pesquisa
    - Armazenamento
    - Distribuição
- Portanto urge a necessidade de estratégias eficientes de vacinação!!!


---
# Modelo de Infecção Proposto

<p align="center">
    <img src="img/seihards.png" width="1200px" />
</p>

---

# COLETA DE DADOS - REDE

- Em 2008, a Comissão Europeia criou o projeto POLYMOD para estudar padrões de contatos na Europa.
- Questionários via ligações aleatórias ou entrevistas em 8 países europeus.
    - Informações pessoais e ambientais (escola, trabalho, casa, etc.).
    - Detalhes sobre contatos físicos diários (idade, duração, frequência).
        
<span style="font-size: 0.2em; color: red;">https://socialcontactdata.org/</span>

---
# COLETA DE DADOS - REDE



---

## COLETA DE DADOS - REDE



![left width:520](img/h.png)


---

## COLETA DE DADOS - REDE



![right width:1020](img/media_std.png)


---

# Coleta de dados - COVID
- Coletado dados de diversos artigos da literatura e do OpenDataSus.
- Distribuição de faixa etária foi utilizada a do Brasil.
![bg right width:650](img/faixas_brasil_polymod.png)
---

# Simulação

- A simulação dura 465 dias;
- No dia 100 são vacinados uma fração da população que estejam no estágio Suscetível, Recuperados, Assintomáticos ou Expostos;
- Se ainda sim sobrar vacinas os outros sítios serão vacinados quando entrarem naqueles compartimentos.
- A simulação foi realizada para uma rede de tamanho 10.000 com 400 redes diferentes.

---
# Vacinação 

- Foram usadas mais de 30 estratégias baseadas em centralidades para redes sem ponderação nas arestas e mais de 60 para redes com ponderação;
- Foram utilizadas 3 métricas: Fração de Mortos, Tempo Total Hospitalizado e Fração de Infectados;
- Para centralidades que usam pesos nos nós foram utilizadas duas abordagens: Altruísta e Individualista.
---
# Vacinação 


<div class="columns">   
    <div class="text-center">
        <h2>Altruísta</h2>
        <img src="./img/altruista.png" width="450px">
    </div>
    <div class="text-center">
        <h2>Individualista</h2>
        <img src="./img/individualista.png" width="450px">
    </div>
</div>


---

# Resultados




---
# Contribuições

- Investigação de métricas de centralidade para identificar indivíduos chave na propagação do COVID-19
- Proposição de um modelo de rede considerando conexões entre diferentes faixas etárias
- Desenvolvimento de um modelo de propagação da COVID-19 mais complexo
- Redução significativa da mortalidade e hospitalizações com estratégias baseadas em PageRank ponderado

---

# Desafios

- Limitações na abrangência dos dados do POLYMOD para diferentes países e culturas;
- Necessidade de explorar o impacto da superlotação de hospitais no modelo epidemiológico proposto;
- Avaliação da eficácia das estratégias de vacinação em diferentes cenários epidemiológicos;
- As métricas de centralidade são alteradas com remoções dos nós.

