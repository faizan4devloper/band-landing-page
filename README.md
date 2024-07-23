import React, { useState, useEffect, useRef } from 'react';
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
      if (!isTypingStopped) {
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
    setIsTyping(true);
    clearTimeout(typewriterRef.current);
  };

  return (
    <div className={styles.searchBar}>
      <input
        type="text"
        placeholder={isTyping ? '' : placeholder}
        value={query}
        onChange={handleChange}
        className={styles.searchInput}
      />
    </div>
  );
};

export default SearchBar;
