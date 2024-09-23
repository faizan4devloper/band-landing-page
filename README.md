import React, { useState, useEffect } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import Home from './components/Pages/Home/Home';
import UploadDocuments from './components/Pages/UploadDocuments';
import Breadcrumbs from './components/Breadcrumbs/Breadcrumbs'; // Importing the Breadcrumbs component
import { BeatLoader } from 'react-spinners';
import styles from './App.module.css'; // Assuming you're using CSS modules

function App() {
    const [loading, setLoading] = useState(true); // Manage loading state

    // Simulate loading process and hide loader after 2 seconds
    useEffect(() => {
        const timer = setTimeout(() => {
            setLoading(false);
        }, 2000); // You can adjust the duration as needed

        return () => clearTimeout(timer); // Cleanup timeout on component unmount
    }, []);

    return (
        <Router>
            <div className="App">
                {/* Show loader when loading is true */}
                {loading ? (
                    <div className={styles.loader}>
                        <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
                    </div>
                ) : (
                    <>
                        <Header />
                        {/* Breadcrumbs component will dynamically display based on current route */}
                        <Breadcrumbs />
                        <Routes>
                            <Route path="/" element={<WelcomeScreen />} />
                            <Route path="/Home" element={<Home />} />
                            <Route path="/Upload-documents" element={<UploadDocuments />} />
                        </Routes>
                    </>
                )}
            </div>
        </Router>
    );
}

export default App;
