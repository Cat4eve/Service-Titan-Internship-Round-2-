# Service-Titan-Internship-Round-2-Questions-And-Answers

Q1. Describe each component of the RAG system (embedder, PDF processor, vector DB, LLM response generation etc.). For each component, describe how you would choose which tool to use (e.g. for the embedding model I will choose to use openAI embeddings because of ….). You can also present certain tools and discuss why you wouldn’t use them.

A1. I will begin with describing the RAG system components one by one and discuss my conclusion based on observations. Before diving in the topic, I wanted quickly mention that Retreival Augmented Generators consists of two main components Retreival (discussed in points 1, 2, 3) and Generation (discussed in point 4).
1) **RAG embedder** - Since the task is for generation of the chatbot, the embedders like [GLoVe](https://nlp.stanford.edu/pubs/glove.pdf) and [FastText](https://arxiv.org/pdf/1607.04606) were out of my view becuase even though they are easy to use but the embedding itself has low contextual awareness in between the words. That is the reason I was analyzing between three well known embedders for me - [OpenAI Embeddings](https://cdn.openai.com/papers/Text_and_Code_Embeddings_by_Contrastive_Pre_Training.pdf), [Sentence BERT](https://arxiv.org/pdf/1908.10084) and [Google USE](https://arxiv.org/pdf/1803.11175) (Universal Sentence Encoder). Upon those, I chose the Google USE as it does not require API calls like OpenAI Embeddings and does not require large amount of computational resources like Setnece BERT, the negative part of Google USE is that it is less customizable in comparison to the Sentence Bert for example, but, for this task I consider Google USE to be the best model.
2) **RAG PDF processor** - This part was not much difficult becuase the python has many useful and interesting libraries for PDF extraction. I considered various variants even Tesseract which is more commonly used for Images rather than texts, but end up picking the most popular and widely used library for python named PyMuPDF. This library is faster in comparison to PDFMiner, shows better performance on long run (I mean when we have long PDFs) in comparison to PyPDF2. So my choice is PyMUPDF.
3) **RAG Vector DB** - Before this internship test I only knew about [Annoy](https://github.com/spotify/annoy) (Approximate Nearest Neighbors Oh Yeah) Vector Database, but I also know that this is not the best model for Chatbots becuase Annoy involves approximate search for efficiency and may lead to contextual issues, which is very crucial for the Chatbots. After proper research I found several models. I was mainly focused on comparing [Milvus](https://www.cs.purdue.edu/homes/csjgwang/pubs/SIGMOD21_Milvus.pdf) and [FAISS](https://arxiv.org/pdf/1702.08734) and came to conclusion that Milvus shows better performance for chatbots as it is more lightweightd and faster compared to FAISS, hence, Milvus deals better while generating a real time response. On the other hand FAISS shows better results on long run, but for this task I am not going to use it. My choice is Milvus.
4) **RAG Response Generator** - From my experience and based on researchs and papers which will be mentioned sooner I would use fine tunned version of BERT named [ALBERT](https://arxiv.org/pdf/1909.11942). In comparison to general [BERT](https://arxiv.org/pdf/1810.04805) model it has better overall scores and in comparison to BERTLarge has nearly 18 times less parameters and trains 1.7x times faster. My final choice is ALBERT and here is the picture of performance. ![Image](https://raw.githubusercontent.com/brightmart/albert_zh/master/resources/state_of_the_art.jpg)




Q2. Describe at least 2 challenges that you will encounter when using the tool(s) that you chose (e.g. when using openAI embeddings, the vector size is too long …), 
and try to think of how you can overcome those challenges.

A2. Below I will describe two challanges that I can face and my strategies for their mitigation.
1) **Challange of data extraction from PDFs** - This issue is related but not limited on PyMuPDF library, in fact all python libraries can have difficulties of extracting the information from manual PDFs because of the font, language, tables, etc. Considering the fact that other tools (especially Milvus) requires having a large dataset, the probability of facing such a challange significantly increases. To solve this issue I would likely use some OCR tools for preprocessing of the PDFs to reduce the chances of having wrong or corrupted outputs in the end.
2) **Challange of working with high dimensional data for Vector DBs** - This issue is related to Milvus Vector DB which is using a lot of data to generate high dimensional vectors which later needs to be used for the search. The challange is that the higher the dimension of the vectors the more time search operation in vector space will take, which is crucial for chatbot that needs to generate a real time response. The solution of the challenge is to configure Milvus in the way to have the ```K``` dimension vectors where ```K``` is not big enough to cause computational issues and not small enough to be easily seperable from other vectors.


Q3. Once you complete choosing the tools, present 5 examples of complex questions that the chatbot you designed will be able to answer, and 5 examples of questions that your chatbot will fail to answer. Present reasons why.
P.S. In fact I adjusted my questions from the example of manual attached by you.

A3. Below I will present five questions that the model could fail to answer as well as five questions which can produce wrong answers by the model.

Let's begin with correct answers.
1) **How can I attach an indoor unit on Conditioner of Model X?** - This information can be directly extraced from manual directly. 
2) **Can I install the air conditioner on the solid base which cannot suport weight of unit for Model Y?** - Once model learns the meaning of negation in the sentences, the answer for this question would not be as difficult as we think of.
3) **How to solve error 255 for air conditioner model Z?** - This require a little bit of analyzis of two major parts. First, is the unerstanding that there is an error with number 255 and model needs to find how to deal with it. Second understanding that the person who asks the question needs help regarind this error so that model needs to outline the part where a person sees the issue and provide rellevant support. However, this is not a very difficult task, in fact this is a little more complex than question number one.
4) **What are safety precautions while working with Kitchen Stove of model W?** - This information can either be directly available from manual or be contextually understood. The reason is that safety precaution is a quite popular section for some products that may cause the harm (at least there is a law requiring to write this kind of information in manuals). So with large and wide amount of manuals, I consider that the model would understand what is safety precatuion and be able to apply it for response generation.
5) **What is the maximum allowed temperature of air conditioner of Model V and what are some warnings regarding it?** - This question requires of splitting it into two questions (which transformer based models are capable of) and then process each question separately and add this information while considering the other one. I mean that the question about maximum temperature can be answered, while second question separately have many warning points, but considering previous question it needs to filter only those which relates to maximum temperature.

Now the questions what will be difficult to be responded.
1) **Can you compare and mention key differences between Air conditioner of Model X and Y?** - Since the installation manuals cover the installation process and usually not compare different models (especially from other companies), hence the model most probably won't learn to distinguish and highlight key differences of models X and Y.
2) **What is ten to the power of ten minus twenty five to the power of eight?** - Well, even being a technical documents, installation guides have very few to do with actual mathematics and I took an extreem example that cannot be answered being learnt from installation manuals.
3) **What to deal with errors on my air conditioner of model Z?** - On one hand, from question three of positive response section we can see that the model is doing well with identification of sertain error mentioned in the document, on the other hand, the errors and their handling is not a big section so there is not really much data so that model can understand and generalize the existince of errors in general and how to generally approach to them.
4) **How much money would I save buying the air conditioner of model W?** - Like math, manuals does not contain information about pricing, hence, the model has no understanding of what is money, what is saving money and what is buying the air conditioner.
5) **Does the noise of vaccum cleaner of model V meets to industry standerds?** - Most probably model won't be able to understand the meaning of industry standards and fail asserting the vaccum cleaner noise with other models and performing some calculations.
