import React from 'react';
import styles from './Header.module.css';

const Header = () => {
    return (
        <header className={styles.header}>
            <h1 className={styles.title}>My Web App</h1>
        </header>
    );
};

export default Header;

.header {
    background-color: #333;
    color: white;
    padding: 1rem;
    text-align: center;
}

.title {
    font-size: 2rem;
}

import React from 'react';
import styles from './Breadcrumbs.module.css';

const Breadcrumbs = ({ path }) => {
    return (
        <nav className={styles.breadcrumbs}>
            {path.map((crumb, index) => (
                <span key={index} className={styles.crumb}>
                    {crumb}
                    {index < path.length - 1 && ' > '}
                </span>
            ))}
        </nav>
    );
};

export default Breadcrumbs;


.breadcrumbs {
    margin: 1rem;
    font-size: 1rem;
}

.crumb {
    color: #555;
}

import React from 'react';
import styles from './WelcomeScreen.module.css';
import Breadcrumbs from '../Breadcrumbs/Breadcrumbs';

const WelcomeScreen = () => {
    return (
        <div className={styles.welcomeScreen}>
            <Breadcrumbs path={['Home', 'Welcome']} />
            <h2>Welcome to My Web App</h2>
            <p>This is the welcome screen.</p>
        </div>
    );
};

export default WelcomeScreen;

.welcomeScreen {
    padding: 2rem;
    text-align: center;
}

import React from 'react';
import Header from './components/Header/Header';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import './App.css';

function App() {
    return (
        <div className="App">
            <Header />
            <WelcomeScreen />
        </div>
    );
}

export default App;

.App {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

