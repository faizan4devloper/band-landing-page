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
    <input type="text" id="autocomplete" placeholder="You are talking to an AI, ask any question..." onkeyup="filterSuggestions()" onkeydown="if (event.key === 'Enter') onEnterPress()">
    <ul id="suggestions" class="suggestions">
        {''.join([f'<li onclick="selectSuggestion(\'{suggestion}\')">{suggestion}</li>' for suggestion in suggestions])}
    </ul>
</div>

<script>
    function filterSuggestions() {{
        var input = document.getElementById('autocomplete').value.toLowerCase();
        var suggestionItems = document.getElementById('suggestions').getElementsByTagName('li');
        for (var i = 0; i < suggestionItems.length; i++) {{
            var item = suggestionItems[i];
            if (item.textContent.toLowerCase().includes(input)) {{
                item.style.display = "";
            }} else {{
                item.style.display = "none";
            }}
        }}
    }}

    function selectSuggestion(value) {{
        document.getElementById('autocomplete').value = value;
        document.getElementById('suggestions').style.display = 'none';
        onEnterPress();
    }}

    function onEnterPress() {{
        var input = document.getElementById('autocomplete').value;
        var hiddenField = window.parent.document.getElementById('hidden_input');
        hiddenField.value = input;
        hiddenField.dispatchEvent(new Event('input', {{ bubbles: true }}));
    }}

    // Show suggestions when typing
    document.getElementById('autocomplete').addEventListener('focus', function() {{
        document.getElementById('suggestions').style.display = 'block';
    }});

    // Hide suggestions when clicking outside
    document.addEventListener('click', function(e) {{
        if (!document.querySelector('.search-container').contains(e.target)) {{
            document.getElementById('suggestions').style.display = 'none';
        }}
    }});
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
        background-color: #fff;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        display: none;
        position: absolute;
        width: 100%;
        z-index: 10;
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

# Display the autocomplete HTML and JavaScript in Streamlit
st.components.v1.html(autocomplete_html, height=400)

# Hidden input to capture the value from the JavaScript code
hidden_input = st.text_input("Hidden Input", key="hidden_input")

# Display the selected input value
st.write("You entered:", hidden_input)
