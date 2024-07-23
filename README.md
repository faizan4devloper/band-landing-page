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



@keyframes typing {
  from { width: 0; }
  to { width: 100%; }
}

@keyframes blink-caret {
  from, to { border-color: transparent; }
  50% { border-color: #888; }
}

.searchBar {
  margin: 20px 0;
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 95%;
  left: 69%;
}

.searchInput {
  width: 100%;
  max-width: 400px;
  padding: 8px 16px;
  border: 1px solid #d3d3d3;
  border-radius: 50px;
  font-size: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  outline: none;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.searchInput::placeholder {
  color: #888;
  font-size: 12px;
  opacity: 0.7;
  white-space: nowrap;
  overflow: hidden;
  display: inline-block;
  width: 100%;
  animation: typing 4s steps(30, end) 1 normal both, blink-caret 0.75s step-end infinite;
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3);
  background-color: #fff;
  transform: scale(1.02);
}
