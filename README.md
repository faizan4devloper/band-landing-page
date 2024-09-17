import React from 'react';
import styles from './Breadcrumbs.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faHome, faChevronRight } from '@fortawesome/free-solid-svg-icons'; // Import desired icons

const Breadcrumbs = ({ path }) => {
    return (
        <nav className={styles.breadcrumbs} aria-label="Breadcrumb">
            {path.map((crumb, index) => (
                <span key={index} className={styles.crumb}>
                    {/* Add Home Icon for the first breadcrumb */}
                    {index === 0 && (
                        <FontAwesomeIcon icon={faHome} className={styles.icon} />
                    )}
                    <span
                        className={`${styles.link} ${
                            index === path.length - 1 ? styles.current : ''
                        }`}
                        aria-current={index === path.length - 1 ? 'page' : undefined}
                    >
                        {crumb}
                    </span>
                    {/* Add Chevron Icon as a separator */}
                    {index < path.length - 1 && (
                        <FontAwesomeIcon icon={faChevronRight} className={styles.separator} />
                    )}
                </span>
            ))}
        </nav>
    );
};

export default Breadcrumbs;




.breadcrumbs {
    margin: 1rem;
    font-size: 1rem;
    display: flex;
    align-items: center;
    color: #333;
}

.crumb {
    display: flex;
    align-items: center;
}

.icon {
    margin-right: 0.5rem;
    color: #555;
}

.link {
    color: #007bff; /* Link color */
    text-decoration: none;
    transition: color 0.3s ease;
}

.link:hover {
    color: #0056b3; /* Darker shade on hover */
    text-decoration: underline;
}

.current {
    font-weight: bold;
    color: #555; /* Current breadcrumb color */
}

.separator {
    margin: 0 0.5rem;
    color: #999;
    font-size: 0.8rem; /* Slightly smaller size for separators */
}
