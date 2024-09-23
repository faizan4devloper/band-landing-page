import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import styles from './Breadcrumbs.module.css'; // Import CSS module

const Breadcrumbs = () => {
    const location = useLocation();
    const paths = location.pathname.split('/').filter(Boolean);

    return (
        <nav className={styles.breadcrumbs}>
            <ul className={styles.breadcrumbsList}>
                {/* Home link */}
                <li className={styles.breadcrumbItem}>
                    <Link to="/" className={styles.link}>Home</Link>
                </li>
                {paths.map((path, index) => {
                    const fullPath = `/${paths.slice(0, index + 1).join('/')}`;
                    return (
                        <React.Fragment key={index}>
                            <span className={styles.separator}>/</span>
                            <li className={styles.breadcrumbItem}>
                                <Link to={fullPath} className={styles.link}>
                                    {path.replace(/-/g, ' ')}
                                </Link>
                            </li>
                        </React.Fragment>
                    );
                })}
            </ul>
        </nav>
    );
};

export default Breadcrumbs;


/* Breadcrumbs.module.css */
.breadcrumbs {
    display: flex;
    justify-content: center;
    padding: 10px;
    background-color: transparent;
}

.breadcrumbsList {
    list-style: none;
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 0;
    margin: 0;
    font-size: 1.25rem; /* Adjust size to match the example */
    font-family: Arial, sans-serif;
    color: #7d71f7; /* Light purple color */
}

.breadcrumbItem {
    display: flex;
    align-items: center;
}

.link {
    text-decoration: none;
    color: #7d71f7; /* Match the purple color */
    font-weight: 400;
    transition: color 0.3s ease;
}

.link:hover {
    color: #5e4dc4; /* Slightly darker on hover */
}

.separator {
    margin: 0 10px;
    color: #d4c6f4; /* Light separator color */
    font-weight: 300;
}

.breadcrumbItem:last-child .link {
    font-weight: 700; /* Bold for the last item */
}
