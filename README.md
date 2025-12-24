# AI Resume Assistant using RAG (Retrieval-Augmented Generation)

## What is This Project?

This is an **AI-powered resume assistant** that can answer questions about your resume intelligently. Instead of manually searching through your resume, you can simply ask questions like "What are my technical skills?" or "Does this candidate have backend experience?" and get accurate answers instantly.

## How Does RAG Work? (Explained Simply)

Think of RAG like a **smart library system**:

1. **Your Resume = The Book**: Your PDF resume is the source of truth
2. **The Librarian = Retriever**: Finds the most relevant sections when you ask a question
3. **The Brain = AI Model**: Reads those sections and generates a human-like answer
4. **The Translator = Embeddings**: Converts text into numbers so computers can understand similarity

### The RAG Pipeline (Step-by-Step)

```
Your Question 
    ↓
Convert to Numbers (Embeddings)
    ↓
Search Library (Vector Store)
    ↓
Find Relevant Resume Sections (Retriever)
    ↓
Send to AI Brain (Model + Prompt)
    ↓
Get Your Answer!
```

## Key Components Explained

### 1. **Model (The Brain)**
- **What it does**: Generates the final human-like response
- **Used here**: Google's Gemini 2.5 Flash model
- **Why**: It's the "thinking" part that understands context and creates answers

### 2. **Embeddings (The Translator)**
- **What it does**: Converts text into numerical vectors (lists of numbers)
- **Why**: Computers can't understand "experience" or "skills" directly, but they can compare numbers to find similarity
- **Example**: "Python programming" and "coding in Python" would have very similar number representations

### 3. **Document Loader (Data Entry)**
- **What it does**: Opens your PDF and reads the text
- **Output**: Raw text from your resume, split by pages

### 4. **Text Splitter (The Chopper)**
- **What it does**: Breaks your resume into small, digestible chunks (500 characters each)
- **Why**: Imagine searching a 500-page book vs. a well-indexed encyclopedia. Smaller chunks = faster, more precise answers
- **Overlap**: 100 characters overlap ensures we don't lose context between chunks

### 5. **Vector Store (The Library)**
- **What it does**: Stores all your resume chunks as numerical vectors
- **Used here**: FAISS (Facebook AI Similarity Search)
- **Why**: Allows lightning-fast searches through thousands of chunks to find the most relevant ones
- **Magic**: Your question is also converted to numbers, then the system finds chunks with the most similar numbers

### 6. **Retriever (The Librarian)**
- **What it does**: Takes your question, converts it to an embedding, and fetches the top 5 most relevant resume chunks
- **How**: Performs similarity search on the vector store

### 7. **Prompt (The Instructions)**
- **What it does**: Gives the AI model specific instructions on how to behave
- **Example**: "Act like a recruiter. Only use information from the resume. Be factual and concise."

### 8. **Output Parser**
- **What it does**: Cleans up the model's verbose response to give you just the answer

## How It All Connects

The **RAG Chain** is the complete workflow:

```python
Question → Retriever → Finds relevant chunks → 
Model receives (question + chunks + prompt) → 
Generates answer → Parser cleans it → Final response
```

## Setup Instructions

### Prerequisites
```bash
pip install langchain langchain-google-genai langchain-community 
pip install langchain-text-splitters pypdf faiss-cpu langchain_classic
```

### Get Your API Key

**Option 1: Google Gemini (Used in this implementation)**
1. Visit [Google AI Studio](https://aistudio.google.com/api-keys)
2. Generate your free API key
3. Add it to the code: `os.environ['GOOGLE_API_KEY'] = "YOUR_KEY_HERE"`

**Option 2: OpenAI (Alternative implementation)**
1. Visit [OpenAI Platform](https://platform.openai.com/api-keys)
2. Sign up and generate your API key
3. Add it to the code: `os.environ['OPENAI_API_KEY'] = "YOUR_KEY_HERE"`
4. Use the OpenAI version from `openai_rag.ipynb`

### Required Files
- Place your resume PDF in the same directory as the script
- Name it `resume.pdf` or update the loader path

## Example Questions You Can Ask

- "Summarize the candidate's profile in 5 bullet points"
- "What are the candidate's strongest technical skills?"
- "Does the candidate have backend development experience?"
- "Is the candidate suitable for a Data Scientist role?"
- "What kind of roles is this candidate best suited for?"
- "Give me top 5 interview questions based on working experience"

## Why RAG Instead of Just AI?

| Without RAG | With RAG |
|------------|----------|
| AI might hallucinate facts | Answers grounded in your actual resume |
| Generic responses | Specific, factual answers from your document |
| Limited context | Can handle large documents efficiently |
| No source verification | Always pulls from verified source (your PDF) |

## Technical Flow

1. **Indexing Phase** (One-time setup):
   - Load PDF → Split into chunks → Create embeddings → Store in vector database

2. **Query Phase** (Every question):
   - User asks question → Convert to embedding → Find similar chunks → 
   - Pass chunks + question to model → Generate answer → Return response

## Customization Options

- **Change Model**: Swap `gemini-2.5-flash` with any LangChain-supported model
- **Adjust Chunk Size**: Modify `chunk_size=500` for larger/smaller context windows
- **Retrieval Count**: Change `k=5` to retrieve more or fewer chunks
- **Temperature**: Adjust `temperature=0.5` for more creative (higher) or factual (lower) responses

## Use Cases

✅ **Recruiters**: Quickly screen resumes with targeted questions  
✅ **Job Seekers**: Prepare for interviews by identifying weak spots  
✅ **HR Teams**: Automate initial candidate assessments  
✅ **Career Coaches**: Analyze resumes systematically  

## Alternative: OpenAI Version

Want to use OpenAI instead of Google Gemini? Check out `openai_rag.ipynb` in the repository for the same implementation using:
- `ChatOpenAI` model
- `OpenAIEmbeddings`

---

**Built with**: LangChain, Google Gemini, FAISS, Python  
**License**: Open Source  
**Contributions**: Welcome!