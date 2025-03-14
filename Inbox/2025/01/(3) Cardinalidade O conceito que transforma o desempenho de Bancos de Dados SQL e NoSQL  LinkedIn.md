---
created: 2025-01-20T17:52:36 (UTC -03:00)
tags: []
source: https://www.linkedin.com/pulse/cardinalidade-o-conceito-que-transforma-desempenho-de-jhonathan-6jrnf/
author: Jhonathan de Souza Soares
---

# (3) Cardinalidade: O conceito que transforma o desempenho de Bancos de Dados SQL e NoSQL | LinkedIn

> ## Excerpt
> Cardinalidade é um conceito essencial em bancos de dados que mede a unicidade dos valores em uma coluna ou campo. Embora seja frequentemente associado ao desempenho de consultas SQL, seu impacto se estende a bancos de dados NoSQL, aprendizado de máquina e modelagem de dados.

---
Cardinalidade é um conceito essencial em bancos de dados que mede a unicidade dos valores em uma coluna ou campo. Embora seja frequentemente associado ao desempenho de consultas SQL, seu impacto se estende a bancos de dados NoSQL, aprendizado de máquina e modelagem de dados.

Este artigo explora a cardinalidade em profundidade, seus impactos em diferentes contextos e exemplos práticos, com foco em profissionais de software que desejam otimizar sistemas de banco de dados.

## O Que é Cardinalidade?

A cardinalidade refere-se ao número de valores únicos em uma coluna ou campo de banco de dados:

-   **Alta Cardinalidade:** A maioria dos valores é única (ex.: IDs de usuários, endereços de e-mail).
-   **Baixa Cardinalidade:** Poucos valores únicos (ex.: gêneros, status ativo/inativo).
-   **Zero Cardinalidade:** Nenhum valor é único (ex.: todos os valores da coluna são iguais).

Essa característica influencia diretamente o desempenho de consultas, a eficácia de índices e a eficiência na modelagem de dados. Imagine uma tabela de banco de dados com cadastros básicos de usuários:

![cardinalidade
](https://media.licdn.com/dms/image/v2/D4D12AQHhXrt4R29P4Q/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1737166424983?e=1743033600&v=beta&t=WBwdyjjQZnoqrip_jh_vRKac4UoTManwS3haZz9qwVo)

___

### Impactos da Cardinalidade em Bancos de Dados SQL

Nos bancos relacionais, a cardinalidade desempenha um papel crucial na eficácia dos índices e no tempo de execução das consultas.

### Como a Cardinalidade afeta SQL?

1.  **Alta Cardinalidade:**Índices em colunas de alta cardinalidade são geralmente eficazes, permitindo localizar registros rapidamente.Exemplo: Um índice em uma coluna UserID é ideal para identificar registros únicos.
2.  **Baixa Cardinalidade:**Índices em colunas de baixa cardinalidade frequentemente são ineficazes, pois não ajudam a restringir suficientemente o conjunto de resultados.Exemplo: Um índice em uma coluna Gender (com valores M/F) será ignorado em muitas situações, optando-se por um escaneamento completo da tabela.
3.  **Manutenção de Índices:**Índices em colunas de alta cardinalidade podem consumir mais memória e aumentar os custos de manutenção.

### Exemplo Prático em SQL

Uma tabela Users com 1 milhão de registros possui colunas com diferentes níveis de cardinalidade:

```
<!---->CREATE TABLE Users (
    UserID INT PRIMARY KEY, -- Alta cardinalidade
    Name VARCHAR(100),      -- Alta cardinalidade
    Country VARCHAR(50),    -- Média cardinalidade
    Gender CHAR(1)          -- Baixa cardinalidade
);

-- Índice eficiente
CREATE INDEX idx_name ON Users(Name);

-- Índice ineficaz
CREATE INDEX idx_gender ON Users(Gender);<span> </span>
```

Aqui, o índice em Name é útil, enquanto o índice em Gender é desperdiçado.

### Impactos da Cardinalidade em Bancos de Dados NoSQL

Bancos NoSQL, como MongoDB, lidam com cardinalidade de forma diferente, devido à modelagem flexível de documentos e estratégias de particionamento.

### Como Cardinalidade afeta NoSQL?

1.  **Alta Cardinalidade:**Útil para particionamento, pois distribui dados uniformemente em clusters.Exemplo: Particionar dados de sensores IoT por SensorID.
2.  **Baixa Cardinalidade:**Ideal para agrupamentos. Por exemplo, dados de status (Ativo/Inativo) podem ser referenciados para evitar duplicação.
3.  **Modelagem de Documentos:**Embedding (dados embutidos) é usado quando campos de alta cardinalidade estão fortemente relacionados ao documento pai.

### Exemplo Prático em NoSQL (MongoDB)

**Embedding (Alta Cardinalidade):**

```
<!---->{
  "UserID": "user123",
  "Orders": [
    {"OrderID": "order001", "Amount": 250},
    {"OrderID": "order002", "Amount": 450}
  ]
}<span> </span>
```

**Particionamento (Alta Cardinalidade):**

Dados de sensores IoT particionados por SensorID:

```
<!---->{
  "SensorID": "sensor123",
  "Readings": [
    {"Timestamp": "2025-01-14T10:00:00Z", "Value": 45},
    {"Timestamp": "2025-01-14T10:05:00Z", "Value": 47}
  ]
}<span> </span>
```

**Referenciamento (Baixa Cardinalidade):** Para evitar duplicação de dados:

**Coleção** Departments:

```
<!---->{
  "_id": "IT",
  "DepartmentName": "Tecnologia da Informação"
}<span> </span>
```

**Coleção** Employees:

```
<!---->{
  "EmployeeID": 1001,
  "Name": "Maria",
  "DepartmentID": "IT"
}<span> </span>
```

___

## 8 ou 80, como funciona cada extremo de cada cardinalidade?

### Alta Cardinalidade

**Definição:** Alta cardinalidade ocorre quando a maioria dos valores em uma coluna ou campo é única. Por exemplo, IDs de usuários, endereços de e-mail ou timestamps.

### Prós:

### Precisão de Busca

**Alta cardinalidade permite localizar registros específicos de forma eficiente. Exemplo: Um índice em um campo como** UserID facilita buscas diretas.

```
<!---->SELECT * FROM Users WHERE UserID = 12345;<!---->
```

### Adequado para Índices:

Colunas de alta cardinalidade são excelentes candidatas para índices, pois cada valor restringe muito bem o conjunto de resultados.

### Particionamento em NoSQL:

Em sistemas NoSQL, usar campos de alta cardinalidade como chaves de particionamento distribui os dados uniformemente entre nós do cluster, evitando sobrecarga em um único servidor.

### Contras:

### Custo de Manutenção

-   Índices em colunas de alta cardinalidade consomem mais memória e aumentam o tempo de operações como INSERT e UPDATE, pois o índice precisa ser constantemente atualizado.

### Consultas Agregadas Custosas

-   Consultas que dependem de agregações (ex.: COUNT, SUM) podem ser mais lentas, pois cada valor precisa ser avaliado individualmente.

### Baixa Cardinalidade

**Definição:** Baixa cardinalidade ocorre quando há poucos valores únicos em relação ao número total de linhas. Exemplos: colunas de gênero (M/F), status (Ativo/Inativo), ou países em um sistema global.

### Prós:

### Eficiente para Agrupamento e Agregação:

Baixa cardinalidade é ideal para consultas que agrupam ou agregam dados, como

```
<!---->SELECT Country, COUNT(*) FROM Users GROUP BY Country;<!---->
```

### Menor Custo de Armazenamento:

Dados de baixa cardinalidade podem ser compactados com mais facilidade, economizando espaço de armazenamento.

### Simples para Referenciamento em NoSQL:

Em bancos como MongoDB, colunas de baixa cardinalidade podem ser referenciadas em coleções separadas, economizando espaço e evitando duplicação.

### Contras:

### Índices Pouco Eficazes:

-   Colunas de baixa cardinalidade geralmente não restringem o suficiente o conjunto de resultados, tornando índices menos úteis.
-   Exemplo: Um índice em Gender com valores M e F apontará para muitas linhas, forçando o banco a realizar um “table scan”.

### Desempenho Inferior em Consultas Precisas:

-   Consultas que dependem exclusivamente de colunas de baixa cardinalidade podem ser lentas, já que muitos registros compartilham o mesmo valor.

## Quando Utilizar Alta ou Baixa Cardinalidade?

### Alta Cardinalidade:

Use quando:

-   Você precisa de buscas rápidas e específicas.
-   Há muitos valores únicos que ajudam a restringir consultas.
-   O campo será usado como chave de particionamento em bancos NoSQL.
-   Um índice em UserID para localizar registros de um usuário específico.

Evite quando:

-   O custo de manutenção do índice supera os benefícios (ex.: em tabelas com atualizações frequentes).
-   Consultas dependem de agregações ou agrupamentos.

### Baixa Cardinalidade:

Use quando:

-   Você realiza consultas que agrupam ou agregam dados.
-   O campo é usado em combinação com outros de alta cardinalidade em índices compostos.
-   Referenciamento de dados é uma solução viável para evitar duplicação.

Evite quando:

-   Você depende exclusivamente da coluna de baixa cardinalidade para filtrar grandes conjuntos de dados.

___

## Exemplo Comparativo Prático

Imagine uma tabela de usuários:

![](https://media.licdn.com/dms/image/v2/D4D12AQHtFb-hZhe-Jg/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1737166556182?e=1743033600&v=beta&t=TyPycmAPJVGi15SqB_zOQukAKBVRTMxqr-L8sXd6XlU)

### Alta Cardinalidade (UserID):

Consulta para buscar dados de um usuário específico:

```
<!---->SELECT * FROM Users WHERE UserID = 12345;<!---->
```

**Motivo:** Alta cardinalidade torna o índice eficiente para restringir os resultados.

### Baixa Cardinalidade (Gender):

Consulta para contar o número de usuários por gênero:

```
<!---->SELECT Gender, COUNT(*) FROM Users GROUP BY Gender;<!---->
```

**Motivo:** A baixa cardinalidade facilita agrupamentos, mas um índice em Gender seria ineficaz sozinho para buscas individuais.

Entender como e quando usar alta ou baixa cardinalidade é fundamental para otimizar sistemas SQL e NoSQL. O segredo está em analisar o padrão de consultas e o comportamento esperado dos dados. Com essas práticas, você pode garantir desempenho, escalabilidade e eficiência em seus projetos de banco de dados.

### Práticas Recomendadas

1.  **Entenda Seu Dataset:**Analise os dados para determinar a cardinalidade de cada coluna.
2.  **Use Índices Compostos:**Combine alta e baixa cardinalidade para melhorar consultas complexas.
3.  **Otimize Particionamento:**Escolha chaves de alta cardinalidade para distribuir dados uniformemente em clusters NoSQL.
4.  **Evite Excesso de Índices:**Em colunas de baixa cardinalidade, índices podem ser contraproducentes.

Este artigo foi originalmente publicado em: [https://codigosimples.net/2025/01/14/cardinalidade-o-conceito-que-transforma-o-desempenho-de-bancos-de-dados-sql-e-nosql/](https://codigosimples.net/2025/01/14/cardinalidade-o-conceito-que-transforma-o-desempenho-de-bancos-de-dados-sql-e-nosql/)
