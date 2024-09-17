import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import Breadcrumbs from './components/Breadcrumbs/Breadcrumbs';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import Home from './components/Pages/Home'; // Create a HomeScreen component
import UploadDocuments from './components/Pages/UploadDocuments';

function App() {
    return (
        <Router>
            <div className="App">
                <Header />
                <Breadcrumbs />
                <Routes>
                    <Route path="/" element={<WelcomeScreen />} />
                    <Route path="/home" element={<Home />} />
                    <Route path="/upload-documents" element={<UploadDocuments/>}/>
                </Routes>
            </div>
        </Router>
    );
}

export default App;


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
        home: 'Home',
        'upload-documents': 'Upload Documents',
    };

    return (
        <nav className={styles.breadcrumbs} aria-label="Breadcrumb">
            <Link to="/" className={styles.crumb}>
                <FontAwesomeIcon icon={faHome} className={styles.icon} />
            </Link>
            {pathnames.map((value, index) => {
                const to = `/${pathnames.slice(0, index + 1).join('/')}`;
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
