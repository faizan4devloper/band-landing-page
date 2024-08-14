import streamlit as st

# Example suggestions list
suggestions = [
    "What is AI?",
    "How does machine learning work?",
    "Tell me about React components",
    "Explain cloud computing",
    "What is AWS?"
]

# Create the HTML and JavaScript code
autocomplete_html = f"""
<div class="search-container">
    <input type="text" id="autocomplete" list="suggestions" placeholder="You are talking to an AI, ask any question..." onkeydown="if (event.key === 'Enter') onEnterPress()">
    <datalist id="suggestions">
        {''.join([f'<option value="{suggestion}"></option>' for suggestion in suggestions])}
    </datalist>
</div>

<script>
    function onEnterPress() {{
        var input = document.getElementById('autocomplete').value;
        var hiddenField = window.parent.document.getElementById('hidden_input');
        hiddenField.value = input;
        hiddenField.dispatchEvent(new Event('input', {{ bubbles: true }}));
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

# Display the autocomplete HTML and JavaScript in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# Hidden input to capture the value from the JavaScript code
hidden_input = st.text_input("Hidden Input", key="hidden_input")

# Display the selected input value
st.write("You entered:", hidden_input)




# Create the HTML and JavaScript for autocomplete functionality
autocomplete_html = f"""
<input type="text" id="autocomplete" list="suggestions" placeholder="You are talking to an AI, ask any question..." style="width: 100%; padding: 10px; font-size: 16px;" onkeydown="if (event.key === 'Enter') onEnterPress()">
<datalist id="suggestions">
    {''.join([f'<option value="{suggestion}"></option>' for suggestion in suggestions])}
</datalist>

<script>
    function onEnterPress() {{
        var input = document.getElementById('autocomplete').value;
        var hiddenField = window.parent.document.getElementById('hidden_input');
        hiddenField.value = input;
        hiddenField.dispatchEvent(new Event('input', {{ bubbles: true }}));
    }}
</script>
"""


body {
  font-family: "Arial", sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #f0f0f5;
}

.search-container {
  position: relative;
  width: 400px;
  max-width: 90%;
}

#search-input {
  width: 100%;
  padding: 12px 16px;
  font-size: 18px;
  border: 2px solid #ddd;
  border-radius: 30px;
  outline: none;
  transition: border-color 0.3s, box-shadow 0.3s;
}

#search-input:focus {
  border-color: #007bff;
  box-shadow: 0 0 10px rgba(0, 123, 255, 0.2);
}

.suggestions {
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
}

.suggestions li {
  padding: 12px 16px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s, color 0.3s;
}

.suggestions li:hover {
  background-color: #007bff;
  color: #fff;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
