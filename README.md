import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import './Breadcrumbs.css'; // Custom CSS for styling

const Breadcrumbs = () => {
  const location = useLocation();
  const pathnames = location.pathname.split('/').filter(x => x);

  return (
    <nav className="breadcrumbs">
      <Link to="/">Home</Link>
      {pathnames.map((pathname, index) => {
        const to = `/${pathnames.slice(0, index + 1).join('/')}`;
        return (
          <span key={to}>
            <span className="breadcrumb-separator">/</span>
            <Link to={to}>{pathname.charAt(0).toUpperCase() + pathname.slice(1)}</Link>
          </span>
        );
      })}
    </nav>
  );
};

export default Breadcrumbs;
