import React from 'react';
import styles from './Header.module.css';
import logo from '../../assets/logo.png'; // Ensure this path points to your logo

const Header = () => {
  return (
    <header className={styles.header}>
      <div className={styles.logoContainer}>
        <img src={logo} alt="Logo" className={styles.logo} />
        <h1 className={styles.logoText}>MyApp</h1>
      </div>
      <nav className={styles.navbar}>
        <ul className={styles.navLinks}>
          <li className={styles.navItem}><a href="#home">Home</a></li>
          <li className={styles.navItem}><a href="#features">Features</a></li>
          <li className={styles.navItem}><a href="#about">About</a></li>
          <li className={styles.navItem}><a href="#contact">Contact</a></li>
        </ul>
      </nav>
    </header>
  );
};

export default Header;



/* Header.module.css */

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background-color: #4f46e5; /* Dark purple/navy background */
  color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.logoContainer {
  display: flex;
  align-items: center;
}

.logo {
  width: 50px;
  height: 50px;
  margin-right: 0.75rem;
}

.logoText {
  font-size: 1.5rem;
  font-weight: bold;
  letter-spacing: 1px;
}

.navbar {
  display: flex;
}

.navLinks {
  display: flex;
  list-style: none;
  gap: 1.5rem;
}

.navItem a {
  text-decoration: none;
  color: white;
  font-weight: 500;
  transition: color 0.3s ease;
}

.navItem a:hover {
  color: #e0e7ff; /* Light shade on hover */
}



import './App.css';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';

function App() {
  return (
    <div className="App">
      <Header />
      <LoginPage />
    </div>
  );
}

export default App;
