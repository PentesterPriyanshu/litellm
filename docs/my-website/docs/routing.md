# Reliability - Model Fallbacks, Manage Multiple Deployments

## Model Fallbacks
Never fail a request using LiteLLM, LiteLLM allows you to define fallback models for completion requests
```python
from litellm import completion
# if gpt-4 fails, retry the request with gpt-3.5-turbo->command-nightly->claude-instant-1
response = completion(model="gpt-4",messages=messages, fallbacks=["gpt-3.5-turbo" "command-nightly", "claude-instant-1"])

# if azure/gpt-4 fails, retry the request with fallback api_keys/api_base
response = completion(model="azure/gpt-4", messages=messages, api_key=api_key, fallbacks=[{"api_key": "good-key-1"}, {"api_key": "good-key-2", "api_base": "good-api-base-2"}])

```

## Manage Multiple Deployments

Use this if you're trying to load-balance across multiple deployments (e.g. Azure/OpenAI). 

`Router` prevents failed requests, by picking the deployment which is below rate-limit and has the least amount of tokens used. 

In production, [Router connects to a Redis Cache](#redis-queue) to track usage across multiple deployments.

### Quick Start

```python
from litellm import Router

model_list = [{ # list of model deployments 
	"model_name": "gpt-3.5-turbo", # openai model name 
	"litellm_params": { # params for litellm completion/embedding call 
		"model": "azure/chatgpt-v-2", 
		"api_key": os.getenv("AZURE_API_KEY"),
		"api_version": os.getenv("AZURE_API_VERSION"),
		"api_base": os.getenv("AZURE_API_BASE")
	},
	"tpm": 240000,
	"rpm": 1800
}, {
    "model_name": "gpt-3.5-turbo", # openai model name 
	"litellm_params": { # params for litellm completion/embedding call 
		"model": "azure/chatgpt-functioncalling", 
		"api_key": os.getenv("AZURE_API_KEY"),
		"api_version": os.getenv("AZURE_API_VERSION"),
		"api_base": os.getenv("AZURE_API_BASE")
	},
	"tpm": 240000,
	"rpm": 1800
}, {
    "model_name": "gpt-3.5-turbo", # openai model name 
	"litellm_params": { # params for litellm completion/embedding call 
		"model": "gpt-3.5-turbo", 
		"api_key": os.getenv("OPENAI_API_KEY"),
	},
	"tpm": 1000000,
	"rpm": 9000
}]

router = Router(model_list=model_list)

# openai.ChatCompletion.create replacement
response = router.completion(model="gpt-3.5-turbo", 
				messages=[{"role": "user", "content": "Hey, how's it going?"}]

print(response)
```

### Redis Queue 

In production, we use Redis to track usage across multiple Azure deployments.

```python
router = Router(model_list=model_list, 
                redis_host=os.getenv("REDIS_HOST"), 
                redis_password=os.getenv("REDIS_PASSWORD"), 
                redis_port=os.getenv("REDIS_PORT"))

print(response)
```

### Deploy Router 

1. Clone repo
```shell
 git clone https://github.com/BerriAI/litellm
```

2. Create + Modify router_config.yaml (save your azure/openai/etc. deployment info)

```shell
cp ./router_config_template.yaml ./router_config.yaml
```

3. Build + Run docker image 

```shell
docker build -t litellm-proxy . --build-arg CONFIG_FILE=./router_config.yaml 
```

```shell
docker run --name litellm-proxy -e PORT=8000 -p 8000:8000 litellm-proxy
```

### Test 

```curl
curl 'http://0.0.0.0:8000/router/completions' \
--header 'Content-Type: application/json' \
--data '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hey"}]
}'
```