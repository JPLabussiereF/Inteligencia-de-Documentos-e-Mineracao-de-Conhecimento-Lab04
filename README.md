# Passo a passo, processo, insights e possibilidades aprendidas durante o conteúdo **"Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados"**

## **1. Passo a passo:**

**Primeiramente** vamos ter que criar três recursos no [Portal do Azure](https://portal.azure.com). Um Recurso do *Azure AI Search*, *IA do Azure* e armazenamento. 

Após esses três recursos criado, vamos para o a parte de **Carregar documentos para o armazenamento do Azure**, e no painel do menu esquerdo, selecione Containers. 

![Imagem do passo a passo, sobre como achar os "containers"](Praticas\PassoAPasso\Part1.png)

**Insira as seguintes configurações e clique e crie um novo Contêiner:**

- **Nome:** Coffee-Reviews.
- **Nível de acesso público:** Container (acesso de leitura anônimo para containers e blobs).
- **Avançado:** Sem alterações.

Agora, Em uma nova guia do navegador, baixe as **[avaliações de café compactadas](https://aka.ms/mslearn-coffee-reviews)** em `https://aka.ms/mslearn-coffee-reviewse` e extraia os arquivos para a pasta de avaliações.

![Imagem do passo a passo, sobre como achar os "a pasta de avaliações"](Praticas\PassoAPasso\Part2.png)

Agora, no "painel Carregar *blob*", **selecione os arquivos extraidos e depois clique em "carregar":**

![Imagem do passo a passo, sobre como achar os "o painel Carregar blob"](Praticas\PassoAPasso\Part3.png)

Depois que o ***upload* for concluído**, você poderá fechar o painel ***Upload blob***. Pois, seus documentos estão agora em seu **contêiner de armazenamento de avaliações de café**.

Com essa etapa feita, agora vamos **Indexar os documentos**

No [Portal do Azure](https://portal.azure.com), navegue até o recurso ***Azure AI Search***, na página Visão geral e selecione **Importar dados**.






## Referências

 - [DIO - Microsoft Azure AI Fundamentals](https://web.dio.me/track/a088cda7-a37f-451a-b392-46fa7e6ddc55)
 - [Explore an Azure AI Search index (UI)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html)

