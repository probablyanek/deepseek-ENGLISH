# DeepSeek V3 Free Service

[![](https://img.shields.io/github/license/llm-red-team/deepseek-free-api.svg)](LICENSE)
![](https://img.shields.io/github/stars/llm-red-team/deepseek-free-api.svg)
![](https://img.shields.io/github/forks/llm-red-team/deepseek-free-api.svg)
![](https://img.shields.io/docker/pulls/vinlic/deepseek-free-api.svg)

Supports high-speed streaming output, multi-turn dialogue, internet search, R1 deep thinking, and silent deep thinking. Zero-configuration deployment with multi-token support.

Fully compatible with ChatGPT interface.

Also, check out the following ten free APIs:

Moonshot AI (Kimi.ai) interface to API [kimi-free-api](https://github.com/LLM-Red-Team/kimi-free-api)

Zhipu AI (Zhipu Qingyan) interface to API [glm-free-api](https://github.com/LLM-Red-Team/glm-free-api)

Step Star (Yuewen StepChat) interface to API [step-free-api](https://github.com/LLM-Red-Team/step-free-api)

Ali Tongyi (Qwen) interface to API [qwen-free-api](https://github.com/LLM-Red-Team/qwen-free-api)

MetaSo AI (Metaso) interface to API [metaso-free-api](https://github.com/LLM-Red-Team/metaso-free-api)

ByteDance (Doubao) interface to API [doubao-free-api](https://github.com/LLM-Red-Team/doubao-free-api)

ByteDance (Jimeng AI) interface to API [jimeng-free-api](https://github.com/LLM-Red-Team/jimeng-free-api)

iFlytek Spark (Spark) interface to API [spark-free-api](https://github.com/LLM-Red-Team/spark-free-api)

MiniMax (Hailuo AI) interface to API [hailuo-free-api](https://github.com/LLM-Red-Team/hailuo-free-api)

Emohaa AI (Emohaa) interface to API [emohaa-free-api](https://github.com/LLM-Red-Team/emohaa-free-api)

## Table of Contents

* [Disclaimer](#disclaimer)
* [Effect Examples](#effect-examples)
* [Preparation](#preparation)
  * [Multi-Account Access](#multi-account-access)
* [Docker Deployment](#docker-deployment)
  * [Docker-compose Deployment](#docker-compose-deployment)
* [Render Deployment](#render-deployment)
* [Vercel Deployment](#vercel-deployment)
* [Native Deployment](#native-deployment)
* [Recommended Clients](#recommended-clients)
* [API List](#api-list)
  * [Dialogue Completion](#dialogue-completion)
  * [userToken Survival Detection](#usertoken-survival-detection)
* [Notes](#notes)
  * [Nginx Reverse Proxy Optimization](#nginx-reverse-proxy-optimization)
  * [Token Statistics](#token-statistics)
* [Star History](#star-history)
  
## Disclaimer

**Reverse engineering APIs is unstable. It is recommended to use the official DeepSeek API at https://platform.deepseek.com/ for paid usage to avoid the risk of being banned.**

**This organization and individuals do not accept any financial donations or transactions. This project is purely for research, exchange, and learning purposes!**

**For personal use only. Do not provide external services or commercial use to avoid putting pressure on the official service, otherwise bear the risk yourself!**

**For personal use only. Do not provide external services or commercial use to avoid putting pressure on the official service, otherwise bear the risk yourself!**

**For personal use only. Do not provide external services or commercial use to avoid putting pressure on the official service, otherwise bear the risk yourself!**

## Effect Examples

### Identity Verification Demo

![Identity Verification](./doc/example-1.png)

### Multi-turn Dialogue Demo

![Multi-turn Dialogue](./doc/example-2.png)

### Internet Search Demo

![Internet Search](./doc/example-3.png)

## Preparation

Ensure you are within China or have a server within China, otherwise the deployment may not be able to access DeepSeek and thus be unusable.

Obtain the userToken value from [DeepSeek](https://chat.deepseek.com/)

Initiate any dialogue in DeepSeek, then open the developer tools with F12, and find the `userToken` value in Application > LocalStorage. This will be used as the Bearer Token value for Authorization: `Authorization: Bearer TOKEN`

![Obtain userToken](./doc/example-0.png)

### Multi-Account Access

Currently, the same account can only have *one* output at a time. You can provide multiple account userToken values and concatenate them with `,`:

`Authorization: Bearer TOKEN1,TOKEN2,TOKEN3`

Each request will select one from them.

## Docker Deployment

Prepare a server with a public IP and open port 8000.

Pull the image and start the service

```shell
docker run -it -d --init --name deepseek-free-api -p 8000:8000 -e TZ=Asia/Shanghai vinlic/deepseek-free-api:latest
```

View real-time service logs

```shell
docker logs -f deepseek-free-api
```

Restart the service

```shell
docker restart deepseek-free-api
```

Stop the service

```shell
docker stop deepseek-free-api
```

### Docker-compose Deployment

```yaml
version: '3'

services:
  deepseek-free-api:
    container_name: deepseek-free-api
    image: vinlic/deepseek-free-api:latest
    restart: always
    ports:
      - "8000:8000"
    environment:
      - TZ=Asia/Shanghai
```

### Render Deployment

**Note: Some deployment regions may not be able to connect to deepseek. If container logs show request timeouts or connection failures, please switch to other regions for deployment!**
**Note: Free account container instances will automatically stop after a period of inactivity, which may cause delays of 50 seconds or more for the next request. It is recommended to check [Render Container Keep-Alive](https://github.com/LLM-Red-Team/free-api-hub/#Render%E5%AE%B9%E5%99%A8%E4%BF%9D%E6%B4%BB)**

1. Fork this project to your GitHub account.

2. Visit [Render](https://dashboard.render.com/) and log in with your GitHub account.

3. Build your Web Service (New+ -> Build and deploy from a Git repository -> Connect your forked project -> Select deployment region -> Choose Free instance type -> Create Web Service).

4. After the build is complete, copy the assigned domain name and concatenate the URL to access.

### Vercel Deployment

**Note: Vercel free account request response timeout is 10 seconds, but the interface response is usually longer, which may result in a 504 timeout error from Vercel!**

Ensure you have the Node.js environment installed.

```shell
npm i -g vercel --registry http://registry.npmmirror.com
vercel login
git clone https://github.com/LLM-Red-Team/deepseek-free-api
cd deepseek-free-api
vercel --prod
```

## Native Deployment

Prepare a server with a public IP and open port 8000.

Ensure the Node.js environment is installed and configured, and the node command is available.

Install dependencies

```shell
npm i
```

Install PM2 for process management

```shell
npm i -g pm2
```

Build the project, the dist directory will be created upon successful build

```shell
npm run build
```

Start the service

```shell
pm2 start dist/index.js --name "deepseek-free-api"
```

View real-time service logs

```shell
pm2 logs deepseek-free-api
```

Restart the service

```shell
pm2 reload deepseek-free-api
```

Stop the service

```shell
pm2 stop deepseek-free-api
```

## Recommended Clients

Using the following secondary development clients to access the free-api series projects is faster and simpler, supporting document/image uploads!

LobeChat secondary developed by [Clivia](https://github.com/Yanyutin753/lobe-chat) [https://github.com/Yanyutin753/lobe-chat](https://github.com/Yanyutin753/lobe-chat)

ChatGPT Web secondary developed by [时光@](https://github.com/SuYxh) [https://github.com/SuYxh/chatgpt-web-sea](https://github.com/SuYxh/chatgpt-web-sea)

## API List

Currently supports the `/v1/chat/completions` interface compatible with openai. You can use openai or other compatible clients to access the interface, or use services like [dify](https://dify.ai/) to access.

### Dialogue Completion

Dialogue completion interface, compatible with openai's [chat-completions-api](https://platform.openai.com/docs/guides/text-generation/chat-completions-api).

**POST /v1/chat/completions**

The header needs to set the Authorization header:

```
Authorization: Bearer [userToken value]
```

Request data:
```json
{
    // model name
    // default: deepseek
    // deep thinking: deepseek-think or deepseek-r1
    // internet search: deepseek-search
    // silent mode (no output of thinking process or internet search results): deepseek-think-silent or deepseek-r1-silent or deepseek-search-silent
    // deep thinking but the thinking process is wrapped in <details> foldable tags (requires page support): deepseek-think-fold or deepseek-r1-fold
    "model": "deepseek",
    // default multi-turn dialogue is based on message merging, which may lead to reduced capabilities in some scenarios and is limited by the maximum token count per turn
    // if you want to get the native multi-turn dialogue experience, you can pass the id obtained from the previous message to continue the context
    // "conversation_id": "50207e56-747e-4800-9068-c6fd618374ee@2",
    "messages": [
        {
            "role": "user",
            "content": "Who are you?"
        }
    ],
    // set to true if using streaming response, default false
    "stream": false
}
```

Response data:
```json
{
    "id": "50207e56-747e-4800-9068-c6fd618374ee@2",
    "model": "deepseek",
    "object": "chat.completion",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": " I am DeepSeek Chat, an intelligent assistant developed by DeepSeek Company, aiming to provide information query, dialogue exchange, and question answering services through natural language processing and machine learning technologies."
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 1,
        "completion_tokens": 1,
        "total_tokens": 2
    },
    "created": 1715061432
}
```

### userToken Survival Detection

Check if the userToken is alive. If alive, live is true, otherwise false. Do not call this interface frequently (less than 10 minutes).

**POST /token/check**

Request data:
```json
{
    "token": "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9..."
}
```

Response data:
```json
{
    "live": true
}
```

## Notes

### Nginx Reverse Proxy Optimization

If you are using Nginx to reverse proxy deepseek-free-api, add the following configuration items to optimize the streaming output effect and improve the experience.

```nginx
# Turn off proxy buffering. When set to off, Nginx will immediately send client requests to the backend server and immediately send the response received from the backend server back to the client.
proxy_buffering off;
# Enable chunked transfer encoding. Chunked transfer encoding allows the server to send data in chunks for dynamically generated content without needing to know the size of the content in advance.
chunked_transfer_encoding on;
# Enable TCP_NOPUSH, which tells Nginx to send data as much as possible before sending packets to the client. This is usually used with sendfile to improve network efficiency.
tcp_nopush on;
# Enable TCP_NODELAY, which tells Nginx not to delay sending data and to send small packets immediately. In some cases, this can reduce network latency.
tcp_nodelay on;
# Set the keep-alive timeout, here set to 120 seconds. If there is no further communication between the client and server within this time, the connection will be closed.
keepalive_timeout 120;
```

### Token Statistics

Since the inference side is not in deepseek-free-api, tokens cannot be counted and will return a fixed number.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=LLM-Red-Team/deepseek-free-api&type=Date)](https://star-history.com/#LLM-Red-Team/deepseek-free-api&Date)
