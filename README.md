// ThemeContext.js
import React, { createContext, useState } from 'react';

export const ThemeContext = createContext();

const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export default ThemeProvider;



// Header.js
import React from 'react';
import { useContext } from 'react';
import { ThemeContext } from '../../ThemeContext';
import lightThemeStyles from '../../lightTheme.module.css';
import darkThemeStyles from '../../darkTheme.module.css';
import styles from './Header.module.css';

const Header = () => {
  const { theme } = useContext(ThemeContext);
  const themeStyles = theme === 'light' ? lightThemeStyles : darkThemeStyles;

  return (
    <header className={`${styles.header} ${themeStyles.body}`}>
      <h1>My Application</h1>
    </header>
  );
};

export default Header;
