# Service-Titan-Internship-Round-2-Questions-And-Answers

Q1. Describe each component of the RAG system (embedder, PDF processor, vector DB, LLM response generation etc.). For each component, describe how you would choose which tool to use (e.g. for the embedding model I will choose to use openAI embeddings because of ….). You can also present certain tools and discuss why you wouldn’t use them.

A1. I will begin with describing the RAG system components one by one and discuss my conclusion based on observations. Before diving into just a quick remark that Retreival Augmented Generators consists of two main components Retreival (discussed in points 1, 2, 3) and Generator (discussed in point 4).
1) **RAG embedder** - Since the task is for generation of the chatbot, the embedders like GLoVe and FastText were out of my view becuase even though they are easy to use but the embedding itself has low contextual awareness in between the words. That is the reason I was analyzing between three well known embedders for me - OpenAI Embeddings, Sentence BERT and Google USE (Universal Sentence Encoder). Upon those, I chose the Google USE as it does not require API calls like OpenAI Embeddings and does not require large amount of computational resources like Setnece BERT, the negative part of Google USE is that it is less customizable in comparison to the Sentence Bert for example, but, for this task I consider Google USE to be the best model.
2) **RAG PDF processor** - This part was not much difficult becuase the python has many useful and interesting libraries for PDF extraction. I considered various variants even Tesseract which is more commonly used for Images rather than texts, but end up picking the most popular and widely used library for python named PyMuPDF. This library is faster in comparison to PDFMiner, shows better performance on long run (I mean when we have long PDFs) in comparison to PyPDF2. So my choice is PyMUPDF.
3) **RAG Vector DB** - Before this test I only knew about Annoy (Approximate Nearest Neighbors Oh Yeah) Vector Database, but I also know that this is not the best model for Chatbots becuase Annoy involves approximate search for efficiency and may lead to contextual issues, which is very crucial for the Chatbots. After proper research I found several models upon which I prefered Milvus. I was mainly focused on comparing Milvus and FAISS and came to conclusion that Milvus is shows better performance for chatbots as it is more lightweightd and faster compared to FAISS and deals better while generating a real time response. On the other hand FAISSS shows better results on long run but for this task I am not going to use it. My choice is Milvus.
4) **RAG Response Generator** - From my experience and based on researchs and papers which will be mentioned sooner I would use fine tunned version of BERT named [ALBERT](https://arxiv.org/pdf/1909.11942). In comparison to general [BERT](https://arxiv.org/pdf/1810.04805) model it has better overall scores and in comparison to BERTLarge has nearly 18 times less parameters and trains 1.7x times faster. My final choice is ALBERT and here is the picture of performance. ![Image](https://raw.githubusercontent.com/brightmart/albert_zh/master/resources/state_of_the_art.jpg)




Q2. Describe at least 2 challenges that you will encounter when using the tool(s) that you chose (e.g. when using openAI embeddings, the vector size is too long …), 
and try to think of how you can overcome those challenges.

A2. Below I will describe two challanges that I can face and my strategies for their mitigation.
1) **Challange of data extraction from PDFs** - This issue is related but not limited on PyMuPDF library, in fact all python libraries can have difficulties of extracting the information from manual PDFs because of the font, language, tables, etc. Considering the fact that other tools (especially Milvus) requires having a large dataset, the probability of facing such a challange significantly increases. To solve this issue I would likely use some OCR tools for preprocessing of the PDFs to reduce the chances of having wrong or corrupted outputs in the end.
2) **Challange of working with high dimensional data for Vector DBs** - This issue is related to Milvus Vector DB which is using a lot of data to generate high dimensional vectors which later needs to be used for the search. The challange is that the higher the dimension of the vectors the more time search operation in vector space will take. The solution of the challenge is to configure Milvus in the way to have the ```K``` dimension vectors where ```K``` is not big enough to cause computational issues and not small enough to be seperable.


Q3. Once you complete choosing the tools, present 5 examples of complex questions that the chatbot you designed will be able to answer, and 5 examples of questions that your chatbot will fail to answer. Present reasons why.

A3.
