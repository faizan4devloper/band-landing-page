import React, { useState } from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import { Provider, useDispatch, useSelector } from 'react-redux';
import styles from './App.module.css';
import Personas from './components/Personas/Personas';
import EducationPage from './components/Pages/Education/EducationPage';
import JobPage from './components/Pages/Job/JobPage';
import HealthPage from './components/Pages/Health/HealthPage';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';
import store from './redux/store'; // Import your Redux store
import { AuthProvider } from './context/AuthContext'; // Import Context Provider



function App() {
  // Use Redux state for authentication
  const isAuthenticated = useSelector((state) => state.auth.isAuthenticated);
  const dispatch = useDispatch(); // To dispatch actions

  return (
    <Provider store={store}>
      <AuthProvider>
        <Router>
          <div className={styles.App}>
            <Header />
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
          </div>
        </Router>
      </AuthProvider>
    </Provider>
  );
}

export default App;
