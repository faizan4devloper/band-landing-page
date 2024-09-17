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
