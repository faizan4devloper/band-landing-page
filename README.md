import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import styles from './Breadcrumbs.module.css';

const Breadcrumbs = () => {
  const location = useLocation();
  const paths = location.pathname.split('/').filter((path) => path);

  return (
    <nav className={styles.breadcrumbs}>
      <ul>
        <li>
          <Link to="/">🏠 Home</Link>
        </li>
        {paths.map((path, index) => {
          const fullPath = `/${paths.slice(0, index + 1).join('/')}`;
          return (
            <li key={index}>
              <Link to={fullPath}>
                {path.charAt(0).toUpperCase() + path.slice(1)}
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
  padding: 10px 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  margin-bottom: 20px;
}

.breadcrumbs ul {
  list-style: none;
  display: flex;
  gap: 10px;
}

.breadcrumbs li {
  font-size: 16px;
}

.breadcrumbs a {
  text-decoration: none;
  color: #007bff;
}

.breadcrumbs a:hover {
  text-decoration: underline;
}

.breadcrumbs li::after {
  content: '>';
  margin-left: 10px;
}

.breadcrumbs li:last-child::after {
  content: '';
}






import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import { Provider, useDispatch, useSelector } from 'react-redux';
import styles from './App.module.css';
import Personas from './components/Personas/Personas';
import EducationPage from './components/Pages/Education/EducationPage';
import JobPage from './components/Pages/Job/JobPage';
import HealthPage from './components/Pages/Health/HealthPage';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';
import Breadcrumbs from './components/Breadcrumbs/Breadcrumbs'; // Import Breadcrumbs
import store from './redux/store'; 
import { AuthProvider } from './context/AuthContext';

function App() {
  const isAuthenticated = useSelector((state) => state.auth.isAuthenticated);
  const dispatch = useDispatch();

  return (
    <Provider store={store}>
      <AuthProvider>
        <Router>
          <div className={styles.App}>
            <Header />
            <Breadcrumbs /> {/* Add Breadcrumbs component */}
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
