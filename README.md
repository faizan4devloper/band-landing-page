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
}

.searchInput::placeholder {
  color: #888;
  opacity: 0.7; /* Slightly faded placeholder */
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #f0f0f0; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}


/* Additional animations */
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

.searchBar {
  animation: fadeIn 0.5s ease-in-out;
}

.searchInput {
  transition: all 0.3s ease;
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3);
  background-color: #f0f0f0;
  transform: scale(1.02);
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




/* Animation for placeholder text */
@keyframes slideUp {
  0% {
    opacity: 0;
    transform: translateY(10px);
  }
  50% {
    opacity: 0.5;
    transform: translateY(-5px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

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
  background-color: #fff;
  transition: all 0.3s ease; /* Smooth transition */
}

/* Placeholder animation */
.searchInput::placeholder {
  color: #888;
  opacity: 0.7;
  animation: slideUp 1s ease-in-out infinite; /* Apply the animation */
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #f0f0f0; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}

.searchInput:focus::placeholder {
  opacity: 0; /* Hide placeholder on focus */
  animation: none; /* Disable animation on focus */
}
