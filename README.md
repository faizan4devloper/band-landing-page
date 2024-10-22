import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import styles from './Breadcrumbs.module.css'; // Assuming you are using CSS modules

const Breadcrumbs = () => {
  const location = useLocation();
  const pathnames = location.pathname.split('/').filter((x) => x);

  return (
    <nav className={styles.breadcrumbs}>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        {pathnames.map((value, index) => {
          const to = `/${pathnames.slice(0, index + 1).join('/')}`;

          // Customize the name to display 'Personas' or other specific values
          const breadcrumbName = (value === 'education') ? 'Education' : 
                                (value === 'job') ? 'Job' : 
                                (value === 'health') ? 'Health' : 
                                (value === 'personas') ? 'Personas' : value;

          return (
            <li key={to}>
              <Link to={to}>{breadcrumbName}</Link>
            </li>
          );
        })}
      </ul>
    </nav>
  );
};

export default Breadcrumbs;


function App() {
  const isAuthenticated = useSelector((state) => state.auth.isAuthenticated);
  const dispatch = useDispatch();

  return (
    <Provider store={store}>
      <AuthProvider>
        <Router>
          <div className={styles.App}>
            <Header />
            {isAuthenticated && <Breadcrumbs />} {/* Only show breadcrumbs when authenticated */}
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
