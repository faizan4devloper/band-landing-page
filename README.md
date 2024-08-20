st.experimental_rerunfunctionstreamlit.commands.execution_control.experimental_rerun() -> 'NoReturn'
Rerun the script immediately.

When ``st.experimental_rerun()`` is called, the script is halted - no
more statements will be run, and the script will be queued to re-run
from the top.



import streamlit as st
import uuid
import requests

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https://your-lambda-url"

def handle_input(user_input):
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")

# JavaScript to HTML interaction code
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
                sendInputToStreamlit(suggestion);
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
        sendInputToStreamlit(inputValue);
    }}

    function sendInputToStreamlit(value) {{
        fetch('/_st_input', {{
            method: 'POST',
            headers: {{
                'Content-Type': 'application/json',
            }},
            body: JSON.stringify({{ 'input_value': value }}),
        }}).then(response => {{
            if (!response.ok) {{
                throw new Error('Failed to send input to Streamlit');
            }}
            return response.json();
        }}).then(data => {{
            console.log('Response from Streamlit:', data);
            document.getElementById('autocomplete').value = '';  // Clear the input field after submission
        }}).catch(error => {{
            console.error('Error:', error);
        }});
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
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}
</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# Create an endpoint to receive the input value
st.experimental_rerun
st.experimental_get_query_params()
input_value = st.experimental_get_query_params().get('input_value', None)

if input_value:
    handle_input(input_value[0])











import streamlit as st
import uuid
import requests

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https://your-lambda-url"

def handle_input(user_input):
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")

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
                sendInputToStreamlit(suggestion);
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
        sendInputToStreamlit(inputValue);
    }}

    function sendInputToStreamlit(value) {{
        window.parent.postMessage({{ 'type': 'input', 'value': value }}, '*');
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
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}
</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# JS to Python callback using Streamlit's JS API
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=lambda: handle_input(st.session_state["input"]))

# JavaScript to listen to messages and trigger the Python function
st.components.v1.html("""
<script>
    window.addEventListener("message", (event) => {
        if (event.data.type === "input") {
            const inputValue = event.data.value;
            window.parent.document.getElementById("input").value = inputValue;
            window.parent.document.getElementById("input").dispatchEvent(new Event('input', { bubbles: true }));
        }
    });
</script>
""", height=0, width=0)








import streamlit as st
import uuid
import requests

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

if "input_data" not in st.session_state:
    st.session_state.input_data = []

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https://your-lambda-url"

def handle_input(user_input):
    # Capture and store the input data
    st.session_state.input_data.append(user_input)
    
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")

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
                sendInputToStreamlit(suggestion);
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
        sendInputToStreamlit(inputValue);
    }}

    function sendInputToStreamlit(value) {{
        // Use the Streamlit API to trigger the Python function directly
        fetch('/_stcore/run?callback=handle_input&args=' + encodeURIComponent(JSON.stringify(value)))
            .then(response => response.json())
            .then(data => {{
                // You can handle the response here if needed
            }})
            .catch(error => {{
                console.error('Error:', error);
            }});

        // Clear the input field
        document.getElementById('autocomplete').value = '';
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
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}
</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# Register the callback function to handle input
def callback_function(value):
    handle_input(value)

st.session_state['callback'] = callback_function

# Display captured input data (for debugging or logging purposes)
st.write("Captured Input Data:", st.session_state.input_data)






import streamlit as st
import uuid
import requests

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https://your-lambda-url"

def handle_input(user_input):
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")

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
                sendInputToStreamlit(suggestion);
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
        sendInputToStreamlit(inputValue);
    }}

    function sendInputToStreamlit(value) {{
        // Use the Streamlit API to trigger the Python function directly
        fetch('/_stcore/run?callback=handle_input&args=' + encodeURIComponent(JSON.stringify(value)))
            .then(response => response.json())
            .then(data => {{
                // You can handle the response here if needed
            }})
            .catch(error => {{
                console.error('Error:', error);
            }});

        // Clear the input field
        document.getElementById('autocomplete').value = '';
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
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}
</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# Register the callback function to handle input
def callback_function(value):
    handle_input(value)

st.session_state['callback'] = callback_function





import streamlit as st
import uuid
import requests

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

if "input" not in st.session_state:
    st.session_state.input = ""

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https://your-lambda-url"

def handle_input():
    user_input = st.session_state["input"]
    
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")

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
                sendInputToStreamlit(suggestion);
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
        sendInputToStreamlit(inputValue);
    }}

    function sendInputToStreamlit(value) {{
        // Send the value to Streamlit
        window.parent.document.getElementById('input').value = value;
        window.parent.document.getElementById('input').dispatchEvent(new Event('input', {{ bubbles: true }}));

        // Clear the input field
        document.getElementById('autocomplete').value = '';
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
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}
</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# Hidden input to capture the value and trigger Lambda request
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=handle_input)




import streamlit as st
import uuid
import requests

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

if "input" not in st.session_state:
    st.session_state.input = ""

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https://your-lambda-url"

def handle_input():
    user_input = st.session_state["input"]
    
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")

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
                window.parent.document.getElementById('input').value = suggestion;
                window.parent.document.getElementById('input').dispatchEvent(new Event('input', {{ bubbles: true }}));
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
        window.parent.document.getElementById('input').value = inputField.value;
        window.parent.document.getElementById('input').dispatchEvent(new Event('input', {{ bubbles: true }}));
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
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}
</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# Hidden input to capture the value and trigger Lambda request
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=handle_input)





import streamlit as st
import uuid
import requests

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

if "input" not in st.session_state:
    st.session_state.input = ""

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https://your-lambda-url"

def handle_input():
    user_input = st.session_state["input"]
    
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")

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
                window.parent.document.getElementById('input').value = suggestion;
                window.parent.document.getElementById('input').dispatchEvent(new Event('input', {{ bubbles: true }}));
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
        window.parent.document.getElementById('input').value = inputField.value;
        window.parent.document.getElementById('input').dispatchEvent(new Event('input', {{ bubbles: true }}));
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
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}
</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# Hidden input to capture the value and trigger Lambda request
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=handle_input)

# Display the selected input value
st.write("You entered:", st.session_state.get("input", ""))







def write_top_bar():
    col1, col2, col3 = st.columns([2, 10, 3])
    with col3:
        clear = st.button("Clear Chat", key="clear_chat_button")
    return clear





DuplicateWidgetID: There are multiple identical st.button widgets with the same generated key.

When a widget is created, it's assigned an internal key based on its structure. Multiple widgets with an identical structure will result in the same internal key, which causes this error.

To fix this error, please pass a unique key argument to st.button.

Traceback:
File "/home/ec2-user/ServiceSphere_BP_onlychatbot/workdir/app.py", line 128, in <module>
    clear = write_top_bar()
            ^^^^^^^^^^^^^^^
File "/home/ec2-user/ServiceSphere_BP_onlychatbot/workdir/app.py", line 125, in write_top_bar
    clear = st.button("Clear Chat")
            ^^^^^^^^^^^^^^^^^^^^^^^


import streamlit as st
import uuid
import requests
import warnings
warnings.filterwarnings('ignore')

# Define the title and header
st.header("❓ KB Assist Only ChatBot 🔎", divider='blue')
text_template_write = "<span style='color:{color}; font-size:{font_size}; font-style:{font_style};'>{content}</span>"
text = text_template_write.format(color="yellow", font_size="17px", font_style="bold", content="[ The Smart Way to Get Answers ]")
st.write(text, unsafe_allow_html=True)

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

if "questions" not in st.session_state:
    st.session_state.questions = []

if "answers" not in st.session_state:
    st.session_state.answers = []

if "input" not in st.session_state:
    st.session_state.input = ""

# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Lambda Function URL
LAMBDA_URL = "https://your-lambda-url.amazonaws.com/"  # Replace with your actual Lambda URL

# Function to handle user input and call AWS Lambda
def handle_input():
    user_input = st.session_state["input"]
    
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.session_state.questions.append({"question": user_input, "id": len(st.session_state.questions)})
        st.session_state.answers.append({"answer": response.json().get("answer", "No response received"), "id": len(st.session_state.answers)})
    else:
        st.error("Failed to connect to Lambda.")

# Clear chat function
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

# Display the chat history
with st.container():
    for q, a in zip(st.session_state.questions, st.session_state.answers):
        st.write(f"**User**: {q['question']}")
        st.write(f"**Bot**: {a['answer']}")

st.markdown("---")

# Autocomplete HTML and JavaScript
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
                window.parent.document.getElementById('input').value = suggestion;
                window.parent.document.getElementById('input').dispatchEvent(new Event('input', {{ bubbles: true }}));
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
        window.parent.document.getElementById('input').value = inputField.value;
        window.parent.document.getElementById('input').dispatchEvent(new Event('input', {{ bubbles: true }}));
    }}
</script>

<style>
    body {{
        font-family: "Arial", sans-serif;
        background-color: #f0f0f5;
    }}

    .search-container {{
        position: relative;
        width: 400px;
        max-width: 90%;
        margin: 0 auto;
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
        scrollbar-width: thin;
        scrollbar-color: #007bff #f0f0f5;
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
</style>
"""

# Display the autocomplete HTML
st.components.v1.html(autocomplete_html, height=300)

# Hidden input to capture the value
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=handle_input)

# Display the selected input value
st.write("You entered:", st.session_state.get("input", ""))






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

# Lambda Function URL
LAMBDA_URL = "dummy"

def handle_input():
    user_input = st.session_state["input"]
    
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
    else:
        st.error("Failed to connect to Lambda.")


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

# Create the HTML and JavaScript code
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
                var hiddenField = window.parent.document.getElementById('hidden_input');
                hiddenField.value = suggestion;
                hiddenField.dispatchEvent(new Event('input', {{ bubbles: true }}));
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
        var selectedSuggestion = document.querySelector('.suggestions li.selected');
        if (selectedSuggestion) {{
            selectedSuggestion.click();
        }}
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

# Display the autocomplete HTML
st.components.v1.html(autocomplete_html, height=300)

# Hidden input to capture the value
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=handle_input)

# Display the selected input value
st.write("You entered:", st.session_state.get("input", ""))
