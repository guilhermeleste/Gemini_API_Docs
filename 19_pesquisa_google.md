**Embasamento com a Pesquisa Google na API Gemini**

O recurso de **Embasamento com a Pesquisa Google** na API Gemini e no AI Studio é projetado para aprimorar a precisão e a atualidade das respostas do modelo. Quando ativado, além de fornecer respostas mais fundamentadas, a API Gemini retorna fontes de embasamento (links de suporte embutidos) e Sugestões da Pesquisa Google junto com o conteúdo da resposta. Essas sugestões direcionam os usuários aos resultados de pesquisa correspondentes à resposta fundamentada.

**Configurando o Embasamento com a Pesquisa Google**

A partir do Gemini 2.0, a Pesquisa Google está disponível como uma ferramenta integrada. Isso significa que o modelo pode decidir quando utilizar a Pesquisa Google. Abaixo está um exemplo de como configurar a Pesquisa como uma ferramenta utilizando o SDK do Python:

```python
from google import genai
from google.genai.types import Tool, GenerateContentConfig, GoogleSearch

# Inicializar o cliente com a chave de API
client = genai.Client(api_key='SUA_CHAVE_DE_API')

# Definir o modelo a ser utilizado
model_id = "gemini-2.0-flash"

# Configurar a ferramenta de Pesquisa Google
google_search_tool = Tool(
    google_search=GoogleSearch()
)

# Fazer uma solicitação ao modelo com a ferramenta de pesquisa configurada
response = client.models.generate_content(
    model=model_id,
    contents="Quando será o próximo eclipse solar total nos Estados Unidos?",
    config=GenerateContentConfig(
        tools=[google_search_tool],
        response_modalities=["TEXT"],
    )
)

# Exibir a resposta do modelo
for each in response.candidates[0].content.parts:
    print(each.text)

# Exibir o conteúdo da web utilizado para embasamento
print(response.candidates[0].grounding_metadata.search_entry_point.rendered_content)
```


**Sugestões da Pesquisa Google**

Para utilizar o Embasamento com a Pesquisa Google, é necessário exibir as Sugestões da Pesquisa Google, que são consultas sugeridas incluídas nos metadados da resposta fundamentada. Essas sugestões auxiliam os usuários a encontrar resultados de pesquisa correspondentes à resposta embasada. Ao implementar essas sugestões em seu aplicativo, assegure-se de que elas sejam exibidas de forma destacada e que, ao serem selecionadas, direcionem os usuários diretamente para a página de resultados de pesquisa do Google correspondente.

**Considerações Finais**

O Embasamento com a Pesquisa Google é uma funcionalidade poderosa que melhora a confiabilidade e a relevância das respostas geradas pelo modelo Gemini. Ao integrar essa funcionalidade, os desenvolvedores podem oferecer aos usuários respostas mais precisas e atualizadas, enriquecidas com fontes verificáveis e sugestões de pesquisa adicionais.



**Usar as Sugestões de Pesquisa do Google**

Para utilizar o recurso de embasamento com a Pesquisa Google, é necessário ativar as Sugestões de Pesquisa do Google. Essas sugestões auxiliam os usuários a encontrar resultados de pesquisa correspondentes a uma resposta embasada. Especificamente, é preciso exibir as consultas de pesquisa incluídas nos metadados da resposta embasada.

**Requisitos para Sugestões de Pesquisa do Google**

**O que fazer:**

- Exibir a sugestão de pesquisa exatamente como fornecida, sem modificações, em conformidade com os requisitos de exibição.

- Direcionar os usuários diretamente para a página de resultados da Pesquisa Google (SRP) ao interagirem com a sugestão de pesquisa.

**O que não fazer:**

- Incluir telas intermediárias ou etapas adicionais entre a interação do usuário e a exibição da SRP.

- Mostrar outros resultados ou sugestões de pesquisa ao lado da Sugestão de Pesquisa ou da resposta do LLM embasada associada.

**Requisitos de Exibição**

- Exibir a sugestão de pesquisa exatamente como fornecida, sem alterações nas cores, fontes ou aparência.

- Garantir que a sugestão de pesquisa seja renderizada conforme especificado nos modelos fornecidos, incluindo modos claro e escuro.

- Sempre que uma resposta embasada for exibida, a sugestão de pesquisa correspondente deve permanecer visível.

- **Branding**: É necessário seguir rigorosamente as Diretrizes do Google para o uso de características da marca por terceiros.

- As sugestões de pesquisa do Google devem ter, no mínimo, a largura total da resposta embasada.

**Comportamento ao Tocar**

- Ao tocar na sugestão de pesquisa, o usuário deve ser direcionado para uma página de resultados da Pesquisa Google (SRP) com o termo de pesquisa exibido na sugestão.

- A SRP pode ser aberta no navegador dentro do aplicativo ou em um navegador separado.

- É importante não minimizar, remover ou obstruir a exibição da SRP de qualquer forma.

**Código para Implementar uma Sugestão de Pesquisa do Google**

Ao usar a API para embasar uma resposta de pesquisa, a resposta do modelo fornece HTML e CSS compatíveis no campo `renderedContent`, que você implementa para mostrar sugestões de pesquisa no seu aplicativo. O HTML e o CSS fornecidos na resposta da API se adaptam automaticamente às configurações do dispositivo do usuário, sendo exibidos no modo claro ou escuro com base na preferência do usuário indicada por `@media(prefers-color-scheme)`.

