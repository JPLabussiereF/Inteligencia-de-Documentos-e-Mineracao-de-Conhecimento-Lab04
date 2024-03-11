# Passo a passo, processo, insights e possibilidades aprendidas durante o conteúdo **"Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados"**

## **1. Passo a passo:**

**Primeiramente** vamos ter que criar três recursos no [Portal do Azure](https://portal.azure.com). Um Recurso do *Azure AI Search*, *IA do Azure* e armazenamento. 

Após esses três recursos criado, vamos para o a parte de **Carregar documentos para o armazenamento do Azure**, e no painel do menu esquerdo, selecione Containers. 

![Imagem do passo a passo, sobre como achar os "containers"](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part1.png?raw=true)

**Insira as seguintes configurações e clique e crie um novo Contêiner:**

- **Nome:** Coffee-Reviews.
- **Nível de acesso público:** Container (acesso de leitura anônimo para containers e blobs).
- **Avançado:** Sem alterações.

Agora, Em uma nova guia do navegador, baixe as **[avaliações de café compactadas](https://aka.ms/mslearn-coffee-reviews)** em `https://aka.ms/mslearn-coffee-reviewse` e extraia os arquivos para a pasta de avaliações.

![Imagem do passo a passo, sobre como achar "a pasta de avaliações"](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part2.png?raw=true)

Agora, no "painel Carregar *blob*", **selecione os arquivos extraidos e depois clique em "carregar":**

![Imagem do passo a passo, sobre como achar "o painel Carregar blob"](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part3.png?raw=true)

Depois que o ***upload* for concluído**, você poderá fechar o painel ***Upload blob***. Pois, seus documentos estão agora em seu **contêiner de armazenamento de avaliações de café**.

Com essa etapa feita, agora vamos **Indexar os documentos**

No [Portal do Azure](https://portal.azure.com), navegue até o recurso ***Azure AI Search***, na página Visão geral e selecione **Importar dados**.


![Imagem do passo a passo, sobre como achar a opção "importar dados"](Praticas\PassoAPasso\Part4.png)

Na página **Conectar-se aos seus dados**, na lista **Fonte de Dados**, selecione ***Azure Blob Storage***. Preencha os detalhes do armazenamento de dados com os seguintes valores:

- **Fonte de dados:** Armazenamento de Blobs do Azure.
- **Nome da fonte de dados:** coffee-customer-data.
- **Dados a extrair:** Conteúdo e metadados.
- **Modo de análise:** Padrão.
- **Cadeia de conexão:** *Selecione **Escolha uma conexão existente**. Selecione sua conta de armazenamento, selecione o contêiner **de avaliações de café** e clique em **Selecionar**.
- **Autenticação de identidade gerenciada:** Nenhuma.
- **Nome do contêiner:** esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente.
- **Pasta Blob:** deixe em branco.
- **Descrição:** Avaliações sobre Fourth Coffee Shops.

Depois, **Selecione Próximo: Adicionar habilidades cognitivas**, caso queira **(essa parte é opcional)**. Agora, secção **Anexar Serviços Cognitivos**, selecione o seu recurso de serviços *Azure AI*. E Na seção **Adicionar enriquecimentos** coloque as seguintes informações:

- Altere o **nome da qualificação** para **coffee-skillset**.
- Marque a caixa de seleção **Habilitar OCR e mesclar todo o texto no campo merged_content**.
> ❗ **Nota** É importante selecionar **Habilitar OCR** para ver todas as opções de campo enriquecido.

- Certifique-se de que o **campo Dados de origem** esteja configurado como **merged_content**.
- Altere o **nível de granularidade de enriquecimento** para **Páginas (blocos de 5.000 caracteres)**.
- Não selecione Habilitar enriquecimento incremental

**Selecione os seguintes campos enriquecidos:**

![Imagem do passo a passo, o que colocar em: "campos enriquecidos"](Praticas\PassoAPasso\Part5.png)

Em **Salvar enriquecimentos em um armazenamento de conhecimento**, selecione:

- Projeções de imagem
- Documentos
- Páginas
- Frases chave
- Entidades
- Detalhes da imagem
- Referências de imagem

![Imagem de observação](Praticas\PassoAPasso\Obs.png)

- Selecione **projeções de blob do Azure: Documento**. O qual, é uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento. Não altere o nome do contêiner.

- Selecione **Próximo: Personalizar índice de destino**. Altere o **nome do índice** para ***coffee-index***.

Agora, certifique-se de que a **chave** esteja configurada como **metadata_storage_path**. Deixe **o nome do sugeridor** em branco e o **modo de pesquisa** preenchido automaticamente.

Por fim, revise as configurações padrão dos campos de índice. Selecione **filtrável** para todos os campos que já estão selecionados por padrão. E selecione **Próximo: Criar um indexador**.

Altere o **nome do indexador** para **coffee-indexer** e deixe a **programação** definida como **Once(Uma vez)**. **Expanda as opções avançadas** e certifique-se de que a opção ***Base-64 Encode Keys*** esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente, e depois selecione Enviar para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:

- Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
- Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
- Mapeia os campos extraídos para o índice.

Volte à página de recursos do *Azure AI Search*. No painel esquerdo, em **Gerenciamento de pesquisa**, selecione **Indexadores**. Selecione o **indexador de café** recém-criado. Espere um minuto e selecione **↻ Atualize** até que o **Status** indique sucesso. Depois disso, voce pode selecionar o nome do indexador para ver mais detalhes.

![Mais detalhes do indexador](Praticas\PassoAPasso\Part6.png)

Agora, vamos aprender a **Consultar o índice**

Na página Visão geral do serviço de pesquisa, selecione **Explorador de pesquisa** na parte superior da tela.

![Explorador de pesquisa](Praticas\PassoAPasso\Part7.png)

Observe como o índice selecionado é o índice de café que você criou. Abaixo do índice selecionado, altere a visualização para **JSON view**.

![JSON view](Praticas\PassoAPasso\Part8.png)

No campo **do editor de consultas JSON**, copie e cole:

```
{
    "search": "*",
    "count": true
}
```

Selecione **Pesquisar**. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo ***@odata.count***. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.

Agora vamos filtrar por localização. No campo **do editor de consultas JSON**, copie e cole:

```
{
 "search": "locations:'Chicago'",
 "count": true
}
```

Selecione **Pesquisar**. A consulta pesquisa todos os documentos no índice e filtra revisões com localização em Chicago. Você deveria ver `3` no `@odata.count` campo.

Agora vamos filtrar por sentimento. No campo **do editor de consultas JSON**, copie e cole:

```
{
 "search": "sentiment:'negative'",
 "count": true
}
```

Selecione **Pesquisar**. A consulta pesquisa todos os documentos no índice e filtra revisões com sentimento negativo. Você deveria ver `1` no `@odata.count` campo.

> ❗ **Nota** Veja como os resultados são classificados por **`@search.score`**. Esta é a pontuação atribuída pelo mecanismo de pesquisa para mostrar o quão próximos os resultados correspondem à consulta fornecida.

E por fim, vamos **revisar o armazenamento de conhecimento**, para finalizar a prática.

No portal do Azure, navegue de volta para a sua conta de armazenamento do Azure, No painel do menu esquerdo, selecione ***Containers***. Selecione o contêiner **de armazenamento de conhecimento**.

![Armazenamento de conhecimento](Praticas\PassoAPasso\part9.png)

Em seguida, selecione qualquer um dos itens e clique no arquivo **objectprojection.json**.

![objectprojection.json](Praticas\PassoAPasso\Part10.png)

E selecione **Editar** para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.

![Editar](Praticas\PassoAPasso\Part11.png)

Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar à conta de armazenamento Containers.

![Rertornar](Praticas\PassoAPasso\Part12.png)

Em Containers, selecione o contêiner coffee-skillset-image-projection. Selecione qualquer um dos itens.

![coffee-skillset-image-projection](Praticas\PassoAPasso\Part13.png)

Selecione qualquer um dos arquivos **.jpg**. Selecione **Editar** para ver a imagem armazenada no documento. Observe como todas as imagens dos documentos são armazenadas desta forma.

Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar à conta de armazenamento Containers.

Selecione **Navegador de armazenamento** no painel esquerdo e selecione **Tabelas**. Há uma tabela para cada entidade no índice. Selecione a tabela coffeeSkillsetKeyPhrases.

![Tabela](Praticas\PassoAPasso\Part14.png)

- Observe as frases-chave que o armazenamento de conhecimento conseguiu capturar do conteúdo das avaliações. Muitos dos campos são chaves, portanto você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.

















## Referências

 - [DIO - Microsoft Azure AI Fundamentals](https://web.dio.me/track/a088cda7-a37f-451a-b392-46fa7e6ddc55)
 - [Explore an Azure AI Search index (UI)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html)

