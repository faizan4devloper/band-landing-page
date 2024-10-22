import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import styles from './Breadcrumbs.module.css';

const Breadcrumbs = () => {
  const location = useLocation();
  const paths = location.pathname.split('/').filter((path) => path);

  const createBreadcrumbLink = (path, index) => {
    const fullPath = `/${paths.slice(0, index + 1).join('/')}`;

    if (path === 'education' || path === 'job' || path === 'health') {
      return (
        <>
          <li key="personas">
            <Link to="/personas">Personas</Link>
          </li>
          <li key={index}>
            <Link to={fullPath}>{path.charAt(0).toUpperCase() + path.slice(1)}</Link>
          </li>
        </>
      );
    }

    return (
      <li key={index}>
        <Link to={fullPath}>
          {path.charAt(0).toUpperCase() + path.slice(1)}
        </Link>
      </li>
    );
  };

  return (
    <nav className={styles.breadcrumbs}>
      <ul>
        <li>
          <Link to="/">🏠 Home</Link>
        </li>
        {paths.map((path, index) => createBreadcrumbLink(path, index))}
      </ul>
    </nav>
  );
};

export default Breadcrumbs;
