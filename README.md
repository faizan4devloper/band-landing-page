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
                    <Route path="/Home" element={<Home />} />
                    <Route path="/Upload-documents" element={<UploadDocuments />} />
                </Routes>
            </div>
        </Router>
    );
}

export default App;
