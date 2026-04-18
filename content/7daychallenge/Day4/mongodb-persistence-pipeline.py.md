---
title: Mongodb-Persistence-Pipeline.Py
date: 2026-04-19
author: Your Name
cell_count: 12
score: 10
---

```python
!pip install pymongo python-dotenv
```

    Requirement already satisfied: pymongo in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (4.16.0)
    Requirement already satisfied: python-dotenv in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (1.2.1)
    Requirement already satisfied: dnspython<3.0.0,>=2.6.1 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from pymongo) (2.8.0)
    


```python
#Set Mongo URI
import os

os.environ["MONGO_URI"] = "mongodb://localhost:27017/"

mongo_uri = os.getenv("MONGO_URI")

print("Mongo URI Loaded:", mongo_uri is not None)
print("Mongo URI:", mongo_uri)
```

    Mongo URI Loaded: True
    Mongo URI: mongodb://localhost:27017/
    


```python
#Connect MongoDB + Ping
from pymongo import MongoClient

client = MongoClient(mongo_uri)

client.admin.command("ping")

db = client["day4_persistence"]
collection = db["entries"]

print("Connected to MongoDB")
```

    Connected to MongoDB
    


```python
#insert_one
from datetime import datetime

result_one = collection.insert_one({
    "name": "Brindha",
    "task": "MongoDB Connection",
    "status": "Success",
    "created_at": datetime.now()
})

print("Inserted One ID:", result_one.inserted_id)
```

    Inserted One ID: 69e138c7b42b899c98557622
    


```python
#insert_many
docs = [
    {"topic": "MongoDB Atlas", "level": "Beginner"},
    {"topic": "PyMongo", "level": "Intermediate"},
    {"topic": "Persistent Storage", "level": "Important"}
]

result_many = collection.insert_many(docs)

print("Inserted Many IDs:", result_many.inserted_ids)
```

    Inserted Many IDs: [ObjectId('69e138f4b42b899c98557623'), ObjectId('69e138f4b42b899c98557624'), ObjectId('69e138f4b42b899c98557625')]
    


```python
#find_one
single_doc = collection.find_one({"name": "Brindha"})
print(single_doc)
```

    {'_id': ObjectId('69e138c7b42b899c98557622'), 'name': 'Brindha', 'task': 'MongoDB Connection', 'status': 'Success', 'created_at': datetime.datetime(2026, 4, 17, 1, 0, 15, 316000)}
    


```python
#find with filter
results = collection.find({"level": "Beginner"})

for doc in results:
    print(doc)
```

    {'_id': ObjectId('69e138f4b42b899c98557623'), 'topic': 'MongoDB Atlas', 'level': 'Beginner'}
    


```python
#projection
results = collection.find(
    {"level": "Beginner"},
    {"_id": 0, "topic": 1}
)

for doc in results:
    print(doc)
```

    {'topic': 'MongoDB Atlas'}
    


```python
#Load AI model
from llama_cpp import Llama

model_path = r"C:\Users\Brindha Sivakumar\Downloads\tinyllama-1.1b-chat-v1.0-q4_k_m.gguf"

llm = Llama(
    model_path=model_path,
    n_ctx=2048,
    verbose=False
)

print("Model Loaded")
```

    Model Loaded
    


```python
#Generate AI output
prompt = "Explain persistent storage in simple words"

output = llm.create_chat_completion(
    messages=[{"role": "user", "content": prompt}],
    max_tokens=100
)

response = output["choices"][0]["message"]["content"]

print("AI Response:")
print(response)
```

    AI Response:
    Persistent storage is a type of storage that can be accessed even after the computer or device is turned off or powered off. It is a type of storage that stores data even when the computer or device is not running. Persistent storage is typically used for storing data that is essential for the user's work or personal use, such as documents, photos, music, and videos. Persistent storage is different from temporary storage, which is used for temporary data that is deleted when the computer or
    


```python
#Save AI output
collection.insert_one({
    "prompt": prompt,
    "response": response,
    "source": "llama",
    "created_at": datetime.now()
})

print("AI output saved")
```

    AI output saved
    


```python
#Retrieve AI output
doc = collection.find_one({"source": "llama"})
print(doc)
```

    {'_id': ObjectId('69e13a7cb42b899c98557626'), 'prompt': 'Explain persistent storage in simple words', 'response': "Persistent storage is a type of storage that can be accessed even after the computer or device is turned off or powered off. It is a type of storage that stores data even when the computer or device is not running. Persistent storage is typically used for storing data that is essential for the user's work or personal use, such as documents, photos, music, and videos. Persistent storage is different from temporary storage, which is used for temporary data that is deleted when the computer or", 'source': 'llama', 'created_at': datetime.datetime(2026, 4, 17, 1, 7, 32, 872000)}
    


---
**Score: 10**