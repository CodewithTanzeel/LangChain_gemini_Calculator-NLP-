# LangChain-Gemini-Agent

## Overview
This project integrates Google's Gemini model with LangChain to create an AI-powered conversational agent. It leverages LangChain's agent framework, memory management, and tool system to process natural language queries, maintain conversation history, and perform mathematical computations through a custom calculator tool.

## Features
- **Conversational AI**: Utilizes Gemini AI for advanced natural language processing.
- **Tool Integration**: Implements a calculator tool for evaluating mathematical expressions.
- **Memory Management**: Retains conversation history via LangChain’s memory modules.
- **Agent Execution**: Uses LangChain’s agent system to handle tool invocation dynamically.

---

## Installation

### Prerequisites
Ensure the following dependencies are installed:
- Python 3.8+
- Google Colab or Jupyter Notebook
- Required Python packages

### Install Dependencies
```bash
pip install langchain langchain_google_genai
```

### Set Up Environment Variables
Before executing the script, configure your Google API key:
```python
import os
os.environ["GOOGLE_API_KEY"] = "your_google_api_key"
```

---

## Implementation Details

### 1. Calculator Class
```python
class Calculator:
    def calculate(self, expression: str) -> str:
        try:
            result = eval(expression, {"__builtins__": None}, {})
            return str(result)
        except Exception as e:
            return f"Error: {e}"
```
- Defines a `Calculator` class to evaluate mathematical expressions securely.
- Uses Python’s `eval()` with restricted built-in function access.

---

### 2. Initializing the Gemini Model
```python
from langchain_google_genai import ChatGoogleGenerativeAI

gemini_model = ChatGoogleGenerativeAI(
    model="gemini-1.5-flash", api_key=os.environ["GOOGLE_API_KEY"]
)
```
- Instantiates the Gemini AI model using LangChain’s `ChatGoogleGenerativeAI` class.
- Requires an API key stored in environment variables.

---

### 3. Defining the Calculator as a Tool
```python
from langchain.agents import Tool

calculator = Calculator()
calculator_tool = Tool(
    name="Calculator",
    func=calculator.calculate,
    description="A simple calculator that evaluates mathematical expressions."
)
```
- Wraps the `Calculator` instance as a LangChain `Tool`.
- Assigns a name and description for agent interaction.

---

### 4. Initializing the Agent
```python
from langchain.agents import initialize_agent, AgentType

tools = [calculator_tool]
agent_executor = initialize_agent(
    tools=tools,
    llm=gemini_model,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)
```
- Defines the agent using `initialize_agent`.
- Uses `ZERO_SHOT_REACT_DESCRIPTION` for automatic tool invocation.
- Enables verbosity for debugging.

---

### 5. Setting Up Memory for Conversations
```python
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory()
```
- Utilizes `ConversationBufferMemory` to persist and retrieve conversation history.

---

### 6. Creating a Conversational Chain
```python
from langchain_core.runnables.history import RunnableWithMessageHistory

chain = RunnableWithMessageHistory(
    runnable=gemini_model,
    get_session_history=lambda _: memory
)
```
- Implements `RunnableWithMessageHistory` for memory integration in the chatbot.
- Ensures contextual continuity during conversations.

---

## Usage
```python
response = agent_executor.run("What is 50 divided by 5?")
print(response)
```
- Executes the agent with a sample query.
- Returns AI-generated responses along with tool execution when necessary.

---

## Future Enhancements
- Expand tool functionalities beyond basic arithmetic.
- Integrate additional memory types for enhanced context tracking.
- Deploy as an API for real-world applications.

## Author
**Tanzeel Ahmad**

## Copyright
&copy; 2025 Tanzeel Ahmad. All rights reserved.

## License
This project is licensed under the MIT License.

