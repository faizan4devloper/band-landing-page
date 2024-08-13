<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Search Bar with Suggestions</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="search-container">
      <input
        type="text"
        id="search-input"
        placeholder="Search..."
        autocomplete="off"
      />
      <ul id="suggestions-list" class="suggestions"></ul>
    </div>

    <script src="index.js"></script>
  </body>
</html>





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

const searchInput = document.getElementById("search-input");
const suggestionsList = document.getElementById("suggestions-list");

const suggestions = [
  "Olympic 2024",
  "Olympic news",
  "Olympic schedule",
  "Olympic sports",
  "Olympic history",
];

searchInput.addEventListener("input", () => {
  const query = searchInput.value.toLowerCase();
  suggestionsList.innerHTML = "";
  if (query) {
    const filteredSuggestions = suggestions.filter((suggestion) =>
      suggestion.toLowerCase().includes(query)
    );
    if (filteredSuggestions.length) {
      filteredSuggestions.forEach((suggestion) => {
        const listItem = document.createElement("li");
        listItem.textContent = suggestion;
        listItem.addEventListener("click", () => {
          searchInput.value = suggestion;
          suggestionsList.innerHTML = "";
          suggestionsList.style.display = "none";
        });
        suggestionsList.appendChild(listItem);
      });
      suggestionsList.style.display = "block";
    } else {
      suggestionsList.style.display = "none";
    }
  } else {
    suggestionsList.style.display = "none";
  }
});
