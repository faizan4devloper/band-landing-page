// mainContent.js

import React from 'react';
import './mainContent.css'; // Import CSS for styling

const MainContent = () => {
  return (
    <div className="main-content">
      {/* Other content */}
      <section className="benefits-section">
        <h2>Email EAR benefits</h2>
        <div className="benefits-table">
          <table>
            <thead>
              <tr>
                <th>Benefit</th>
                <th>Description</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Benefit 1</td>
                <td>Description of Benefit 1</td>
              </tr>
              <tr>
                <td>Benefit 2</td>
                <td>Description of Benefit 2</td>
              </tr>
              {/* Add more rows as needed */}
            </tbody>
          </table>
        </div>
      </section>
      {/* Other sections */}
    </div>
  );
};

export default MainContent;




/* mainContent.css */

.main-content {
  /* Add general styles for the main content area if needed */
}

.benefits-section {
  margin-bottom: 20px; /* Example margin for section */
}

.benefits-table {
  margin-top: 20px; /* Example margin for table */
}

.benefits-table table {
  width: 100%;
  border-collapse: collapse;
}

.benefits-table th, .benefits-table td {
  padding: 8px;
  text-align: left;
  border: 1px solid #ddd;
}

.benefits-table th {
  background-color: #f2f2f2;
  color: #333;
}
