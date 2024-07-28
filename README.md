
# Smart Document Assistant

Smart Document Assistant provides an interface to interact with and extract information from various document types. It supports `.pptx`, `.pdf`, and `.docx` files and leverages advanced NLP techniques for document understanding.

## Dependencies

The dependencies for this project are installed using the following commands:

```bash
# Install core libraries
!pip install -q -U torch datasets transformers tensorflow langchain playwright html2text sentence_transformers faiss-cpu
!pip install -q accelerate==0.21.0 peft==0.4.0 bitsandbytes==0.40.2 trl==0.4.7
!pip install pypdf2 python-docx python-pptx gradio
!playwright install
!playwright install-deps
```

## Project Flow

1. **Uploading File**
   - Supports `.pptx`, `.pdf`, and `.docx` files.
   - User-friendly file upload interface provided by Gradio.

2. **Document-Type Detection**
   - Identifies the document type based on the file extension.

3. **Parsing**
   - Calls specific parsers depending on the detected document type.

4. **Chunking**
   - Uses Langchain's `split_text()` function to create chunks from the parsed document.

5. **Creating Embeddings and Storing in Vector Stores**
   - Chunks are converted to embeddings using `HuggingFaceEmbeddings` and stored in a vector store using FAISS:
     ```python
     db = FAISS.from_texts(chunked_documents, HuggingFaceEmbeddings(model_name='sentence-transformers/all-mpnet-base-v2'))
     retriever = db.as_retriever()
     ```

6. **Creating RAG Chain**
   - Embeddings are used to create a RAG chain, interfacing with the Mistral 7B Large Language Model's API.

7. **Query Answering / Chat with Files**
   - Gradio-based interface allows users to ask questions and receive answers based on the uploaded document.

## How to Run

1. **Set Up Environment**
   - Run the commands from the **Dependencies** section to install required libraries.

2. **Run the Application**
   - Execute the final notebook or script to start the application.

3. **Interact with the Interface**
   - Use the Gradio interface to upload documents and ask questions.
