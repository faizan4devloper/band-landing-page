/* Breadcrumbs.module.css */
.breadcrumbs {
    display: flex;
    align-items: center;
    padding: 10px 20px;
    background-color: #f4f4f4;
    border-radius: 8px;
    margin: 15px 0;
}

.breadcrumbsList {
    list-style: none;
    display: flex;
    gap: 10px;
    padding: 0;
    margin: 0;
}

.breadcrumbItem {
    display: flex;
    align-items: center;
    color: #007bff;
    text-decoration: none;
    font-size: 1rem;
    transition: color 0.3s ease;
}

.breadcrumbItem:hover {
    color: #0056b3;
}

.breadcrumbIcon {
    margin-right: 5px;
}

.separator {
    content: "/";
    margin-left: 10px;
    color: #ccc;
}

.lastItemSeparator {
    content: "";
}




import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faHome, faUpload, faCheckCircle } from '@fortawesome/free-solid-svg-icons';
import styles from './Breadcrumbs.module.css'; // Import CSS module

const Breadcrumbs = () => {
    const location = useLocation();
    const paths = location.pathname.split('/').filter((path) => path);

    const breadcrumbs = [
        { path: '/', label: 'Home', icon: faHome },
        { path: '/home', label: 'Dashboard', icon: faHome },
        { path: '/upload-documents', label: 'Upload Documents', icon: faUpload }
    ];

    const getBreadcrumbData = (path) => breadcrumbs.find((breadcrumb) => breadcrumb.path === path) || {};

    return (
        <nav className={styles.breadcrumbs}>
            <ul className={styles.breadcrumbsList}>
                <li>
                    <Link to="/" className={styles.breadcrumbItem}>
                        <FontAwesomeIcon icon={faHome} className={styles.breadcrumbIcon} />
                        <span>Home</span>
                    </Link>
                </li>
                {paths.map((path, index) => {
                    const fullPath = `/${paths.slice(0, index + 1).join('/')}`;
                    const breadcrumbData = getBreadcrumbData(fullPath);
                    return (
                        <li key={index}>
                            <Link to={fullPath} className={styles.breadcrumbItem}>
                                <FontAwesomeIcon icon={breadcrumbData.icon || faCheckCircle} className={styles.breadcrumbIcon} />
                                <span>{breadcrumbData.label}</span>
                            </Link>
                            {index < paths.length - 1 && <span className={styles.separator}>/</span>}
                        </li>
                    );
                })}
            </ul>
        </nav>
    );
};

export default Breadcrumbs;
