# Data_Analysis_Agent

Overview of the Data Interaction Agent
Frameworks and Models Used
Framework/Model	Purpose	Specific Use
LangChain	Facilitates interaction with LLMs and data sources	create_pandas_dataframe_agent function to create an agent for processing and interacting with Pandas DataFrames
OpenAI	Provides LLMs for generating responses	Create an OpenAI object to access GPT models
Streamlit	Creates interactive web applications	User interface for uploading CSV files, entering queries, and displaying results
Python-environ	Manages environment variables	Load the OpenAI API key from a .env file
Step-by-Step Workflow
1. Setting Up the Environment
Install Libraries: Install LangChain, OpenAI, Streamlit, and python-environ.
API Key Management: Create a .env file to store the OpenAI API key securely.
2. Creating the Agent
Load API Key: Use python-environ to read the API key from the .env file.
Read Data: Load the CSV file into a Pandas DataFrame.
Initialize LLM: Create an OpenAI object for accessing the LLM.
Create Agent: Use LangChain’s create_pandas_dataframe_agent to generate an agent that can interact with the LLM and DataFrame.
3. Querying the Agent
Formulate the Prompt: Define how the agent should respond to various query types, including tables, bar charts, line charts, and text answers.
Process the Query: Send the user’s query to the agent, which processes it and generates an appropriate response based on the data.
4. Setting Up the Streamlit Interface
User Interface: Build an interface to allow users to upload CSV files, enter queries, and submit them.
Handle File Upload: Enable users to upload their CSV files.
Handle Queries: Accept and process user queries through the agent.
Display Responses: Show the results in the Streamlit app, including text answers, tables, bar charts, or line charts, depending on the agent’s response.
5. Running the Application
Start the App: Launch the Streamlit app to open a browser window for user interaction.
Upload CSV and Query: Users upload CSV files and submit queries. The app processes the queries and displays results accordingly.
