import React, { useState, useEffect } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import Home from './components/Pages/Home/Home';
import UploadDocuments from './components/Pages/UploadDocuments';
import Breadcrumbs from './components/Breadcrumbs/Breadcrumbs';
import Loader from './components/Loader/Loader'; // Importing the Loader component

function App() {
    const [loading, setLoading] = useState(true); // State to manage loading

    useEffect(() => {
        // Simulate a delay for loader (e.g., initial data fetching or app loading time)
        const timer = setTimeout(() => {
            setLoading(false);
        }, 2000); // Adjust the duration as needed (2 seconds here)

        return () => clearTimeout(timer); // Cleanup timer
    }, []);

    if (loading) {
        return <Loader loading={loading} />; // Display loader on initial app load
    }

    return (
        <Router>
            <div className="App">
                <Header />
                <Breadcrumbs />
                <Routes>
                    <Route path="/" element={<WelcomeScreen />} />
                    <Route path="/Home" element={<Home />} />
                    <Route path="/Upload-documents" element={<UploadDocuments />} />
                </Routes>
            </div>
        </Router>
    );
}

export default App;


import React from 'react';
import ClipLoader from 'react-spinners/ClipLoader';

const Loader = ({ loading }) => {
    return (
        <div style={{ display: 'flex', justifyContent: 'center', alignItems: 'center', height: '100vh' }}>
            <ClipLoader color="#3498db" loading={loading} size={80} />
        </div>
    );
};

export default Loader;

npm install react-spinners
