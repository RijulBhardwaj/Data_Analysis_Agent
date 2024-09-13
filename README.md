# Data_Analysis_Agent

Frameworks and Models Used
LangChain:

Purpose: Facilitates interaction with large language models (LLMs) and integrates them with data sources.
Specific Use: create_pandas_dataframe_agent function is used to create an agent that can process and interact with data in a Pandas DataFrame.
OpenAI:

Purpose: Provides the LLM (such as GPT models) that the agent will use for generating responses.
Specific Use: An OpenAI object is created with an API key to access the LLM.
Streamlit:

Purpose: Creates an interactive web application to allow users to interact with the CSV data and visualize results.
Specific Use: Provides the user interface for uploading CSV files, entering queries, and displaying responses.
Python-environ:

Purpose: Manages environment variables, such as API keys.
Specific Use: Loads the OpenAI API key from a .env file.
Step-by-Step Workflow
Setting Up the Environment:

Install required libraries: LangChain, OpenAI, Streamlit, and python-environ.
Create a .env file to securely store the OpenAI API key.
Creating the Agent:

Load the API Key: Use python-environ to load the OpenAI API key from the .env file.
Read Data: Load the CSV data into a Pandas DataFrame.
Initialize the LLM: Create an OpenAI object to access the language model.
Create the Agent: Use LangChain’s create_pandas_dataframe_agent to create an agent that can interact with the LLM and the DataFrame.
Querying the Agent:

Formulate the Prompt: Define how the agent should respond to different types of queries, such as generating tables, bar charts, line charts, or text answers.
Process the Query: Pass the user’s query to the agent, which processes it and generates a response based on the data.
Setting Up the Streamlit Interface:

User Interface: Create an interface where users can upload a CSV file, enter a query, and submit it.
Handle File Upload: Allow users to upload their CSV files.
Handle Queries: Accept user queries and send them to the agent for processing.
Display Responses: Based on the agent's response, display the results in the Streamlit app. This could include text answers, tables, bar charts, or line charts.
Running the Application:

Start the App: Launch the Streamlit app using a command. This opens a browser window where users can interact with the application.
Upload CSV and Query: Users upload their CSV files and submit queries. The app then processes the query through the agent and displays the results in the appropriate format.
Summary
The application is built using LangChain for integrating LLMs with data, OpenAI for providing the LLM, and Streamlit for creating a user-friendly web interface. The workflow involves setting up the environment, creating an agent to process data and queries, and using Streamlit to build an interactive interface for users to interact with their data and view the results.
