<h1 align="center"> 🎓Career Assistant: A Retrieval-Augmented Agentic Career Guide</h1>

**Career Assistant** is an AI tool designed to help with career decision-making. It assesses user personality and skills through interactive interviews and recommends suitable career paths using a comprehensive knowledge graph based on the RIASEC model.

<h2>⚙️To use the app locally follow these <strong>step by step</strong> instructions</h2>

<li><strong>Step 1:</strong> Create a .env file in your root directory</li>

</br>

<li><strong>Step 2:</strong> Create a virtual environment and install the necessary dependencies</li>    
  
  ```
    # Create virtual environment
    python -m venv venv

    # Activate it (Windows)
    .\venv\Scripts\activate

    # Activate it (Mac/Linux)
    source venv/bin/activate

    # Install requirements
    pip install -r requirements.txt
   ```

<li><strong>Step 3:</strong> Setup Groq for LLMs:
  <ol>
    <li>Create and login to your account in Groq: https://console.groq.com/keys. <br/></li>
    <li>Go to API Keys and create an api key (it's free): https://console.groq.com/keys. <br/></li>
    <li>Copy and then add this api key to your .env as: GROQ_API_KEY = "your new api key..." <br/></li>
  </ol>
</li>

<br/>

<li><strong>Step 4:</strong> View the CSV files we will use to add data into Neo4j:</li>

I created [graph_functions.py](https://github.com/MarcDagher/Career_Assistant/blob/main/Knowledge_Graph/CSV_to_Knowledge_Graph/graph_functions.py/) in order to populate the data into the Neo4j graph. For this, we need the CSVs in a specific format. In [Knowledge_Graph/Combined CSVs](https://github.com/MarcDagher/Career_Assistant/tree/main/Knowledge_Graph/Combined%20CSVs/) we have the original CSVs from ONet. They need to be in a specific format for us to use them with graph_functions.py. The formatted CSVs are ready and you can see them in [Knowledge_Graph/Formatted CSVs](https://github.com/MarcDagher/Career_Assistant/tree/main/Knowledge_Graph/Formatted%20CSVs/). But, if you want to see how I formatted them check [this](https://github.com/MarcDagher/Career_Assistant/blob/main/Knowledge_Graph/CSV_to_Knowledge_Graph/format_csvs.ipynb).

<br/> <br/>

<li><strong>Step 5:</strong> Setup Neo4j:
  <ol>
    <li>To read about the installation details go to: https://neo4j.com/docs/desktop-manual/current/installation/download-installation/.</li>
    <li>To install Neo4j desktop go to: https://neo4j.com/deployment-center/?desktop-gdb.</li>
    <li>Search for Neo4j Desktop (It should be the 3rd box).</li> 
    <li>Choose your os (ex: I'm on windows) and click download.</li> 
    <li>Fill in your info and click download desktop.</li> 
    <li>You should see an activation key pop-up, copy it. After download is complete, insert the Software Key when prompted to do so.</li> 
    <li>Open Neo4j's Setup.</li> 
    <li>Choose installation options, click next, then click install.</li> 
    <li>Click finish and open Neo4j Desktop.</li> 
    <li>When Neo4j desktop opens, click on +New to create a new project.</li> 
    <li>When the new project shows up, click on +Add on the top right then choose Local DBMS.</li> 
    <li>Call it whatever you want, I will name it "Career_Assistant_KG" and write a password then click on create.</li> 
    <li>Click on start to start the Knowledge Graph.</li> 
    <li>Go to your .env file and add 3 variables: 
      <ol>
        <li>NEO4J_URI = 'bolt://localhost:7687' or another port of your choosing. make sure to use bolt.</li>
        <li>NEO4J_USERNAME usually is "neo4j" by default.</li>
        <li>NEO4J_PASSWORD = "the password you set earlier".</li>
      </ol>
    </li>
    <br/>
    <strong>*Very Important:*</strong> 
    <li>Go to Neo4j desktop, hover over your project, click on the settings (...) next to open, click on Open Folder, click on Plugins, this should take you to a local folder called plugins.</li>
    <li>Navigate to the parent folder of plugins, you should see bin, certificates...  labs. Go inside labs, copy the file apoc-5.24.0-core.jar and paste it in the plugins folder.</li>
    <li>Then go back to the parent folder, go to conf, then neo4j.conf. Open it as a txt file, cntrl-f, search for dbms.security.procedures.unrestricted and assign it to apoc.* (dbms.security.procedures.unrestricted=apoc.*)</li>
  </ol>
</li>

</br>

<li><strong>Step 6:</strong> Populate the graph with data from formatted CSVs:
  <ol>
    <li>

To populate the graph go to [create_graph_from_structured_data.ipynb](https://github.com/MarcDagher/Career_Assistant/blob/main/Knowledge_Graph/CSV_to_Knowledge_Graph/create_graph_from_structured_data.ipynb).</li>    
    <li>Run the cells to add the data.</br> 
    NOTE: in create_graph_from_structured_data you can see the CSV files that we could add. However, we only added the first 150 rows of one CSV file due to the usage limits of the Groq's free version. Ideally, we would want to add all the data which would result in a graph pf around 90,000 relations between nodes. (This will also add latency) </li>
    <li> Extra step: Go to Neo4j Desktop, hover over your new Project/DB, click on Open to open a browser. Inside the browser click on the first cell next to neo4j$ and write the following code to view the 150 relations we have created: 
    
    
      Match (n) return n limit 150
     
    
</li>
  </ol>
</li>

</br>

<li><strong>Step 7:</strong> Langsmith

  <ol>
    <li>Go to https://langsmith.com and sign in.</li> 
    <li>Click create project.</li>
    <li>You should see Set up observability.</li>
    <li>Click generate api key.</li>
    <li>Copy the box from number 3 and paste it in your .env file.</li>
    <li>Click back (you shouldn't see any changes until we run an llm call).</li>    
  </ol>
</li>

</br>

<li><strong>Step 8:</strong> To launch the app:<br/>
  <ol>
    <li> run this in the terminal: 
    
  ```
    python Agent_App/fast_api_server.py 
  ```
</li>
    <li> run this in the terminal: 
  
  ```
    streamlit run Agent_App/streamlit_app.py 
  ```
</li>
After you interact with the LLM, you should be able to see the tracing in langsmith "Tracing Projects" which is found in the left side-bar.
  </ol>
</li>

</br>

<h2>🕸️Knowledge Graph</h2>
<p>To ensure accurate personality assessment and job recommendations, the system uses a <a href="https://neo4j.com/">Neo4j</a> knowledge graph based on the RIASEC personality model and the <a href="https://www.onetonline.org/find/descriptor/browse/1.C">O*NET dataset</a> to match user traits with suitable career paths. The system asks users a series of questions to analyze their personality traits and then queries a knowledge graph to suggest suitable occupations based on these traits. The key dataset used to create this knowledge graph is the O*NET Dataset, an extensive resource developed by the U.S. Department of Labor to provide detailed and comprehensive information about various occupations.</p>

To populate the knowledge graph with the extracted data from O*Net, a specific <a                                                                            href="https://github.com/MarcDagher/Career_Assistant/blob/main/Knowledge_Graph/CSV_to_Knowledge_Graph/format_csvs.ipynb">CSV format</a> was followed. The CSV format contains three columns: “Node_1,” “Node_2,” and “Relation.” Every node should have a “label,” “property,” and “identifier,” while every relation should have a “label” and optional “properties.” An example row in the CSV would look like this:
<li>Node_1: {'label': 'Occupation', 'properties': "{'title': 'Psychologist'}", 'identifier': "{'title': 'Psychologist'}"}</li>
<li>Node_2: {'label': 'Basic_Skill', 'properties': "{'title': 'Good Listener'}", 'identifier': "{'title': 'Good Listener'}"}</li>
<li>Relation: {'label': 'need_for_basic_skill', 'properties': "{'level': 'high'}"}</li> <br>

<h2>🤖Agent</h2>

The system carries the following tasks:
<li>Conducts a personality test</li>
<li>Decides when to query the graph</li>
<li>Writes cypher code and queries the graph</li> 
<li>Extracts whatever it needs from the queried data</li> 
<li>Suggests suitable career tracks</li> 
<li>Answers any questions</li><br>
To orchestrate this <a href="https://github.com/MarcDagher/Career_Assistant/blob/main/Images_and_Videos/agent.jpg">workflow</a> we used <a href="https://www.langchain.com/langgraph">LangGraph</a> and <a href="https://console.groq.com/docs/models">Groq</a>'s llama-3.2-90b-preview for its tool-use capabilities.

<h2>⚙️Prompt Engineering</h2>

When designing prompts for the agent, each step in the workflow matched a specific task in order to improve response accuracy. Since the system’s workflow contains multiple steps, each step was separated into unique tasks with specific prompts. This approach led to smaller prompts, less tasks on an individual LLM, and fewer hallucinations throughout the system, resulting in more accurate results.

With prompting strategies like zero-shot and few-shot prompting, the system encountered inconsistencies and hallucinations from the LLMs, even with task separation and workload reduction. While example outputs improved format, they did not significantly enhance content, and few-shot prompting failed to deliver accurate results.

To address these issues, structured task completion prompts were developed, inspired by chain-of-thought (CoT) prompting, which encourages coherent intermediate reasoning steps. By guiding the LLM through a step-by-step reasoning process, the system achieved improved coherence and accuracy. This approach enabled the LLMs to generate more logical and relevant outputs, effectively adapting to user interactions.

You can check the prompts <a href="https://github.com/MarcDagher/Career_Assistant/blob/main/Agent_App/FastAPI_Sub_Folder/Helpers/prompts.py">here</a>

<h2>📝LLM Evaluation</h2>

<a href="https://github.com/MarcDagher/Career_Assistant/blob/main/LLM_Evaluation/gpt_synthetic_data.csv">Synthetic data</a> was generated to evaluate the LLM's generation by simulating typical user-agent conversations with GPT-4o mini, creating a ground truth dataset formatted as a CSV file with three columns: conversation, extracted data, and output. This dataset was limited to 22 rows due to LLM rate limits and manually reviewed for applicability. <br>

<a href="https://docs.confident-ai.com/">DeepEval</a>, an evaluation framework for AI models which supports multi-task evaluations and custom metrics, was used for evaluation. The LLM's assessment metrics included:
<li>Answer Relevancy: Relevance of the output to the input</li>
<li>Faithfulness: Alignment of the output with the retrieval context</li>
<li>Logical Suggestions: Coherence of career suggestions based on user-agent conversations</li> <br>
The agent performed well, scoring 90-95% across all metrics. Additionally, LangSmith was used for continuous monitoring of the agent's workflows, which streamlined tracking, debugging, and optimizing AI models, ensuring consistent, high-quality outputs from the LLM.
<a href="https://github.com/MarcDagher/Career_Assistant/blob/main/LLM_Evaluation/eval.ipynb">This</a> is the link to the evaluation notebook.

<h2>🎬Demo</h2>
In this demo, you will see the interaction with the conversational agent, as well as a visualization of the queried data from the knowledge graph and the data that the agent extracted from it. The app was built using FastAPI and Streamlit. <br><br>

![Demo](https://github.com/MarcDagher/Career_Assistant/blob/main/Images_and_Videos/demo%20(1).gif)
