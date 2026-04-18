---
title: Local-Ai-Api
date: 2026-04-19
author: Your Name
cell_count: 9
score: 5
---

```python
#Install required packages
!pip install fastapi uvicorn nest_asyncio llama-cpp-python
```

    Requirement already satisfied: fastapi in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (0.135.3)
    Requirement already satisfied: uvicorn in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (0.44.0)
    Requirement already satisfied: nest_asyncio in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (1.6.0)
    Requirement already satisfied: llama-cpp-python in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (0.3.19)
    Requirement already satisfied: starlette>=0.46.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from fastapi) (1.0.0)
    Requirement already satisfied: pydantic>=2.9.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from fastapi) (2.12.4)
    Requirement already satisfied: typing-extensions>=4.8.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from fastapi) (4.15.0)
    Requirement already satisfied: typing-inspection>=0.4.2 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from fastapi) (0.4.2)
    Requirement already satisfied: annotated-doc>=0.0.2 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from fastapi) (0.0.4)
    Requirement already satisfied: click>=7.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from uvicorn) (8.2.1)
    Requirement already satisfied: h11>=0.8 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from uvicorn) (0.16.0)
    Requirement already satisfied: numpy>=1.20.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from llama-cpp-python) (2.4.4)
    Requirement already satisfied: diskcache>=5.6.1 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from llama-cpp-python) (5.6.3)
    Requirement already satisfied: jinja2>=2.11.3 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from llama-cpp-python) (3.1.6)
    Requirement already satisfied: colorama in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from click>=7.0->uvicorn) (0.4.6)
    Requirement already satisfied: MarkupSafe>=2.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from jinja2>=2.11.3->llama-cpp-python) (3.0.3)
    Requirement already satisfied: annotated-types>=0.6.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from pydantic>=2.9.0->fastapi) (0.6.0)
    Requirement already satisfied: pydantic-core==2.41.5 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from pydantic>=2.9.0->fastapi) (2.41.5)
    Requirement already satisfied: anyio<5,>=3.6.2 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from starlette>=0.46.0->fastapi) (4.10.0)
    Requirement already satisfied: idna>=2.8 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from anyio<5,>=3.6.2->starlette>=0.46.0->fastapi) (3.11)
    Requirement already satisfied: sniffio>=1.1 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from anyio<5,>=3.6.2->starlette>=0.46.0->fastapi) (1.3.1)
    


```python
#Import libraries
from fastapi import FastAPI
from pydantic import BaseModel
from llama_cpp import Llama
import nest_asyncio
import uvicorn
```


```python
#Fix Jupyter event loop
nest_asyncio.apply()
```


```python
#Create FastAPI app
app = FastAPI()
```


```python
#Load model
llm = Llama(
    model_path=r"C:\Users\Brindha Sivakumar\kact\7 Days GenAI Challenge\day5\llama\models\model.gguf",
    n_ctx=512,
    verbose=False
)

print("Model loaded successfully")
```

    llama_context: n_ctx_seq (512) < n_ctx_train (2048) -- the full capacity of the model will not be utilized
    

    Model loaded successfully
    


```python
#Define schema
class PromptRequest(BaseModel):
    prompt: str
    max_tokens: int = 100
    temperature: float = 0.7
```


```python
#Create API endpoint
@app.post("/generate")
def generate_text(request: PromptRequest):
    response = llm(
        request.prompt,
        max_tokens=request.max_tokens,
        temperature=request.temperature
    )

    text = response["choices"][0]["text"].strip()
    return {"response": text}
```


```python
#Run FatAPI inside Jupyter
config = uvicorn.Config(app, host="127.0.0.1", port=8000, log_level="info")
server = uvicorn.Server(config)

await server.serve()
```

    INFO:     Started server process [21232]
    INFO:     Waiting for application startup.
    INFO:     Application startup complete.
    INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
    

    INFO:     127.0.0.1:58653 - "GET / HTTP/1.1" 404 Not Found
    INFO:     127.0.0.1:58653 - "GET / HTTP/1.1" 404 Not Found
    INFO:     127.0.0.1:58653 - "GET /docs HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58653 - "GET /openapi.json HTTP/1.1" 200 OK
    INFO:     127.0.0.1:53701 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:65003 - "GET /generate HTTP/1.1" 405 Method Not Allowed
    INFO:     127.0.0.1:58973 - "GET /generate HTTP/1.1" 405 Method Not Allowed
    INFO:     127.0.0.1:55419 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:55552 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:55552 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:55552 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:55552 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58917 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58917 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58917 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58917 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58917 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58917 - "POST /generate HTTP/1.1" 200 OK
    INFO:     127.0.0.1:58917 - "POST /generate HTTP/1.1" 200 OK
    


```python
#Open Swagger
#Open this in browser:
http://127.0.0.1:8000/docs
```


---
**Score: 5**