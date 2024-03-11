# Passo a passo, processo, insights e possibilidades aprendidas durante o conteúdo **"Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados"**

## **1. Passo a passo**

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


![Imagem do passo a passo, sobre como achar a opção "importar dados"](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part4.png?raw=true)

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

![Imagem do passo a passo, o que colocar em: "campos enriquecidos"](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part5.png?raw=true)

Em **Salvar enriquecimentos em um armazenamento de conhecimento**, selecione:

- Projeções de imagem
- Documentos
- Páginas
- Frases chave
- Entidades
- Detalhes da imagem
- Referências de imagem

![Imagem de observação](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Obs.png?raw=true)

- Selecione **projeções de blob do Azure: Documento**. O qual, é uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento. Não altere o nome do contêiner.

- Selecione **Próximo: Personalizar índice de destino**. Altere o **nome do índice** para ***coffee-index***.

Agora, certifique-se de que a **chave** esteja configurada como **metadata_storage_path**. Deixe **o nome do sugeridor** em branco e o **modo de pesquisa** preenchido automaticamente.

Por fim, revise as configurações padrão dos campos de índice. Selecione **filtrável** para todos os campos que já estão selecionados por padrão. E selecione **Próximo: Criar um indexador**.

Altere o **nome do indexador** para **coffee-indexer** e deixe a **programação** definida como **Once(Uma vez)**. **Expanda as opções avançadas** e certifique-se de que a opção ***Base-64 Encode Keys*** esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente, e depois selecione Enviar para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:

- Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
- Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
- Mapeia os campos extraídos para o índice.

Volte à página de recursos do *Azure AI Search*. No painel esquerdo, em **Gerenciamento de pesquisa**, selecione **Indexadores**. Selecione o **indexador de café** recém-criado. Espere um minuto e selecione **↻ Atualize** até que o **Status** indique sucesso. Depois disso, voce pode selecionar o nome do indexador para ver mais detalhes.

![Mais detalhes do indexador](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part6.png?raw=true)

Agora, vamos aprender a **Consultar o índice**

Na página Visão geral do serviço de pesquisa, selecione **Explorador de pesquisa** na parte superior da tela.

![Explorador de pesquisa](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part7.png?raw=true)

Observe como o índice selecionado é o índice de café que você criou. Abaixo do índice selecionado, altere a visualização para **JSON view**.

![JSON view](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part8.png?raw=true)

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

![Armazenamento de conhecimento](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part9.png?raw=true)

Em seguida, selecione qualquer um dos itens e clique no arquivo **objectprojection.json**.

![objectprojection.json](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part10.png?raw=true)

E selecione **Editar** para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.

![Editar](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part11.png?raw=true)

Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar à conta de armazenamento Containers.

![Rertornar](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part12.png?raw=true)

Em Containers, selecione o contêiner coffee-skillset-image-projection. Selecione qualquer um dos itens.

![coffee-skillset-image-projection](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part13.png?raw=true)

Selecione qualquer um dos arquivos **.jpg**. Selecione **Editar** para ver a imagem armazenada no documento. Observe como todas as imagens dos documentos são armazenadas desta forma.

Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar à conta de armazenamento Containers.

Selecione **Navegador de armazenamento** no painel esquerdo e selecione **Tabelas**. Há uma tabela para cada entidade no índice. Selecione a tabela coffeeSkillsetKeyPhrases.

![Tabela](https://github.com/JPLabussiereF/Inteligencia-de-Documentos-e-Mineracao-de-Conhecimento-Lab04/blob/main/Praticas/PassoAPasso/Part14.png?raw=true)

- Observe as frases-chave que o armazenamento de conhecimento conseguiu capturar do conteúdo das avaliações. Muitos dos campos são chaves, portanto você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.

## **2. Utilização AI Search para indexação e consulta de Dados**


A utilização da AI (Inteligência Artificial) para indexação e consulta de dados, conhecida como AI Search, é uma abordagem avançada para encontrar e acessar informações relevantes em grandes conjuntos de dados. Ao contrário dos métodos tradicionais de pesquisa que dependem de palavras-chave ou estruturas de dados predefinidas, o AI Search emprega algoritmos de aprendizado de máquina e processamento de linguagem natural para entender o conteúdo e o contexto dos dados.

Na indexação, a AI Search analisa e organiza os dados de forma inteligente, identificando padrões, relacionamentos e significados semânticos. Isso permite uma indexação mais precisa e uma recuperação mais eficiente de informações relevantes durante as consultas.

Durante a consulta, a AI Search interpreta as perguntas ou consultas dos usuários de forma mais sofisticada, considerando o contexto e a intenção por trás das palavras-chave. Isso resulta em resultados de pesquisa mais relevantes e personalizados, mesmo em conjuntos de dados complexos e não estruturados.

A AI Search é amplamente utilizada em uma variedade de contextos, como motores de busca na web, sistemas de gerenciamento de conteúdo, plataformas de comércio eletrônico e assistentes virtuais. Sua capacidade de compreender o significado subjacente dos dados torna-a uma ferramenta poderosa para a descoberta e a análise de informações em um mundo cada vez mais rico em dados.

## **3. Recomendação de Tópicos para estudos**

- **Algoritmos de Indexação:** Explore os algoritmos utilizados na indexação de dados, como indexação invertida, árvores B e hashing.

- **Processamento de Linguagem Natural (PLN):** Aprofunde-se nos conceitos e técnicas de PLN aplicados à compreensão de consultas de pesquisa e ao entendimento de documentos.

- **Aprendizado de Máquina para Recomendação:** Estude como os modelos de aprendizado de máquina são usados para recomendar resultados de pesquisa personalizados com base no comportamento do usuário e no contexto.

- **Estratégias de Recuperação de Informações:** Analise as estratégias e técnicas para recuperar informações relevantes em grandes conjuntos de dados, incluindo busca booleana, busca vetorial e busca probabilística.

- **Ferramentas e Plataformas de AI Search:** Explore as principais ferramentas e plataformas disponíveis para implementar sistemas de AI Search, como Elasticsearch, Apache Solr, Algolia, entre outras.

- **Desafios e Soluções em AI Search:** Estude os desafios enfrentados na implementação de sistemas de AI Search, como escalabilidade, precisão da pesquisa e interpretação de consultas complexas, e aprenda sobre as soluções propostas para superar esses desafios.

- **Aplicações em Setores Específicos:** Pesquise como o AI Search é aplicado em setores específicos, como saúde, comércio eletrônico, finanças, mídia e entretenimento, para melhorar a descoberta e a análise de dados.

- **Ética e Privacidade em AI Search:** Explore as questões éticas e de privacidade relacionadas ao uso de AI Search, como a responsabilidade dos algoritmos, a transparência nos resultados da pesquisa e o uso ético dos dados do usuário.

**Esses tópicos fornecem uma base sólida, bons estudos!**

## Referências

 - [DIO - Microsoft Azure AI Fundamentals](https://web.dio.me/track/a088cda7-a37f-451a-b392-46fa7e6ddc55)
 - [Explore an Azure AI Search index (UI)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html)

