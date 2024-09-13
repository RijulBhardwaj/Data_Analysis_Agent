# Data_Analysis_Agent



---

# CSV Query Agent

## Getting Started

To get started, you'll need to install the following packages:

| Package         | Installation Command           |
|-----------------|--------------------------------|
| `langchain`     | `pip install langchain`        |
| `openai`        | `pip install openai`           |
| `streamlit`     | `pip install streamlit`        |
| `python-environ`| `pip install python-environ`   |
| `tabulate`      | `pip install tabulate`         |

## Setting Up the Agent

We're using `create_pandas_dataframe_agent` from LangChain. An agent is a software program that can access and use a large language model (LLM). The agent processes user input and generates responses by leveraging data from sources such as CSV files.

### Steps to Set Up

1. **Create a `.env` File**:
   Add your OpenAI API key to a `.env` file:
   ```
   apikey=your_openai_api_key
   ```

2. **Create `agent.py`**:
   - **Imports and API Key Setup**:
     ```python
     from langchain import OpenAI
     from langchain.agents import create_pandas_dataframe_agent
     import pandas as pd
     import environ

     env = environ.Env()
     environ.Env.read_env()

     API_KEY = env("apikey")
     ```

   - **Create the Agent**:
     ```python
     def create_agent(filename: str):
         """
         Create an agent that can access and use a large language model (LLM).

         Args:
             filename: The path to the CSV file that contains the data.

         Returns:
             An agent that can access and use the LLM.
         """

         # Create an OpenAI object.
         llm = OpenAI(openai_api_key=API_KEY)

         # Read the CSV file into a Pandas DataFrame.
         df = pd.read_csv(filename)

         # Create a Pandas DataFrame agent.
         return create_pandas_dataframe_agent(llm, df, verbose=False)
     ```

   - **Query the Agent**:
     ```python
     def query_agent(agent, query):
         """
         Query an agent and return the response as a string.

         Args:
             agent: The agent to query.
             query: The query to ask the agent.

         Returns:
             The response from the agent as a string.
         """

         prompt = (
             """
                 For the following query, if it requires drawing a table, reply as follows:
                 {"table": {"columns": ["column1", "column2", ...], "data": [[value1, value2, ...], [value1, value2, ...], ...]}}

                 If the query requires creating a bar chart, reply as follows:
                 {"bar": {"columns": ["A", "B", "C", ...], "data": [25, 24, 10, ...]}}

                 If the query requires creating a line chart, reply as follows:
                 {"line": {"columns": ["A", "B", "C", ...], "data": [25, 24, 10, ...]}}

                 There can only be two types of chart, "bar" and "line".

                 If it is just asking a question that requires neither, reply as follows:
                 {"answer": "answer"}
                 Example:
                 {"answer": "The title with the highest rating is 'Gilead'"}

                 If you do not know the answer, reply as follows:
                 {"answer": "I do not know."}

                 Return all output as a string.

                 All strings in "columns" list and data list, should be in double quotes,

                 For example: {"columns": ["title", "ratings_count"], "data": [["Gilead", 361], ["Spider's Web", 5164]]}

                 Lets think step by step.

                 Below is the query.
                 Query: 
                 """
             + query
         )

         # Run the prompt through the agent.
         response = agent.run(prompt)

         # Convert the response to a string.
         return response.__str__()
     ```

## Setting Up the Streamlit Interface

Streamlit is a Python library for creating web apps for machine learning and data science. Create a file named `interface.py`:

### Imports and Decode Function

```python
import streamlit as st
import pandas as pd
import json
from agent import query_agent, create_agent

def decode_response(response: str) -> dict:
    """Convert the string response from the model to a dictionary object.

    Args:
        response (str): Response from the model

    Returns:
        dict: Dictionary with response data
    """
    return json.loads(response)
```

### Write Response Function

```python
def write_response(response_dict: dict):
    """
    Write a response from an agent to a Streamlit app.

    Args:
        response_dict: The response from the agent.

    Returns:
        None.
    """

    # Check if the response is an answer.
    if "answer" in response_dict:
        st.write(response_dict["answer"])

    # Check if the response is a bar chart.
    if "bar" in response_dict:
        data = response_dict["bar"]
        df = pd.DataFrame(data)
        df.set_index("columns", inplace=True)
        st.bar_chart(df)

    # Check if the response is a line chart.
    if "line" in response_dict:
        data = response_dict["line"]
        df = pd.DataFrame(data)
        df.set_index("columns", inplace=True)
        st.line_chart(df)

    # Check if the response is a table.
    if "table" in response_dict:
        data = response_dict["table"]
        df = pd.DataFrame(data["data"], columns=data["columns"])
        st.table(df)
```

### Create Streamlit Interface

```python
st.title("👨‍💻 Chat with your CSV")

st.write("Please upload your CSV file below.")

data = st.file_uploader("Upload a CSV")

query = st.text_area("Insert your query")

if st.button("Submit Query", type="primary"):
    # Create an agent from the CSV file.
    agent = create_agent(data)

    # Query the agent.
    response = query_agent(agent=agent, query=query)

    # Decode the response.
    decoded_response = decode_response(response)

    # Write the response to the Streamlit app.
    write_response(decoded_response)
```

## How It Works

1. **Setup**:
   - Install necessary packages.
   - Configure the API key for OpenAI. We use the gpt 3.5 turbo model in this.

2. **Agent Creation**:
   - **`create_agent`**: Reads a CSV file into a Pandas DataFrame and creates an agent using LangChain's `create_pandas_dataframe_agent`.

3. **Query Processing**:
   - **`query_agent`**: Constructs a prompt based on user queries, sends it to the agent, and processes the response.

4. **Interface Setup**:
   - **Streamlit**: Provides a user interface for file upload and query submission. It processes the agent's response and displays results in the form of text, tables, or charts.

By following these steps, you can interact with your CSV data and generate responses dynamically based on user queries.

---
