import React, { useEffect, useState } from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import { Provider, useDispatch, useSelector } from 'react-redux';
import ClipLoader from 'react-spinners/ClipLoader'; // Import the spinner

import styles from './App.module.css';
import Personas from './components/Personas/Personas';
import EducationPage from './components/Pages/Education/EducationPage';
import JobPage from './components/Pages/Job/JobPage';
import HealthPage from './components/Pages/Health/HealthPage';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';
import store from './redux/store'; 
import { AuthProvider } from './context/AuthContext';

function App() {
  const isAuthenticated = useSelector((state) => state.auth.isAuthenticated);
  const dispatch = useDispatch();

  const [loading, setLoading] = useState(true); // Loading state

  // Simulate loading (e.g., authentication check) on initial load
  useEffect(() => {
    setTimeout(() => setLoading(false), 1000); // Adjust timing as needed
  }, []);

  return (
    <Provider store={store}>
      <AuthProvider>
        <Router>
          {/* Loader at the top of all content */}
          {loading && (
            <div className={styles.topLoader}>
              <ClipLoader color="#0073e6" loading={loading} size={40} />
            </div>
          )}
          {!loading && <Header />}
          <div className={styles.App}>
            {!loading && (
              <Routes>
                <Route
                  path="/"
                  element={<LoginPage setIsAuthenticated={() => dispatch({ type: 'LOGIN' })} />}
                />
                {isAuthenticated ? (
                  <>
                    <Route path="/personas" element={<Personas />} />
                    <Route path="/education" element={<EducationPage />} />
                    <Route path="/job" element={<JobPage />} />
                    <Route path="/health" element={<HealthPage />} />
                  </>
                ) : (
                  <Route path="*" element={<LoginPage setIsAuthenticated={() => dispatch({ type: 'LOGIN' })} />} />
                )}
              </Routes>
            )}
          </div>
        </Router>
      </AuthProvider>
    </Provider>
  );
}

export default App;
