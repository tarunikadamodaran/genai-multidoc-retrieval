## Design and Implementation of a Multidocument Retrieval Agent Using LlamaIndex

### AIM:
To design and implement a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles, and to evaluate its performance by testing it with diverse queries, analyzing its ability to deliver concise, relevant, and accurate responses.

### PROBLEM STATEMENT:
The objective is to create an agent that can handle multiple research articles or documents and retrieve relevant information based on user queries. By leveraging LlamaIndex, we aim to build an efficient retrieval system that can access multiple documents, extract the necessary data, and synthesize it into meaningful responses, improving the speed and accuracy of information retrieval.
### DESIGN STEPS:
Algorithm for PDF Analysis and Query Processing Using Agent Tools
Input Initialization Inputs: urls: A list of URLs pointing to the research papers (optional for download). papers: Local filenames of the PDF files. Output: Tools for each paper (vector_tool and summary_tool).
Set Up Document Processing Tools Import the get_doc_tools function for generating document-specific tools. Iterate over the papers list: Print the current paper being processed. For each paper, call get_doc_tools with: The paper file path. The stem of the file path (used as an identifier). Store the generated tools (vector_tool and summary_tool) in a dictionary, mapping to the paper.
Initialize Tools Combine all tools from the dictionary into a single list (initial_tools).
Set Up the LLM Import and initialize the OpenAI LLM: Use the gpt-4 model.
Configure the Agent Worker Import FunctionCallingAgentWorker and AgentRunner to manage agent functionalities. Create an agent worker using FunctionCallingAgentWorker.from_tools: Pass the combined initial_tools list. Set the LLM (llm). Enable verbose output for detailed logging.
Run the Agent Query Create an AgentRunner instance with the configured worker. Query the agent with a question: Include specific queries about datasets and results from the LongLoRA paper.
Output the Result Retrieve and display the response from the agent.
#### STEP 1:
Collect and prepare research papers in PDF format. Download relevant papers such as MetaGPT, LongLoRA, and Self-RAG, and save them locally
#### STEP 2:
Use get_doc_tools from a utility script to create vector-based retrieval and summarization tools for each paper using LlamaIndex. These tools enable semantic search and summarization functionalities.
#### STEP 3:
Initialize an agent using the FunctionCallingAgentWorker and AgentRunner classes from LlamaIndex and print the response for your query.
### PROGRAM:
```
from helper import get_openai_api_key
OPENAI_API_KEY = get_openai_api_key()
import nest_asyncio
nest_asyncio.apply()urls = [
    "https://openreview.net/pdf?id=Mgyr7EY7X9",
    "https://openreview.net/pdf?id=RpQya1h1dh",
    "https://openreview.net/pdf?id=ptiLVPeOWD",
]

papers = [
    "formalspecific.pdf",
    "Responsibility.pdf",
    "RightReasons.pdf",
]
from utils import get_doc_tools
from pathlib import Path

paper_to_tools_dict = {}
for paper in papers:
    print(f"Getting tools for paper: {paper}")
    vector_tool, summary_tool = get_doc_tools(paper, Path(paper).stem)
    paper_to_tools_dict[paper] = [vector_tool, summary_tool]
initial_tools = [t for paper in papers for t in paper_to_tools_dict[paper]]from llama_index.llms.openai import OpenAI

llm = OpenAI(model="gpt-3.5-turbo")
len(initial_tools)from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.agent import AgentRunner

agent_worker = FunctionCallingAgentWorker.from_tools(
    initial_tools, 
    llm=llm, 
    verbose=True
)
agent = AgentRunner(agent_worker)
response = agent.query(
    "what is The need for formal specifications of morally relevant information?"
 )
response = agent.query("Give me a summary of the three pdfs")
print(str(response))
```

### OUTPUT:
Added user message to memory: what is The need for formal specifications of morally relevant information?
=== Calling Function ===
Calling function: vector_tool_formalspecific with args: {"query": "The need for formal specifications of morally relevant information"}
=== Function Output ===
The need for formal specifications of morally relevant information arises from the limitations of informal specifications in enabling automated moral reasoning and algorithmic decision-making. Formal specifications are essential to go beyond simply comparing choices based on values and to provide a theory explaining why one option is morally superior. They help in resolving conflicts by introducing more structure, such as argumentation theory, and are crucial for justifying the morality of an option in morally conflicting situations. Additionally, formal specifications clarify the semantics and distinctions between values, factors, principles, norms, moral judgments, and moral decisions, aiding in disciplinary clarity and facilitating comparison with related work in fields like machine ethics and AI alignment.
=== Calling Function ===
Calling function: summary_tool_formalspecific with args: {"input": "The need for formal specifications of morally relevant information arises from the limitations of informal specifications in enabling automated moral reasoning and algorithmic decision-making. Formal specifications are essential to go beyond simply comparing choices based on values and to provide a theory explaining why one option is morally superior. They help in resolving conflicts by introducing more structure, such as argumentation theory, and are crucial for justifying the morality of an option in morally conflicting situations. Additionally, formal specifications clarify the semantics and distinctions between values, factors, principles, norms, moral judgments, and moral decisions, aiding in disciplinary clarity and facilitating comparison with related work in fields like machine ethics and AI alignment."}
=== Function Output ===
Formal specifications of morally relevant information are crucial for enabling automated moral reasoning and algorithmic decision-making. They provide a structured framework that surpasses simple value-based comparisons, aiding in resolving conflicts, justifying moral superiority, and clarifying the distinctions between various moral aspects. This clarity enhances disciplinary understanding and allows for comparisons with related work in fields such as machine ethics and AI alignment.
=== LLM Response ===
The need for formal specifications of morally relevant information is essential for enabling automated moral reasoning and algorithmic decision-making. Formal specifications provide a structured framework that goes beyond simple value-based comparisons, aiding in resolving conflicts, justifying moral superiority, and clarifying the distinctions between various moral aspects. This clarity enhances disciplinary understanding and allows for comparisons with related work in fields such as machine ethics and AI alignment.

### RESULT:
A multidocument retrieval agent was successfully designed and implemented using LlamaIndex. The system was able to extract and synthesize information from multiple research articles and responded effectively to diverse queries. It demonstrated the ability to deliver concise, relevant, and accurate responses, validating its performance and usefulness for multi-document analysis tasks.
