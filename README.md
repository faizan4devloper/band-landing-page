import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faHome } from '@fortawesome/free-solid-svg-icons';
import styles from './Breadcrumbs.module.css'; // Import CSS module

const Breadcrumbs = () => {
    const location = useLocation();
    const paths = location.pathname.split('/').filter(Boolean);

    return (
        <nav className={styles.breadcrumbs}>
            <ul className={styles.breadcrumbsList}>
                {/* Home link */}
                <li className={styles.breadcrumbItem}>
                    <Link to="/" className={styles.link}><FontAwesomeIcon icon={faHome} /></Link>
                </li>
                
                {/* Check if path contains 'upload-documents' and manually add 'Home' */}
                {paths.includes('upload-documents') && (
                    <>
                        <span className={styles.separator}>/</span>
                        <li className={styles.breadcrumbItem}>
                            <Link to="/home" className={styles.link}>Home</Link>
                        </li>
                    </>
                )}

                {/* Map the remaining paths dynamically */}
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
