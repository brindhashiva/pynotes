---
title: Llama-Mongodb-Ai-System
date: 2026-04-17
author: Your Name
cell_count: 26
score: 25
---

```python
!pip install pymongo llama-cpp-python
```

    Requirement already satisfied: pymongo in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (4.16.0)
    Requirement already satisfied: llama-cpp-python in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (0.3.19)
    Requirement already satisfied: dnspython<3.0.0,>=2.6.1 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from pymongo) (2.8.0)
    Requirement already satisfied: typing-extensions>=4.5.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from llama-cpp-python) (4.15.0)
    Requirement already satisfied: numpy>=1.20.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from llama-cpp-python) (2.4.4)
    Requirement already satisfied: diskcache>=5.6.1 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from llama-cpp-python) (5.6.3)
    Requirement already satisfied: jinja2>=2.11.3 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from llama-cpp-python) (3.1.6)
    Requirement already satisfied: MarkupSafe>=2.0 in C:\Users\Brindha Sivakumar\miniconda3\Lib\site-packages (from jinja2>=2.11.3->llama-cpp-python) (3.0.3)
    


```python
#MongoDB Connection
from pymongo import MongoClient
from datetime import datetime

client = MongoClient("mongodb://localhost:27017/")
db = client["memoryvault_ai"]
collection = db["entries"]

print("Connected to MongoDB")
```

    Connected to MongoDB
    


```python
#Insert Data
collection.insert_one({
    "name": "Brindha",
    "task": "Day 3 Pipeline from Python",
    "status": "Success"
})

print("Data inserted successfully")
```

    Data inserted successfully
    


```python
#Read Data
for doc in collection.find():
    print(doc)
```

    {'_id': ObjectId('69e0d0fd97f365e15cf318bf'), 'name': 'Brindha', 'project': 'MemoryVault AI', 'status': 'In Progress'}
    {'_id': ObjectId('69e0d15597f365e15cf318c0'), 'title': 'Learning Log', 'content': 'Worked on MongoDB + AI pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 38, 53, 714000)}
    {'_id': ObjectId('69e0d34a97f365e15cf318c3'), 'title': 'Learning Log', 'content': 'Worked on AI + MongoDB pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 47, 14, 429000)}
    {'_id': ObjectId('69e11a8a4b0b1db8f40f16c9'), 'name': 'Brindha', 'task': 'Day 3 Pipeline from Python', 'status': 'Success'}
    


```python
#Update Data
collection.update_one(
    {"name": "Brindha"},
    {"$set": {"status": "In Progress"}}
)

print("Data updated successfully")
```

    Data updated successfully
    


```python
#Read Updated Data
for doc in collection.find():
    print(doc)
```

    {'_id': ObjectId('69e0d0fd97f365e15cf318bf'), 'name': 'Brindha', 'project': 'MemoryVault AI', 'status': 'In Progress'}
    {'_id': ObjectId('69e0d15597f365e15cf318c0'), 'title': 'Learning Log', 'content': 'Worked on MongoDB + AI pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 38, 53, 714000)}
    {'_id': ObjectId('69e0d34a97f365e15cf318c3'), 'title': 'Learning Log', 'content': 'Worked on AI + MongoDB pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 47, 14, 429000)}
    {'_id': ObjectId('69e11a8a4b0b1db8f40f16c9'), 'name': 'Brindha', 'task': 'Day 3 Pipeline from Python', 'status': 'Success'}
    


```python
#Delete Data
collection.delete_one({"status": "In Progress"})

print("Data deleted successfully")
```

    Data deleted successfully
    


```python
#Final Check After Delete
for doc in collection.find():
    print(doc)
```

    {'_id': ObjectId('69e0d15597f365e15cf318c0'), 'title': 'Learning Log', 'content': 'Worked on MongoDB + AI pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 38, 53, 714000)}
    {'_id': ObjectId('69e0d34a97f365e15cf318c3'), 'title': 'Learning Log', 'content': 'Worked on AI + MongoDB pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 47, 14, 429000)}
    {'_id': ObjectId('69e11a8a4b0b1db8f40f16c9'), 'name': 'Brindha', 'task': 'Day 3 Pipeline from Python', 'status': 'Success'}
    


```python
#Insert with Date
collection.insert_one({
    "title": "Learning Log",
    "content": "Worked on MongoDB and AI pipeline",
    "created_at": datetime.now()
})

print("Inserted with date")
```

    Inserted with date
    


```python
#Date Query
results = collection.find({
    "created_at": {
        "$gte": datetime(2026, 4, 1),
        "$lte": datetime(2026, 4, 30)
    }
})

for doc in results:
    print(doc)
```

    {'_id': ObjectId('69e0d15597f365e15cf318c0'), 'title': 'Learning Log', 'content': 'Worked on MongoDB + AI pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 38, 53, 714000)}
    {'_id': ObjectId('69e0d34a97f365e15cf318c3'), 'title': 'Learning Log', 'content': 'Worked on AI + MongoDB pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 47, 14, 429000)}
    {'_id': ObjectId('69e11c4f4b0b1db8f40f16ca'), 'title': 'Learning Log', 'content': 'Worked on MongoDB and AI pipeline', 'created_at': datetime.datetime(2026, 4, 16, 22, 58, 47, 448000)}
    


```python
#Field Operators
#$set
collection.update_one(
    {"title": "Learning Log"},
    {"$set": {"content": "Updated MongoDB and AI pipeline notes"}}
)

print("Used $set")
```

    Used $set
    


```python
#$inc
collection.update_one(
    {"title": "Learning Log"},
    {"$inc": {"views": 1}}
)

print("Used $inc")
```

    Used $inc
    


```python
#$push
collection.update_one(
    {"title": "Learning Log"},
    {"$push": {"tags": "mongodb"}}
)

print("Used $push")
```

    Used $push
    


```python
#$unset
collection.update_one(
    {"title": "Learning Log"},
    {"$unset": {"views": ""}}
)

print("Used $unset")
```

    Used $unset
    


```python
#Check final document
for doc in collection.find():
    print(doc)
```

    {'_id': ObjectId('69e0d15597f365e15cf318c0'), 'title': 'Learning Log', 'content': 'Updated MongoDB and AI pipeline notes', 'created_at': datetime.datetime(2026, 4, 16, 17, 38, 53, 714000), 'tags': ['mongodb']}
    {'_id': ObjectId('69e0d34a97f365e15cf318c3'), 'title': 'Learning Log', 'content': 'Worked on AI + MongoDB pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 47, 14, 429000)}
    {'_id': ObjectId('69e11a8a4b0b1db8f40f16c9'), 'name': 'Brindha', 'task': 'Day 3 Pipeline from Python', 'status': 'Success'}
    {'_id': ObjectId('69e11c4f4b0b1db8f40f16ca'), 'title': 'Learning Log', 'content': 'Worked on MongoDB and AI pipeline', 'created_at': datetime.datetime(2026, 4, 16, 22, 58, 47, 448000)}
    


```python
#Load GGUF Model
model_path = r"C:\Users\Brindha Sivakumar\Downloads\tinyllama-1.1b-chat-v1.0-q4_k_m.gguf"
```


```python
#Load model
from llama_cpp import Llama

model_path = r"C:\Users\Brindha Sivakumar\Downloads\tinyllama-1.1b-chat-v1.0-q4_k_m.gguf"

llm = Llama(
    model_path=model_path,
    n_ctx=2048,
    verbose=False
)

print("Model loaded successfully")
```

    Model loaded successfully
    


```python
#Basic prompt
output = llm.create_chat_completion(
    messages=[
        {"role": "user", "content": "Explain AI in simple words"}
    ],
    max_tokens=80
)

print(output["choices"][0]["message"]["content"])
```

    AI (Artificial Intelligence) is a field of computer science that involves the development of machines that can think and behave like humans. It involves the use of algorithms, machine learning, and other technologies to create intelligent systems that can perform tasks that are typically performed by humans, such as language translation, image recognition, and chatbots.
    
    AI can be divided into two main
    


```python
#Temperature, top-p, max tokens
output = llm.create_chat_completion(
    messages=[
        {"role": "user", "content": "Explain vector database"}
    ],
    max_tokens=80,
    temperature=0.7,
    top_p=0.9
)

print(output["choices"][0]["message"]["content"])
```

    A vector database is a type of database that stores and manages data in a format that is based on vectors, which are abstract mathematical objects that represent a point in space. These data points are known as vectors, and the database is designed to store and manipulate these vectors in a specific way.
    
    In a vector database, the data is stored in a matrix or grid format, where each row
    


```python
#Connect MongoDB
from pymongo import MongoClient
from datetime import datetime

client = MongoClient("mongodb://localhost:27017/")
db = client["memoryvault_ai"]
collection = db["entries"]

print("MongoDB Connected")
```

    MongoDB Connected
    


```python
#AI → MongoDB
prompt = "What is stateful AI?"

output = llm.create_chat_completion(
    messages=[
        {"role": "user", "content": prompt}
    ],
    max_tokens=100,
    temperature=0.3
)

response = output["choices"][0]["message"]["content"]

collection.insert_one({
    "prompt": prompt,
    "response": response,
    "created_at": datetime.now()
})

print("Saved to MongoDB")
print(response)
```

    Saved to MongoDB
    Stateful AI is a type of machine learning that involves the use of physical hardware and software to store and process data. It is different from stateless AI, which is a form of machine learning that does not involve the use of physical hardware or software.
    
    In stateful AI, data is stored in a physical device, such as a server or a storage device, and processed by the software running on that device. This allows for greater scalability and flexibility, as the
    


```python
#Retrieve stored data
last_doc = collection.find_one(sort=[("created_at", -1)])

print(last_doc)
```

    {'_id': ObjectId('69e121b24b0b1db8f40f16cc'), 'prompt': 'What is stateful AI?', 'response': 'Stateful AI is a type of machine learning that involves the use of physical hardware and software to store and process data. It is different from stateless AI, which is a form of machine learning that does not involve the use of physical hardware or software.\n\nIn stateful AI, data is stored in a physical device, such as a server or a storage device, and processed by the software running on that device. This allows for greater scalability and flexibility, as the', 'created_at': datetime.datetime(2026, 4, 16, 23, 21, 46, 139000)}
    


```python
#Reuse memory
context = last_doc["response"]

new_prompt = f"Based on this: {context} explain in one short sentence"

output = llm.create_chat_completion(
    messages=[
        {"role": "user", "content": new_prompt}
    ],
    max_tokens=60
)

print(output["choices"][0]["message"]["content"])
```

    In stateful AI, data is stored in a physical device, such as a server or a storage device, and processed by the software running on that device. This allows for greater scalability and flexibility, as the explain in one short sentence.
    


```python
#Store multiple entries
prompts = [
    "What is AI?",
    "What is Machine Learning?",
    "What is Deep Learning?"
]

for p in prompts:
    output = llm.create_chat_completion(
        messages=[{"role": "user", "content": p}],
        max_tokens=80
    )

    collection.insert_one({
        "prompt": p,
        "response": output["choices"][0]["message"]["content"],
        "created_at": datetime.now()
    })

print("Multiple entries stored")
```

    Multiple entries stored
    


```python
#Query by date
from datetime import datetime

results = collection.find({
    "created_at": {
        "$gte": datetime(2026, 4, 1),
        "$lte": datetime(2026, 4, 30)
    }
})

for doc in results:
    print(doc)
```

    {'_id': ObjectId('69e0d15597f365e15cf318c0'), 'title': 'Learning Log', 'content': 'Updated MongoDB and AI pipeline notes', 'created_at': datetime.datetime(2026, 4, 16, 17, 38, 53, 714000), 'tags': ['mongodb']}
    {'_id': ObjectId('69e0d34a97f365e15cf318c3'), 'title': 'Learning Log', 'content': 'Worked on AI + MongoDB pipeline', 'created_at': datetime.datetime(2026, 4, 16, 17, 47, 14, 429000)}
    {'_id': ObjectId('69e11c4f4b0b1db8f40f16ca'), 'title': 'Learning Log', 'content': 'Worked on MongoDB and AI pipeline', 'created_at': datetime.datetime(2026, 4, 16, 22, 58, 47, 448000)}
    {'_id': ObjectId('69e121b24b0b1db8f40f16cc'), 'prompt': 'What is stateful AI?', 'response': 'Stateful AI is a type of machine learning that involves the use of physical hardware and software to store and process data. It is different from stateless AI, which is a form of machine learning that does not involve the use of physical hardware or software.\n\nIn stateful AI, data is stored in a physical device, such as a server or a storage device, and processed by the software running on that device. This allows for greater scalability and flexibility, as the', 'created_at': datetime.datetime(2026, 4, 16, 23, 21, 46, 139000)}
    {'_id': ObjectId('69e122094b0b1db8f40f16cd'), 'prompt': 'What is AI?', 'response': 'Artificial Intelligence (AI) is a field of computer science that involves the development of computer systems that can exhibit intelligent behavior, such as problem-solving, decision-making, and learning. AI is a rapidly growing field that has revolutionized many aspects of modern life, including healthcare, finance, and transportation.\n\nAI can be divided into two main', 'created_at': datetime.datetime(2026, 4, 16, 23, 23, 13, 594000)}
    {'_id': ObjectId('69e1220d4b0b1db8f40f16ce'), 'prompt': 'What is Machine Learning?', 'response': 'Machine Learning (ML) is a field of artificial intelligence that involves the development of algorithms and models that can learn from data without being explicitly programmed. ML is used in various applications, including:\n\n1. Predictive Analytics: ML algorithms are used to analyze large datasets to identify patterns and trends that can be used to make predictions.\n\n2. Natural Language Processing (', 'created_at': datetime.datetime(2026, 4, 16, 23, 23, 17, 402000)}
    {'_id': ObjectId('69e122104b0b1db8f40f16cf'), 'prompt': 'What is Deep Learning?', 'response': 'Deep Learning is a type of machine learning that involves the use of deep neural networks, which are composed of multiple layers of neurons that can learn complex patterns and relationships from data. The term "deep" refers to the number of layers in the neural network, while "learning" refers to the process of training the network to make predictions based on the data it has been exposed to.\n\n', 'created_at': datetime.datetime(2026, 4, 16, 23, 23, 20, 733000)}
    


```python
#Force JSON output
prompt = """
Give output only in JSON format:

{
  "top_text": "...",
  "bottom_text": "..."
}

Explain vector database
"""

output = llm.create_chat_completion(
    messages=[
        {"role": "user", "content": prompt}
    ],
    max_tokens=120,
    temperature=0.2
)

print(output["choices"][0]["message"]["content"])
```

    A vector database is a type of database that stores and manages vector data, which is a type of data that is represented by a set of points or coordinates. The vector data can be represented as a set of points, lines, polygons, or other geometric shapes.
    
    A vector database is typically used in applications that require the storage and manipulation of large amounts of vector data, such as mapping, geographic information systems (GIS), and 3D modeling. Vector databases are often used in combination with other types of databases, such as relational databases, to provide
    


---
**Score: 25**