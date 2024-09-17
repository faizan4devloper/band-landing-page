import React, { useState, useEffect } from 'react';
import styles from './Header.module.css';
import HeaderLogo from '../../assets/icons/HCLTechLogoBlue.svg';

const Header = () => {
    const [isVisible, setIsVisible] = useState(true);
    const [lastScrollY, setLastScrollY] = useState(0);

    useEffect(() => {
        const handleScroll = () => {
            const currentScrollY = window.scrollY;

            if (currentScrollY > lastScrollY && currentScrollY > 100) {
                // Scroll down
                setIsVisible(false);
            } else {
                // Scroll up
                setIsVisible(true);
            }

            setLastScrollY(currentScrollY);
        };

        window.addEventListener('scroll', handleScroll);

        return () => {
            window.removeEventListener('scroll', handleScroll);
        };
    }, [lastScrollY]);

    return (
        <header className={`${styles.header} ${isVisible ? styles.visible : styles.hidden}`}>
            <div className={styles.logo}>
                <img src={HeaderLogo} alt="Logo" />
            </div>
        </header>
    );
};

export default Header;
