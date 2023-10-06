
![RasaGPT Logo](https://github.com/paulpierre/RasaGPT/blob/main/github/rasagpt-logo-1.png?raw=true)

<br/><br/>

# 📊 RasaGPT Query Results

The following are questions based on the example dummy data included in the RasaGPT repository. The response framing is great, but retrieval quality is not sufficient due to indexing. Here are some examples of Q&A hits and misses w/ some commentary.

<br/><br/>

# ☑️  Hits



<br/>

### ✅ “Tell me about Pepe Corp”

```bash
Pepe Corp is a groundbreaking parody pharmaceutical and technology company inspired by the iconic Pepe the Frog meme. Our mission is to revolutionize the world through the power of dank memes.
🏷️  company,mission
```

<br/>

### ✅ “What is the weather like in London today?” (It’s expected to say I don’t know)

```bash
I apologize, I'm not sure how to help with that
🏷️  weather
```

<br/>

### ✅ “What is the purpose of Kekzal?”

```bash
Kekzal enhances sexual function.
🏷️  purpose
```

<br/>

### ✅ “What is Kekzal and what are the side effects?”

```bash
Kekzal is a medication, but the document does not provide information on its side effects. It only advises to consult a memologist if side effects persist or become bothersome.
🏷️  Kekzal,side effects
```

>- It probably should not mention “the document” and this can be optimized by modifying the prompt template

<br/>

### ✅ “What should I consider before taking Kekzal?”

```bash
Before taking Kekzal, consider the following precautions: Do not use Kekzal if you have a known allergy to dank memes or an aversion to Pepe-themed roman.
🏷️  precautions
```

>- This is technically a hit, but because of the chunking settings, the original text gets cut off and says “roman” instead of “romantic..” so this is easily fixed
>- Ideally it should include “Kekzal” in the tags

<br/>

### ✅ “What are the overdose symptoms of Kekzal?”

```bash
In case of an overdose, symptoms may include: 1. An insatiable desire to create
🏷️  overdose,symptoms
```

- This is a technical hit, but again indexing cut it off and it is likely due to the configured output limits which is easily fixed

<br/>

### ✅ “Who is the CEO?”

```bash
The CEO of Pepe Corp is Amanda Gigglestein.
🏷️  CEO
```

<br/>

### ✅ “What are the 2023 plans of Pepe Corp?”

```bash
Pepe Corp plans to invest in research and development to create cutting-edge products that address the needs of our diverse customer base. Our R&D team will explore new areas of meme science and technology, developing products that meet the needs of our customers.
🏷️  plans,research and development,cutting-edge products,meme science,technology,customer needs
```

<br/>

### ✅ “What is the revenue for Q1 FY2023?”

```bash
$235 million
🏷️  revenue,Q1,FY2023
```

<br/>

### ✅ “Tell me about the early days of PepeCorp?”

```bash
In the early 21st century, a group of visionary memelords gathered in a dimly lit basement, fueled by their passion for dank memes and the iconic Pepe the Frog.
🏷️  company history,early days
```

<br/><br/>

# ❌ Misses


### ❌ “What are the side effects of Kekzal?”

```bash
Consult your memologist if side effects persist or become bothersome.
🏷️  Kekzal,side effects
```

>- The side effects are in the documentation but it misses it entirely. It did tag the product name unlike earlier sessions, but index optimization can fix this

<br/>

### ❌ “Who is Dr. Olivia Dankstein?”

```bash
I apologize, I'm not sure how to help with that.
🏷️  unknown
```

>- Not good, she is in the Board of Directors document and is Vice-Chair and Chief Scientific Officer of Pepe Corp. Better indexing strategies fix this.

<br/>


### ❌ “What is the list of products provided by Pepe Corp?”

```bash
Pepe Corp has a diverse portfolio of products, ranging from meme-inspired pharmaceuticals to advanced brain implants and mobile applications.
🏷️  products
```

>- Although it did answer, it is insufficient. This is where a tree index with summarization would help

<br/><br/>

# 🔬 Retrieval optimizations

So how could all of this be improved by what I estimate to be more than 70%+? Here are a few solutions I intend to explore:


- Better index-tuning at the LLM library level, specifically using llama-index instead of Langchain which specializes in retrieval
- Usage of [Query Routers Abstractions](https://medium.com/@jerryjliu98/unifying-llm-powered-qa-techniques-with-routing-abstractions-438e2499a0d0) to understand which search strategy to use (one-shot vs few-shot)
- Explore other indexing methods like Tree indexes, Keyword indexes
- Adding chat history for immediate recall and context setting
- Add a secondary adversarial agent ([Dual pattern model](https://simonwillison.net/2023/Apr/25/dual-llm-pattern/)) with the following potential functionalities:
    - Determine if the question has been answered and if not, re-optimize search strategy
    - Ensure prompt injection is not occurring
- Increase baseline similarity search by exploring:
    - Regularly generate “fake” document embeddings based on historical queries and link to actual documents via [HyDE pattern](https://wfhbrian.com/revolutionizing-search-how-hypothetical-document-embeddings-hyde-can-save-time-and-increase-productivity/)
    - Regularly generate “fake” user queries based on documents and link to actual document so user input search and “fake” queries can match better