# Project 1 Planning: The Unofficial Guide

> Write this document before you write any pipeline code.
> Your spec and architecture diagram are what you'll use to direct AI tools (Claude, Copilot, etc.) to generate your implementation — the more specific they are, the more useful the generated code will be.
> Update the Retrieval Approach and Chunking Strategy sections if you change your approach during implementation.
> Update this file before starting any stretch features.

---

## Domain

<!-- What domain did you choose? Why is this knowledge valuable and hard to find through official channels? -->
UIUC off-campus housing advice
My project focuses on UIUC off-campus housing advice, combining student discussions, apartment reviews, and housing resources to help students make informed housing decisions. This knowledge is difficult to find because much of it is scattered across Reddit threads, review websites, and university resources rather than being available in a single searchable location. By using a RAG system, students can ask natural-language questions about apartments, landlords, leasing companies, and housing concerns and receive answers grounded in real student experiences and housing information.
---

## Documents

<!-- List your specific sources: URLs, subreddit names, forum threads, or file descriptions.
     Aim for at least 10 sources that together cover different subtopics or perspectives within your domain. -->

| # | Source | Description | URL or location |
|---|--------|-------------|-----------------|
| 1 | Reddit: Good Off-Campus Housing in Champaign/Urbana Area| UIUC students discuss recommended apartment complexes, landlords, and neighborhoods for off-campus housing in Champaign-Urbana.| https://www.reddit.com/r/UIUC/comments/13o1qv0/good_offcampus_housing_in_champaignurbana_area/|
| 2 | Reddit: Good Rental Companies/Apartments?|Students discuss reputable leasing companies and apartment options near campus. |https://www.reddit.com/r/UIUC/comments/1bj4lpy/good_rental_companiesapartments/ |
| 3 | Reddit: Leasing Company Tier List|Students rank leasing companies based on maintenance, communication, and overall experience. |https://www.reddit.com/r/UIUC/comments/16mb5lj/leasing_company_tier_list/ |
| 4 |Reddit: Best/Worst Off-Campus Apartments |Discussion of apartment complexes that students recommend or advise avoiding. |https://www.reddit.com/r/UIUC/comments/1n1gouf/bestworst_off_campus_apts/ |
| 5 | Reddit: How Bad is University Group?| Student experiences with University Group's management and maintenance practices.|https://www.reddit.com/r/UIUC/comments/17iobyv/how_bad_is_university_group/ |
| 6 | Reddit: Private Landlords?|Students compare renting from private landlords versus large leasing companies. | Students compare renting from private landlords versus large leasing companies.|
| 7 |JSM Living Reviews |Resident reviews discussing apartment quality, maintenance, and leasing experiences with JSM properties. | https://jsmliving.com|
| 8 |Smile Student Living Reviews |Student feedback about amenities, management responsiveness, and living conditions.| https://www.smilestudentliving.com|
| 9 | UIUC Off-Campus Community Living|Official guidance on leasing, tenant rights, and off-campus housing resources for students.| https://occl.illinois.edu|
| 10 | Tenant Union at UIUC Resources| Information on tenant rights, lease agreements, and common housing issues faced by students.| https://occl.illinois.edu/resources/tenant-union|

---

## Chunking Strategy

<!-- How will you split documents into chunks?
     State your chunk size (in tokens or characters), overlap size, and explain why those
     numbers fit the structure of your documents.
     A review-heavy corpus warrants different chunking than a long FAQ. -->

**Chunk size:**
600–800 characters
**Overlap:**
100-character overlap
**Reasoning:**
This size fits my UIUC off-campus housing corpus because many sources are Reddit threads, apartment reviews, and short housing advice pages where useful information is usually contained in short comments or paragraphs. The overlap helps preserve context when a comment mentions an apartment name in one sentence and gives the actual opinion or reasoning in the next sentence.
---

## Retrieval Approach

<!-- Which embedding model are you using (e.g., all-MiniLM-L6-v2 via sentence-transformers)?
     How many chunks will you retrieve per query (top-k)?
     If you were deploying this for real users and cost wasn't a constraint, what tradeoffs
     would you weigh in choosing a different embedding model — context length, multilingual
     support, accuracy on domain-specific text, latency? -->
I will use the all-MiniLM-L6-v2 embedding model through sentence-transformers because it is lightweight, easy to run locally, and works well for general semantic search. For each user query, I plan to retrieve the top 4 most relevant chunks so the LLM has enough context without being overwhelmed by unrelated information. If this system were deployed for real users and cost was not a constraint, I would compare models based on retrieval accuracy for student housing language, context length, latency, and whether the model handles informal text, abbreviations, and multilingual comments well.

**Embedding model:**
all-MiniLM-L6-v2 using sentence-transformers
**Top-k:**
Top 4 chunks per query
**Production tradeoff reflection:**
If this system were used by real students, I would compare embedding models based on retrieval accuracy, speed, context length, and ability to understand informal student language from Reddit posts and reviews. I would also consider whether the model handles abbreviations, housing-specific terms, and multilingual comments well.
---

## Evaluation Plan

<!-- List your 5 test questions with their expected correct answers.
     Questions should be specific enough that you can judge whether the system's response
     is right or wrong. "What are good dining halls?" is too vague.
     "What do students say about wait times at [dining hall name] during lunch?" is testable. -->

| # | Question | Expected answer |
|---|----------|-----------------|
| 1 | Which leasing companies do UIUC students recommend most often?|Students generally recommend companies such as JSM and other landlords with responsive maintenance and good communication. |
| 2 |What complaints do students have about University Group? | Common complaints include slow maintenance responses, communication issues, and dissatisfaction with property management.|
| 3 |What are the advantages of renting from a private landlord instead of a leasing company? | Students report more personalized communication, greater flexibility, and faster responses from some private landlords.|
| 4 | What factors do students consider when choosing an off-campus apartment?| Students frequently mention rent price, location, maintenance quality, safety, amenities, and landlord reputation.|
| 5 |What advice do students give before signing an off-campus lease? | Students recommend researching landlords, reading reviews, understanding lease terms, and touring the property before signing.|

---

## Anticipated Challenges

<!-- What could go wrong? Name at least two specific risks with reasoning.
     Consider: noisy or inconsistent documents, missing source attribution, off-topic
     retrieval, chunks that split key information across boundaries. -->

1.Student-generated sources such as Reddit posts and apartment reviews may contain biased or conflicting opinions, making it difficult to determine which information is most reliable.

2.The retrieval system may return irrelevant chunks or miss important context if key information is split across multiple chunks, leading to incomplete or inaccurate answers.

---

## Architecture

<!-- Draw a diagram of your pipeline showing the five stages:
     Document Ingestion → Chunking → Embedding + Vector Store → Retrieval → Generation
     Label each stage with the tool or library you're using.
     You can use ASCII art, a Mermaid diagram, or embed a sketch as an image.
     You'll use this diagram as context when prompting AI tools to implement each stage. -->
     Document Ingestion
(Python loads Reddit posts, reviews, and housing resources)
        ↓
Chunking
(600–800 character chunks with 100 character overlap)
        ↓
Embedding + Vector Store
(all-MiniLM-L6-v2 embeddings stored in ChromaDB)
        ↓
Retrieval
(Top 4 chunks retrieved using semantic similarity search)
        ↓
Generation
(Groq LLM generates grounded answer with source citations)

---

## AI Tool Plan

<!-- For each part of the pipeline below, describe:
     - Which AI tool you plan to use (Claude, Copilot, ChatGPT, etc.)
     - What you'll give it as input (which sections of this planning.md, which requirements)
     - What you expect it to produce
     - How you'll verify the output matches your spec

     "I'll use AI to help me code" is not a plan.
     "I'll give Claude my Chunking Strategy section and ask it to implement chunk_text()
     with my specified chunk size and overlap" is a plan. -->

**Milestone 3 — Ingestion and chunking:**
Claude and ChatGPT will assist with building the document ingestion and chunking pipeline. I will provide my source list and chunking strategy so the generated code follows the specified chunk size and overlap. To verify the implementation, I will check that all documents are successfully loaded, chunked correctly, and retain their source information.
**Milestone 4 — Embedding and retrieval:**
The embedding and retrieval stage will use all-MiniLM-L6-v2 and ChromaDB for semantic search. Claude and ChatGPT will be used to generate and troubleshoot code for creating embeddings, storing them in the vector database, and retrieving the most relevant chunks. Success will be measured by whether the retrieved results are relevant to my housing-related evaluation questions.
**Milestone 5 — Generation and interface:**
Once retrieval is working, Claude and ChatGPT will help connect the retrieved context to a Groq-powered LLM and create a simple user interface. I will provide the project requirements to ensure responses remain grounded in the retrieved documents and include source citations. Testing will focus on whether answers are accurate, supported by sources, and avoid information that was not retrieved.