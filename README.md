User InputChatbot Response"Hi""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Hello""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Can you help with hacking?""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Tell me about Solution A""[Provide information about Solution A]""What is the technical architecture?""[Provide information on the technical architecture of the solution]""Describe the benefits of Solution B""[Provide the benefits of Solution B]""Goodbye""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Can you help with React JS?""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions."









To instruct your chatbot not to respond to general questions like greetings, you can use a prompt similar to the following:

---

**System Prompt:**

"Your role is to provide specific and detailed information only. If the user asks general or conversational questions such as greetings, chit-chat, or unrelated topics, politely decline to answer and redirect them to focus on relevant queries."

**Example User Interactions:**

1. **User:** "Hello!"
   **Chatbot:** "I'm here to assist with specific queries. Please ask your question related to [insert domain/topic]."

2. **User:** "How are you?"
   **Chatbot:** "Let's focus on your questions. How can I assist you with [insert domain/topic]?"

3. **User:** "What's the weather like?"
   **Chatbot:** "I'm here to help with questions about [insert domain/topic]. Please let me know how I can assist you."

---

This prompt will help your chatbot to stay focused on the intended purpose and avoid engaging in general conversation.









import streamlit as st
import streamlit as st
import uuid
import boto3
import bedrock
import warnings
warnings.filterwarnings('ignore')
import json
import os
import sys
import requests
import json
import base64
import pandas as pd 
from langchain.chains import RetrievalQA
from langchain.document_loaders import TextLoader
from langchain.text_splitter import CharacterTextSplitter
from langchain import PromptTemplate
from langchain.chains.question_answering import load_qa_chain
from langchain.document_loaders.csv_loader import CSVLoader
from langchain.docstore.document import Document
from langchain.chains import VectorDBQA,RetrievalQA


## Q&A - Title ###
st.header("❓ KB Assist Only ChatBot 🔎", divider='blue')
# Define the text to be displayed with placeholders
text_template_write = "<span style='color:{color}; font-size:{font_size}; font-style:{font_style};'>{content}</span>"
# Fill in the placeholders with desired values
color = "yellow"  # Specify the color (e.g., "blue", "red", "#FFA500")
font_size = "17px"  # Specify the font size (e.g., "16px", "20px")
font_style = "bold"  # Specify the font style (e.g., "normal", "italic", "oblique")
content = "[ The Smart Way to Get Answers ]"  # Specify the content of the text
# Format the text string with the specified values
text = text_template_write.format(color=color, font_size=font_size, font_style=font_style, content=content)

# Display the text using st.write
st.write(text, unsafe_allow_html=True)  
##---END---Q&A - Title ###

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https:dummy/"

def handle_input(user_input):
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
        st.session_state.questions.append({"question": user_input, "id": len(st.session_state.questions)})
        st.session_state.answers.append({"answer": response.json(), "id": len(st.session_state.answers)})
        
        print(f"User's input: {user_input}")
    else:
        st.error("Failed to connect to Lambda.")


### Changed 9Aug -AutoComplete
## Not working -> showing 2 boxes and 2nd box only working on enter 
USER_ICON = "user-icon.png"
AI_ICON = "ai-icon.png"

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

if "llm_chain" not in st.session_state:
    st.session_state["llm_app"] = "bedrock"  # Placeholder for your bedrock model
    st.session_state["llm_chain"] = "bedrock_chain"  # Placeholder for your bedrock chain

if "questions" not in st.session_state:
    st.session_state.questions = []

if "answers" not in st.session_state:
    st.session_state.answers = []

if "input" not in st.session_state:
    st.session_state.input = ""

    # Orig lambda url :
# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

def write_top_bar():
    col1, col2, col3 = st.columns([2, 10, 3])
    with col3:
        clear = st.button("Clear Chat")
    return clear

clear = write_top_bar()

if clear:
    st.session_state.questions = []
    st.session_state.answers = []
    st.session_state.input = ""

# def handle_input():
#     input = st.session_state.input
# #         lambda_url = "pop"  # Replace with your endpoint
#     lambda_url="https://pop.lambda-url.us-east-1.on.aws/" # bp-kb-assist-lambda
#     api_url = lambda_url
#     responses = requests.post(api_url, json={'func': 'generate_response', 'question': input})

#     if responses.status_code == 200:
#         response_text = responses.text
#     else:
#         response_text = "Error: Unable to fetch response."

#     st.session_state.questions.append({"question": input, "id": len(st.session_state.questions)})
#     st.session_state.answers.append({"answer": response_text, "id": len(st.session_state.answers)})
#     st.session_state.input = ""

def write_user_message(md):
    col1, col2 = st.columns([1, 12])
    with col1:
        st.image(USER_ICON, use_column_width="always", caption="User")
    with col2:
        st.warning(md["question"])

def render_answer(answer):
    col1, col2 = st.columns([1, 12])
    with col1:
        st.image(AI_ICON, use_column_width="always", caption="Bot")
    with col2:
        st.info(answer["answer"])

def write_chat_message(md):
    chat = st.container()
    with chat:
        render_answer(md)

# Display the chat history
with st.container():
    for q, a in zip(st.session_state.questions, st.session_state.answers):
        write_user_message(q)
        write_chat_message(a)

st.markdown("---")



# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Create the HTML and JavaScript code for autocomplete
autocomplete_html = f"""
<div class="search-container">
    <input type="text" id="autocomplete" placeholder="Start typing to see suggestions..." oninput="onInputChange()" onkeydown="if (event.key === 'Enter') onEnterPress()">
    <ul class="suggestions" id="suggestions-list"></ul>
</div>

<script>
    function onInputChange() {{
        var input = document.getElementById('autocomplete').value.toLowerCase();
        var suggestionsList = document.getElementById('suggestions-list');
        suggestionsList.innerHTML = '';

        var matchingSuggestions = {suggestions}.filter(function(suggestion) {{
            return suggestion.toLowerCase().includes(input);
        }});

        matchingSuggestions.forEach(function(suggestion) {{
            var listItem = document.createElement('li');
            listItem.textContent = suggestion;
            listItem.addEventListener('click', function() {{
                document.getElementById('autocomplete').value = suggestion;
                suggestionsList.style.display = 'none';
                window.parent.handle_input(suggestion);
            }});
            suggestionsList.appendChild(listItem);
        }});

        if (matchingSuggestions.length > 0) {{
            suggestionsList.style.display = 'block';
        }} else {{
            suggestionsList.style.display = 'none';
        }}
    }}

    function onEnterPress() {{
        var inputField = document.getElementById('autocomplete');
        var inputValue = inputField.value;
        inputField.value = ''; // Clear the input field
        window.parent.handle_input(inputValue);
    }}
</script>





<style>
    body {{
        font-family: "Arial", sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
        background-color: #f0f0f5;
    }}

    .search-container {{
        position: relative;
        width: 400px;
        max-width: 90%;
    }}

    #autocomplete {{
        width: 100%;
        padding: 12px 16px;
        font-size: 18px;
        border: 2px solid #ddd;
        border-radius: 30px;
        outline: none;
        transition: border-color 0.3s, box-shadow 0.3s;
    }}

    #autocomplete:focus {{
        border-color: #007bff;
        box-shadow: 0 0 10px rgba(0, 123, 255, 0.2);
    }}

    .suggestions {{
        list-style: none;
        padding: 0;
        margin: 8px 0 0;
        border: 1px solid #ddd;
        border-radius: 8px;
        max-height: 200px;
        overflow-y: auto;
        display: none;
        background-color: #fff;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        animation: fadeIn 0.3s ease-in-out;
        scrollbar-width: thin;
        scrollbar-color: #007bff #f0f0f5;
    }}

    .suggestions::-webkit-scrollbar {{
        width: 8px;
    }}

    .suggestions::-webkit-scrollbar-track {{
        background-color: #f0f0f5;
    }}

    .suggestions::-webkit-scrollbar-thumb {{
        background-color: #007bff;
        border-radius: 4px;
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
        transition: background-color 0.3s, color 0.3s;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}

    @keyframes fadeIn {{
        from {{
            opacity: 0;
            transform: translateY(-10px);
        }}
        to {{
            opacity: 1;
            transform: translateY(0);
        }}
    }}

</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# # JS to Python callback using Streamlit's JS API
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=lambda: handle_input(st.session_state["input"]))

# # JavaScript to listen to messages and trigger the Python function
# st.components.v1.html("""
# <script>
#     window.addEventListener("message", (event) => {
#         if (event.data.type === "input") {
#             const inputValue = event.data.value;
#             window.parent.document.getElementById("input").value = inputValue;
#             window.parent.document.getElementById("input").dispatchEvent(new Event('input', { bubbles: true }));
#         }
#     });
# </script>
# """, height=0, width=0)



Uncaught TypeError: window.parent.handle_input is not a function
    at onEnterPress (VM357 about:srcdoc:33:23)
    at HTMLInputElement.onkeydown (VM361 about:srcdoc:1:28)
