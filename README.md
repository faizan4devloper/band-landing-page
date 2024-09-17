import React from 'react';
import styles from './Header.module.css';
import HeaderLogo from '../../assets/icons/HCLTechLogoBlue.svg';

const Header = () => {
    return (
        <header className={styles.header}>
            <div className={styles.logo}>
            <img src={HeaderLogo} alt="Logo" />
            </div>
        </header>
    );
};

export default Header;

.header {
    /*background-color: #1a1a2e;*/
    color: white;
    padding: 15px;
    border-bottom: 0.1px solid grey;
    text-align: center;
    
    
}

.logo img {
  max-height: 20px;
  cursor: pointer;
}
