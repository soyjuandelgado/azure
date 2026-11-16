# Ejercicios AI-901

https://microsoftlearning.github.io/mslearn-ai-fundamentals/

## 1 - Get started with Microsoft Foundry

## 2 - Get started with generative AI and agents in Microsoft Foundry

1. Crear primero un grupo de recursos en el portal de azure. Las localizaciones válidas pueden ser:
	1. West US
	2. East US
2. En la página de Foundry (https://ai.azure.com), crear un nuevo proyecto Foundry
3. Desde la Home, hay que entrar en Catalog y aquí en la pestaña Models
4. Hay que seleccionar `gpt-5.1` y al hacer el Deploy :
	1. Tokens per Minute Rate Limit = 5000
5. Seguir los pasos hasta crear un agente. Si el boton crear agente está deshabilitado, volver al inicio y cambiar la localización
6. Este es el esquema de mi agente:
 ```yaml
 metadata:
  logo: Avatar_Default.svg
  microsoft.voice-live.enabled: "false"
object: agent.version
id: computing-historian:1
name: computing-historian
version: "1"
description: ""
created_at: 1794827617
definition:
  kind: prompt
  model: gpt-5.1
  instructions: You are an expert in the history of computing and AI. You only answer questions about significant people and events in the development of computing, and about notable vintage computers. Do not engage in conversations on any topic that is unrelated to computing history.
  reasoning:
    effort: low
    summary: detailed
  tools:
    - type: web_search
    - type: file_search
      vector_store_ids:
        - vs_cClDWSkp7Zx68Jex9p4xkyJg
draft: false
status: active
instance_identity:
  principal_id: c6447ec9-d5a2-4f16-8cc3-72c7715efe18
  client_id: c6447ec9-d5a2-4f16-8cc3-72c7715efe18
blueprint:
  principal_id: 03cfe0b2-5ddb-4004-9b0f-87f4294ea6f9
  client_id: 09567b3d-c8e7-4aa3-b490-029acba886af
blueprint_reference:
  type: ManagedAgentIdentityBlueprint
  blueprint_id: computing-historian-0d8d0
agent_guid: 0d8d07d3-ef68-4be0-a99b-a642f3cf8006

 ```

Este es el código de implementacion:

```python
# Before running the sample:
#    pip install "azure-ai-projects>=2.1.0"

from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = "https://xlab-ftd-245-6685-resource.services.ai.azure.com/api/projects/xlab-ftd-245-6685"
agent_name = "computing-historian"
user_message = "Hi computing-historian"

with (
    DefaultAzureCredential() as credential,
    AIProjectClient(
        endpoint=endpoint,
        credential=credential,
        allow_preview=True,
    ) as project_client,
):
    # Bind the OpenAI client to the existing agent endpoint.
    openai_client = project_client.get_openai_client(agent_name=agent_name)

    # GitHub Copilot preview agents require a conversation for every Responses turn.
    conversation = openai_client.conversations.create()
    response = openai_client.responses.create(
        conversation=conversation.id,
        input=[{"role": "user", "content": user_message}],
    )

    print(f"Response output: {response.output_text}")
```

o en C#:

```C#
// dotnet add package Azure.AI.Projects --version 2.1.0-beta.4
// dotnet add package Azure.Identity --version 1.21.0
using Azure.AI.Projects;
using Azure.AI.Extensions.OpenAI;
using Azure.Identity;
using OpenAI;
using OpenAI.Responses;

#pragma warning disable OPENAI001

const string endpoint = "https://xlab-ftd-245-6685-resource.services.ai.azure.com/api/projects/xlab-ftd-245-6685";
const string agentName = "computing-historian";
const string userMessage = "Hi computing-historian";

// Connect to your project using the endpoint from your project page.
// DefaultAzureCredential can use your Azure CLI identity; run `az login` for local authentication.
AIProjectClient projectClient = new(endpoint: new Uri(endpoint), tokenProvider: new DefaultAzureCredential());

ProjectResponsesClient agentClient =
    projectClient.ProjectOpenAIClient.GetProjectResponsesClientForAgentEndpoint(agentName);

// Reuse the agent-scoped SDK pipeline for conversations as well as responses.
ProjectConversationsClient conversationClient = new(
    agentClient.Pipeline,
    new OpenAIClientOptions { Endpoint = agentClient.Endpoint });
ProjectConversation conversation = conversationClient.CreateProjectConversation();

ProjectResponsesClient responseClient =
    projectClient.ProjectOpenAIClient.GetProjectResponsesClientForAgentEndpoint(
        agentName, defaultConversationId: conversation.Id);

ResponseResult response = responseClient.CreateResponse(userMessage);

Console.WriteLine(response.GetOutputText());
```

## 3 - Get started with text analysis in Microsoft Foundry

1. Crear grupo de recursos en Azure con location East-US
2. Ejecutar todo siguiendo el documento
3. Este es el código resultado del detector de PII:

 ```python
 key = "<your-api-key>"
endpoint = "https://xlab-ftd-245-1743-resource.cognitiveservices.azure.com/"

from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

# Authenticate the client using your key and endpoint 
def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, 
            credential=ta_credential)
    return text_analytics_client

client = authenticate_client()

# Example method for detecting sensitive information (PII) from text 
def pii_recognition_example(client):
    documents = [
        "$documents"
    ]
    response = client.recognize_pii_entities(documents, language="en")
    result = [doc for doc in response if not doc.is_error]
    for doc in result:
        print("Redacted Text: {}".format(doc.redacted_text))
        for entity in doc.entities:
            print("Entity: {}".format(entity.text))
            print("	Category: {}".format(entity.category))
            print("	Confidence Score: {}".format(entity.confidence_score))
            print("	Offset: {}".format(entity.offset))
            print("	Length: {}".format(entity.length))
pii_recognition_example(client)

 ```

o en C#:

```C#
using Azure;
using System;
using Azure.AI.TextAnalytics;

namespace Example
{
    class Program
    {
        private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<your-api-key>");
        private static readonly Uri endpoint = new Uri("https://xlab-ftd-245-1743-resource.cognitiveservices.azure.com/");

        // Example method for detecting sensitive information (PII) from text 
        static void RecognizePIIExample(TextAnalyticsClient client)
        {
            string document = "$documents";
        
            PiiEntityCollection entities = client.RecognizePiiEntities(document).Value;
        
            Console.WriteLine($"Redacted Text: {entities.RedactedText}");
            if (entities.Count > 0)
            {
                Console.WriteLine($"Recognized {entities.Count} PII entit{(entities.Count > 1 ? "ies" : "y")}:");
                foreach (PiiEntity entity in entities)
                {
                    Console.WriteLine($"Text: {entity.Text}, Category: {entity.Category}, SubCategory: {entity.SubCategory}, Confidence score: {entity.ConfidenceScore}");
                }
            }
            else
            {
                Console.WriteLine("No entities were found.");
            }
        }

        static void Main(string[] args)
        {
            var client = new TextAnalyticsClient(endpoint, credentials);
            RecognizePIIExample(client);

            Console.Write("Press any key to exit.");
            Console.ReadKey();
        }

    }
}
```


## 4 - Get started with speech in Microsoft Foundry

Para crear el agente, es mejor crear un modelo previamente o seleccionarlo despues, para que funcione correctamente. 

Las variables son las de antes:
- location: East US
- Tokens: 5000

```c#
// dotnet add package Azure.AI.Projects --version 2.1.0-beta.4
// dotnet add package Azure.Identity --version 1.21.0
using Azure.AI.Projects;
using Azure.AI.Extensions.OpenAI;
using Azure.Identity;
using OpenAI;
using OpenAI.Responses;

#pragma warning disable OPENAI001

const string endpoint = "https://xlab-ftd-245-1618-resource.services.ai.azure.com/api/projects/xlab-ftd-245-1618";
const string agentName = "speech-agent";
const string userMessage = "Hi speech-agent";

// Connect to your project using the endpoint from your project page.
// DefaultAzureCredential can use your Azure CLI identity; run `az login` for local authentication.
AIProjectClient projectClient = new(endpoint: new Uri(endpoint), tokenProvider: new DefaultAzureCredential());

ProjectResponsesClient agentClient =
    projectClient.ProjectOpenAIClient.GetProjectResponsesClientForAgentEndpoint(agentName);

// Reuse the agent-scoped SDK pipeline for conversations as well as responses.
ProjectConversationsClient conversationClient = new(
    agentClient.Pipeline,
    new OpenAIClientOptions { Endpoint = agentClient.Endpoint });
ProjectConversation conversation = conversationClient.CreateProjectConversation();

ProjectResponsesClient responseClient =
    projectClient.ProjectOpenAIClient.GetProjectResponsesClientForAgentEndpoint(
        agentName, defaultConversationId: conversation.Id);

ResponseResult response = responseClient.CreateResponse(userMessage);

Console.WriteLine(response.GetOutputText());
```
