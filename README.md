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

/* Placeholder styling with typewriter effect */
.searchInput::placeholder {
  color: #888;
  opacity: 0.7; /* Slightly faded placeholder */
  white-space: nowrap; /* Prevent text wrapping */
  overflow: hidden; /* Hide text that overflows */
  display: inline-block;
  width: 100%; /* Ensure placeholder takes full width */
  animation: typing 4s steps(30, end) 1 normal both, blink-caret 0.75s step-end infinite; /* Apply typewriter animation */
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

/* Caret blinking animation */
@keyframes blink-caret {
  from, to {
    border-color: transparent; /* Invisible caret */
  }
  50% {
    border-color: rgba(95, 30, 193, 1); /* Visible caret */
  }
}

.searchInput:focus::placeholder {
  opacity: 0; /* Hide placeholder text on focus */
  animation: none; /* Stop animation when focused */
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #fff; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}





import React from 'react';
import styles from './SearchBar.module.css';

const SearchBar = ({ query, onQueryChange }) => {
  return (
    <div className={styles.searchBar}>
      <input
        type="text"
        placeholder="Search cards..."
        value={query}
        onChange={(e) => onQueryChange(e.target.value)}
        className={styles.searchInput}
      />
    </div>
  );
};

export default SearchBar;
