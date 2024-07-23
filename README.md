import React from 'react';
import styles from './SearchBar.module.css';

const SearchBar = ({ query, onQueryChange }) => {
  return (
    <div className={styles.searchBar}>
      <input
        type="text"
        placeholder="Find your perfect solution..."
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
  position: absolute;
  bottom: 95%;
  left:69%;
}

.searchInput {
  width: 100%;
  max-width: 400px;
  padding: 8px 16px;
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
  font-size: 12px;
  opacity: 0.7; /* Slightly faded placeholder */
  white-space: nowrap; /* Prevent text wrapping */
  overflow: hidden; /* Hide text that overflows */
  display: inline-block;
  width: 100%; /* Ensure placeholder takes full width */
  animation: typing 4s steps(30, end) 1 normal both, blink-caret 0.75s step-end infinite; /* Apply typewriter animation */
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #fff; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}

