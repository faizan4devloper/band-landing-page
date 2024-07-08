
.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
}


.mainContent h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
  margin-top: 0;
  border-bottom: 2px solid rgba(95, 30, 193, 0.8);
  padding-bottom: 5px;
}

.mainContent ul {
  list-style-type: disc;
  margin-left: 20px;
  padding-left: 20px;
}

.mainContent ul li {
  font-size: 12px;
  line-height: 1.6;
  color: #000;
  margin-bottom: 10px;
}

.mainContent img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 20px 0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.benefits, .description, .demo, .architecture, .adoption, .solution{
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
}

.benefits p, .description li {
  margin: 0;
  font-size: 12px;
}

.benefits h2, .description h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
}

.adoptionTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.adoptionTable th, .adoptionTable td {
  border: 1px solid #ddd;
  font-size: 12px;
  padding: 10px;
  text-align: left;
  vertical-align: top;
}

.adoptionTable th {
  background-color: #5f1ec1;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

.adoptionTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.adoptionTable tr:hover {
  background-color: #f1f1f1;
}

.adoptionTable td:first-child {
  font-weight: bold;
  color: #5f1ec1;
}

.highlight{
  font-style: italic;
  /*font-weight: bold;*/
  color: #DD9313;
  /*color: #7C2917;*/
}






/* Style for .mainContent element */
.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
}

/* Custom Scrollbar Styling */
.mainContent::-webkit-scrollbar {
  width: 12px; /* Width of the scrollbar */
}

.mainContent::-webkit-scrollbar-track {
  background: #f1f1f1; /* Track background color */
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: #5f1ec1; /* Thumb color */
  border-radius: 20px; /* Rounded corners */
  border: 3px solid #f1f1f1; /* Border around the thumb */
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: #3d1299; /* Thumb color on hover */
}

/* Other existing styles for .mainContent */
.mainContent h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
  margin-top: 0;
  border-bottom: 2px solid rgba(95, 30, 193, 0.8);
  padding-bottom: 5px;
}

.mainContent ul {
  list-style-type: disc;
  margin-left: 20px;
  padding-left: 20px;
}

.mainContent ul li {
  font-size: 12px;
  line-height: 1.6;
  color: #000;
  margin-bottom: 10px;
}

.mainContent img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 20px 0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.benefits, .description, .demo, .architecture, .adoption, .solution {
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
}

.benefits p, .description li {
  margin: 0;
  font-size: 12px;
}

.benefits h2, .description h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
}

.adoptionTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.adoptionTable th, .adoptionTable td {
  border: 1px solid #ddd;
  font-size: 12px;
  padding: 10px;
  text-align: left;
  vertical-align: top;
}

.adoptionTable th {
  background-color: #5f1ec1;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

.adoptionTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.adoptionTable tr:hover {
  background-color: #f1f1f1;
}

.adoptionTable td:first-child {
  font-weight: bold;
  color: #5f1ec1;
}

.highlight {
  font-style: italic;
  color: #DD9313;
}





// Header.js
import React from "react";
import styles from "./Header.module.css";
import logoImage from "./HCL Tech.svg";
import { useNavigate } from 'react-router-dom';

  
const Header = () => {
  
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate('/');
  };


  return (
    <div className={styles.navbarWrapper}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img src={logoImage} alt="" onClick={handleImageClick} style={{cursor:'pointer'}} title="Navigate to Home"/>
        </div>
        <div className={styles.right}>
          <button className={styles.button}>Request for live demo</button>
        </div>
      </nav>
      <div className={styles.border}></div>
    </div>
  );
};

export default Header;
