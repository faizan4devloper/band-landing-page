import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import Home from './components/Pages/Home/Home'; // Create a HomeScreen component
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
