import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faHome, faUpload, faCheckCircle } from '@fortawesome/free-solid-svg-icons';
import './Breadcrumbs.css'; // Create custom CSS for breadcrumbs

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
        <nav className="breadcrumbs">
            <ul>
                <li>
                    <Link to="/" className="breadcrumb-item">
                        <FontAwesomeIcon icon={faHome} />
                        <span>Home</span>
                    </Link>
                </li>
                {paths.map((path, index) => {
                    const fullPath = `/${paths.slice(0, index + 1).join('/')}`;
                    const breadcrumbData = getBreadcrumbData(fullPath);
                    return (
                        <li key={index}>
                            <Link to={fullPath} className="breadcrumb-item">
                                <FontAwesomeIcon icon={breadcrumbData.icon || faCheckCircle} />
                                <span>{breadcrumbData.label}</span>
                            </Link>
                        </li>
                    );
                })}
            </ul>
        </nav>
    );
};

export default Breadcrumbs;


.breadcrumbs {
    display: flex;
    align-items: center;
    padding: 10px 20px;
    background-color: #f4f4f4;
    border-radius: 8px;
    margin: 15px 0;
}

.breadcrumbs ul {
    list-style: none;
    display: flex;
    gap: 10px;
}

.breadcrumb-item {
    display: flex;
    align-items: center;
    color: #007bff;
    text-decoration: none;
    font-size: 1rem;
    transition: color 0.3s ease;
}

.breadcrumb-item:hover {
    color: #0056b3;
}

.breadcrumb-item svg {
    margin-right: 5px;
}

.breadcrumbs ul li::after {
    content: "/";
    margin-left: 10px;
    color: #ccc;
}

.breadcrumbs ul li:last-child::after {
    content: "";
}


import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import Home from './components/Pages/Home/Home';
import UploadDocuments from './components/Pages/UploadDocuments';
import Breadcrumbs from './components/Breadcrumbs/Breadcrumbs'; // Importing the Breadcrumbs component

function App() {
    return (
        <Router>
            <div className="App">
                <Header />
                {/* Breadcrumbs component will dynamically display based on current route */}
                <Breadcrumbs />
                <Routes>
                    <Route path="/" element={<WelcomeScreen />} />
                    <Route path="/home" element={<Home />} />
                    <Route path="/upload-documents" element={<UploadDocuments />} />
                </Routes>
            </div>
        </Router>
    );
}

export default App;
