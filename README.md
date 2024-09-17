import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faHome, faChevronRight } from '@fortawesome/free-solid-svg-icons';
import styles from './Breadcrumbs.module.css';

const Breadcrumbs = () => {
    const location = useLocation();
    const pathnames = location.pathname.split('/').filter((x) => x);

    // Define a mapping for path segments to breadcrumb names
    const breadcrumbNameMap = {
        '': 'Home', // Ensure root path maps to Home
        home: 'Home',
        'upload-documents': 'Upload Documents',
    };

    return (
        <nav className={styles.breadcrumbs} aria-label="Breadcrumb">
            <Link to="/" className={styles.crumb}>
                <FontAwesomeIcon icon={faHome} className={styles.icon} />
                Home
            </Link>
            {pathnames.map((value, index) => {
                const to = `/${pathnames.slice(0, index + 1).join('/')}`; // Fixed syntax here
                const isLast = index === pathnames.length - 1;
                const name = breadcrumbNameMap[value] || value.charAt(0).toUpperCase() + value.slice(1);

                return (
                    <span key={to} className={styles.crumb}>
                        <FontAwesomeIcon icon={faChevronRight} className={styles.separator} />
                        {isLast ? (
                            <span className={`${styles.link} ${styles.current}`} aria-current="page">
                                {name}
                            </span>
                        ) : (
                            <Link to={to} className={styles.link}>
                                {name}
                            </Link>
                        )}
                    </span>
                );
            })}
        </nav>
    );
};

export default Breadcrumbs;
