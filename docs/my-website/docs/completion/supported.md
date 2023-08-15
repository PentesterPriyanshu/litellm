# Generation/Completion/Chat Completion Models

### OpenAI Chat Completion Models

| Model Name       | Function Call                          | Required OS Variables                |
|------------------|----------------------------------------|--------------------------------------|
| gpt-3.5-turbo    | `completion('gpt-3.5-turbo', messages)` | `os.environ['OPENAI_API_KEY']`       |
| gpt-3.5-turbo-16k    | `completion('gpt-3.5-turbo-16k', messages)` | `os.environ['OPENAI_API_KEY']`       |
| gpt-3.5-turbo-16k-0613    | `completion('gpt-3.5-turbo-16k-0613', messages)` | `os.environ['OPENAI_API_KEY']`       |
| gpt-4            | `completion('gpt-4', messages)`         | `os.environ['OPENAI_API_KEY']`       |

## Azure OpenAI Chat Completion Models

| Model Name       | Function Call                           | Required OS Variables                     |
|------------------|-----------------------------------------|-------------------------------------------|
| gpt-3.5-turbo    | `completion('gpt-3.5-turbo', messages, azure=True)` | `os.environ['AZURE_API_KEY']`,`os.environ['AZURE_API_BASE']`,`os.environ['AZURE_API_VERSION']` |
| gpt-4            | `completion('gpt-4', messages, azure=True)`         | `os.environ['AZURE_API_KEY']`,`os.environ['AZURE_API_BASE']`,`os.environ['AZURE_API_VERSION']` |

### OpenAI Text Completion Models

| Model Name       | Function Call                              | Required OS Variables                |
|------------------|--------------------------------------------|--------------------------------------|
| text-davinci-003 | `completion('text-davinci-003', messages)` | `os.environ['OPENAI_API_KEY']`       |


### Google VertexAI Models
Sample notebook for calling VertexAI models: https://github.com/BerriAI/litellm/blob/main/cookbook/liteLLM_VertextAI_Example.ipynb

All calls using Vertex AI require the following parameters:
* Your Project ID
`litellm.vertex_project` = "hardy-device-38811" Your Project ID
* Your Project Location
`litellm.vertex_location` = "us-central1" 

Authentication:
VertexAI uses Application Default Credentials, see https://cloud.google.com/docs/authentication/external/set-up-adc for more information on setting this up

VertexAI requires you to set `application_default_credentials.json`, this can be set by running `gcloud auth application-default login` in your terminal

| Model Name       | Function Call                                            |
|------------------|----------------------------------------------------------|
| chat-bison       | `completion('chat-bison', messages)`                    |
| chat-bison@001   | `completion('chat-bison@001', messages)`                |
| text-bison       | `completion('text-bison', messages)`                    |
| text-bison@001   | `completion('text-bison@001', messages)`                |

### Anthropic Models

| Model Name       | Function Call                              | Required OS Variables                |
|------------------|--------------------------------------------|--------------------------------------|
| claude-instant-1  | `completion('claude-instant-1', messages)` | `os.environ['ANTHROPIC_API_KEY']`       |
| claude-2  | `completion('claude-2', messages)` | `os.environ['ANTHROPIC_API_KEY']`       |

### Hugging Face Inference API

All [`text2text-generation`](https://huggingface.co/models?library=transformers&pipeline_tag=text2text-generation&sort=downloads) and [`text-generation`](https://huggingface.co/models?library=transformers&pipeline_tag=text-generation&sort=downloads) models are supported by liteLLM. You can use any text model from Hugging Face with the following steps:

* Copy the `model repo` URL from Hugging Face and set it as the `model` parameter in the completion call.
* Set `hugging_face` parameter to `True`.
* Make sure to set the hugging face API key

Here are some examples of supported models:
**Note that the models mentioned in the table are examples, and you can use any text model available on Hugging Face by following the steps above.**

| Model Name       | Function Call                                                                       | Required OS Variables                |
|------------------|-------------------------------------------------------------------------------------|--------------------------------------|
| [stabilityai/stablecode-completion-alpha-3b-4k](https://huggingface.co/stabilityai/stablecode-completion-alpha-3b-4k)  | `completion(model="stabilityai/stablecode-completion-alpha-3b-4k", messages=messages, custom_llm_provider="huggingface")` | `os.environ['HF_TOKEN']`       |
| [bigcode/starcoder](https://huggingface.co/bigcode/starcoder)                           | `completion(model="bigcode/starcoder", messages=messages, custom_llm_provider="huggingface")`          | `os.environ['HF_TOKEN']`       |
| [google/flan-t5-xxl](https://huggingface.co/google/flan-t5-xxl)                         | `completion(model="google/flan-t5-xxl", messages=messages, custom_llm_provider="huggingface")`         | `os.environ['HF_TOKEN']`       |
| [google/flan-t5-large](https://huggingface.co/google/flan-t5-large)                     | `completion(model="google/flan-t5-large", messages=messages, custom_llm_provider="huggingface")`       | `os.environ['HF_TOKEN']`       |

### AI21 Models

| Model Name       | Function Call                              | Required OS Variables                |
|------------------|--------------------------------------------|--------------------------------------|
| j2-light  | `completion('j2-light', messages)` | `os.environ['AI21_API_KEY']`       |
| j2-mid  | `completion('j2-mid', messages)` | `os.environ['AI21_API_KEY']`       |
| j2-ultra  | `completion('j2-ultra', messages)` | `os.environ['AI21_API_KEY']`       |


### Cohere Models

| Model Name       | Function Call                              | Required OS Variables                |
|------------------|--------------------------------------------|--------------------------------------|
| command-nightly  | `completion('command-nightly', messages)` | `os.environ['COHERE_API_KEY']`       |

### OpenRouter Completion Models

All the text models from [OpenRouter](https://openrouter.ai/docs) are supported by liteLLM.

| Model Name       | Function Call                              | Required OS Variables                |
|------------------|--------------------------------------------|--------------------------------------|
| openai/gpt-3.5-turbo | `completion('openai/gpt-3.5-turbo', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| openai/gpt-3.5-turbo-16k | `completion('openai/gpt-3.5-turbo-16k', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| openai/gpt-4 | `completion('openai/gpt-4', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| openai/gpt-4-32k | `completion('openai/gpt-4-32k', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| anthropic/claude-2 | `completion('anthropic/claude-2', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| anthropic/claude-instant-v1 | `completion('anthropic/claude-instant-v1', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| google/palm-2-chat-bison | `completion('google/palm-2-chat-bison', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| google/palm-2-codechat-bison | `completion('google/palm-2-codechat-bison', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| meta-llama/llama-2-13b-chat | `completion('meta-llama/llama-2-13b-chat', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |
| meta-llama/llama-2-70b-chat | `completion('meta-llama/llama-2-70b-chat', messages)` | `os.environ['OR_SITE_URL']`,`os.environ['OR_APP_NAME']`,`os.environ['OR_API_KEY']`       |