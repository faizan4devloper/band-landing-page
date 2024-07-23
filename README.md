.searchBar {
  margin: 20px 0;
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 95%;
  left: 50%;
  transform: translateX(-50%);
  align-items: center;
}

.inputWrapper {
  position: relative;
  width: 100%;
  max-width: 400px;
}

.searchInput {
  width: 100%;
  padding: 10px 40px 10px 16px; /* Add padding on the right for the icon */
  border: 1px solid #d3d3d3;
  border-radius: 50px;
  font-size: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  outline: none;
  transition: all 0.3s ease;
  position: relative;
}

.searchInput::placeholder {
  color: #888;
  font-size: 12px;
  opacity: 0.7;
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3);
  background-color: #fff;
  transform: scale(1.02);
}

.searchIcon {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  color: #888;
  pointer-events: none; /* Allow clicks to pass through */
}



import React, { useState, useEffect, useRef } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSearch } from '@fortawesome/free-solid-svg-icons';
import styles from './SearchBar.module.css';

const SearchBar = ({ query, onQueryChange }) => {
  const [placeholder, setPlaceholder] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const fullPlaceholder = "Find your perfect solution...";
  const typewriterRef = useRef(null);

  useEffect(() => {
    let currentText = '';
    let index = 0;
    let isTypingStopped = false;

    const typeWriter = () => {
      if (!isTypingStopped && !isTyping) {
        if (index < fullPlaceholder.length) {
          currentText += fullPlaceholder[index];
          setPlaceholder(currentText);
          index++;
          typewriterRef.current = setTimeout(typeWriter, 100);
        } else {
          setTimeout(() => {
            currentText = '';
            setPlaceholder(currentText);
            index = 0;
            typeWriter();
          }, 2000); // Pause before restarting
        }
      }
    };

    typeWriter();

    return () => {
      clearTimeout(typewriterRef.current);
    };
  }, [isTyping]);

  const handleChange = (e) => {
    onQueryChange(e.target.value);
    if (e.target.value.length > 0) {
      setIsTyping(true);
      clearTimeout(typewriterRef.current);
    } else {
      setIsTyping(false);
    }
  };

  return (
    <div className={styles.searchBar}>
      <div className={styles.inputWrapper}>
        <input
          type="text"
          placeholder={isTyping ? '' : placeholder}
          value={query}
          onChange={handleChange}
          className={styles.searchInput}
        />
        <FontAwesomeIcon icon={faSearch} className={styles.searchIcon} />
      </div>
    </div>
  );
};

export default SearchBar;
