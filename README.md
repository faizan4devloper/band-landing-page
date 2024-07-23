import React, { useEffect, useState } from 'react';
import styles from './SearchBar.module.css';

const SearchBar = ({ query, onQueryChange }) => {
  const [placeholderText, setPlaceholderText] = useState("");

  useEffect(() => {
    const fullPlaceholder = "Search cards...";
    let index = 0;

    const typeWriter = () => {
      if (index < fullPlaceholder.length) {
        setPlaceholderText(fullPlaceholder.slice(0, index + 1));
        index++;
        setTimeout(typeWriter, 100); // Adjust typing speed
      }
    };

    typeWriter(); // Start typing animation

    return () => {
      setPlaceholderText(""); // Clear text on unmount
    };
  }, []);

  return (
    <div className={styles.searchBar}>
      <input
        type="text"
        placeholder={placeholderText}
        value={query}
        onChange={(e) => onQueryChange(e.target.value)}
        className={styles.searchInput}
      />
    </div>
  );
};

export default SearchBar;



.searchBar {
  margin: 20px 0;
  display: flex;
  justify-content: center;
}

.searchInput {
  width: 100%;
  max-width: 400px;
  padding: 12px 16px;
  border: 1px solid #d3d3d3;
  border-radius: 50px; /* Rounded corners */
  font-size: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  outline: none;
  transition: all 0.3s ease; /* Smooth transition */
  position: relative;
  overflow: hidden; /* Ensure no overflow of typewriter effect */
}

/* Typewriter effect container */
.searchInput-container {
  position: relative;
}

/* Placeholder styling */
.searchInput::placeholder {
  color: #888;
  opacity: 0; /* Initially hidden */
  position: absolute;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  white-space: nowrap; /* Prevent text wrapping */
  overflow: hidden; /* Hide text that overflows */
  animation: fadeIn 1s ease-out, typing 4s steps(30, end) 1 normal both; /* Apply typewriter animation */
}

/* Placeholder animation: fade in */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 0.7;
  }
}

/* Typewriter effect */
@keyframes typing {
  from {
    width: 0; /* Start with zero width */
  }
  to {
    width: 100%; /* Expand to full width */
  }
}

.searchInput:focus::placeholder {
  opacity: 0; /* Hide placeholder text on focus */
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #fff; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}
