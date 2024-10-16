import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faAnglesRight, faAnglesLeft } from '@fortawesome/free-solid-svg-icons';
import Sidebar from './Sidebar';
import DocumentPreview from './DocumentPreview';
import FormDisplay from './FormDisplay';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    const [selectedForm, setSelectedForm] = useState(null);
    const [isUploadVisible, setIsUploadVisible] = useState(false);

    const toggleUpload = () => {
        setIsUploadVisible(prevState => !prevState);
    };

    return (
        <div className={styles.container}>
            {/* Sidebar */}
            <div className={styles.sidebar}>
                <Sidebar onSelectForm={setSelectedForm} />
            </div>

            {/* FormDisplay */}
            <div className={`${styles.formDisplay} ${isUploadVisible ? styles.reduceWidth : ''}`}>
                <FormDisplay selectedForm={selectedForm} />
            </div>

            {/* UploadDocuments section */}
            <div className={`${styles.uploadDocuments} ${isUploadVisible ? styles.slideIn : styles.slideOut}`}>
                <div className={styles.reviewSection}>
                    {allDocuments.length > 0 ? (
                        <div className={styles.preview}>
                            {allDocuments.map((doc, index) => (
                                <DocumentPreview key={index} document={doc} />
                            ))}
                        </div>
                    ) : (
                        <p className={styles.noFile}>No document available</p>
                    )}
                </div>
            </div>

            {/* Toggle Button */}
            <button className={`${styles.togglePreviewButton} ${isUploadVisible ? styles.open : ''}`} onClick={toggleUpload}>
                <span>{isUploadVisible ? 'Close Preview' : 'Open Preview'}</span>
                <FontAwesomeIcon 
                    icon={isUploadVisible ? faAnglesLeft : faAnglesRight} 
                    className={styles.icon} 
                />
            </button>
        </div>
    );
};

export default UploadDocuments;





.container {
    display: flex;
    flex-direction: row;
    max-width: 100%;
    height: 100vh;
    padding: 20px;
    position: relative;
}

.sidebar {
    flex: 0 0 250px;
    height: 100%;
}

.formDisplay {
    flex: 0 0 70%;
    height: 100%;
    overflow-y: auto;
    transition: flex 0.5s ease;
}

.reduceWidth {
    flex: 0 0 45%;
}

.uploadDocuments {
    right: 0;
    top: 0;
    height: 100%;
    width: 400px;
    background: linear-gradient(135deg, #334155, #1e293b);
    padding: 20px;
    transform: translateX(100%);
    transition: transform 0.5s ease, opacity 0.5s ease;
    opacity: 0;
    overflow-y: auto;
}

.slideIn {
    transform: translateX(0);
    opacity: 1;
}

.slideOut {
    transform: translateX(100%);
    opacity: 0;
}

.togglePreviewButton {
    position: absolute;
    right: 0px;
    top: 280px;
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    color: #fff;
    border: none;
    padding: 14px 18px;
    border-radius: 50px; /* Rounded circular button */
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease, transform 0.3s ease, opacity 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
}

.togglePreviewButton span {
    margin-right: 8px;
    font-size: 1rem;
    font-weight: 600;
}

.togglePreviewButton .icon {
    transition: transform 0.5s ease;
    font-size: 1.5rem;
}

.togglePreviewButton.open .icon {
    transform: rotate(180deg); /* Rotate icon when closing */
}

.togglePreviewButton:hover {
    background-color: #0056b3;
    transform: scale(1.1); /* Slight scale effect on hover */
}

.togglePreviewButton:active {
    transform: scale(0.95); /* Shrink slightly on click */
}

/* Smooth sliding for opening and closing */












import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faAnglesRight } from '@fortawesome/free-solid-svg-icons';
import Sidebar from './Sidebar';
import DocumentPreview from './DocumentPreview';
import FormDisplay from './FormDisplay';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    const [selectedForm, setSelectedForm] = useState(null);
    const [isUploadVisible, setIsUploadVisible] = useState(false); // State to manage UploadDocuments visibility

    const toggleUpload = () => {
        setIsUploadVisible(prevState => !prevState); // Toggle visibility
    };

    return (
        <div className={styles.container}>
            {/* Sidebar */}
            <div className={styles.sidebar}>
                <Sidebar onSelectForm={setSelectedForm} />
            </div>

            {/* FormDisplay */}
            <div className={`${styles.formDisplay} ${isUploadVisible ? styles.reduceWidth : ''}`}>
                <FormDisplay selectedForm={selectedForm} />
            </div>

            {/* UploadDocuments section */}
            <div className={`${styles.uploadDocuments} ${isUploadVisible ? styles.slideIn : styles.slideOut}`}>
                <div className={styles.reviewSection}>
                    {allDocuments.length > 0 ? (
                        <div className={styles.preview}>
                            {allDocuments.map((doc, index) => (
                                <DocumentPreview key={index} document={doc} />
                            ))}
                        </div>
                    ) : (
                        <p className={styles.noFile}>No document available</p>
                    )}
                </div>
            </div>

            {/* Toggle Button */}
            <button className={styles.togglePreviewButton} onClick={toggleUpload}>
                {isUploadVisible ? 'Close Preview' : 'Open Preview'}
                <FontAwesomeIcon icon={faAnglesRight} />
            </button>
        </div>
    );
};

export default UploadDocuments;


.container {
    display: flex;
    flex-direction: row;
    max-width: 100%;
    height: 100vh; /* Full height of the viewport */
    padding: 20px;
    position: relative;
}

.sidebar {
    flex: 0 0 250px; /* Fixed width for Sidebar */
    height: 100%;
}

.formDisplay {
    flex: 0 0 70%; /* Takes remaining space */
    height: 100%;
    overflow-y: auto;
    transition: flex 0.5s ease; /* Smooth transition for width adjustment */
}

/* When UploadDocuments is visible, reduce FormDisplay width */
.reduceWidth {
    flex: 0 0 45%; /* Adjust to desired width */
}

.uploadDocuments {
    right: 0; /* Align to the right */
    top: 0; /* Align to the top */
    height: 100%; /* Full height */
    width: 400px; /* Fixed width for UploadDocuments */
    background:linear-gradient(135deg, #334155, #1e293b);
    /*box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);*/
    padding: 20px;
    transform: translateX(100%); /* Start off-screen */
    transition: transform 0.5s ease, opacity 0.5s ease; /* Smooth transition */
    opacity: 0; /* Start hidden */
    overflow-y: auto; /* Manage overflow */
}

/* Slide-in from the right */
.slideIn {
    transform: translateX(0);
    opacity: 1;
}

/* Slide-out to the right */
.slideOut {
    transform: translateX(100%);
    opacity: 0;
}

.documentHead {
    font-size: 1.4rem;
    margin:0;
    font-weight: bold;
    color: #fff;
    text-align: center;
} 

.reviewSection {
       display: flex;
    margin-top: 30px;
    flex-direction: column;
    /* height: calc(100% - 50px); */
    overflow-y: auto;
}

.noFile {
    color: #67748b;
    font-size: 1.2rem;
    text-align: center;
}

.preview {
    display: flex;
    margin: auto;
    flex-wrap: wrap;
    gap: 15px;
}

.preview > * {
    background-color: #f1f5f9;
    /*border-radius: 8px;*/
    padding: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.togglePreviewButton {
        position: absolute;
    right: 0px;
    top: 280px;
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    color: #fff;
    border: none;
    padding: 14px 18px;
    border-radius: 6px;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease, transform 0.3s ease;
  
}

.togglePreviewButton:hover {
    background-color: #0056b3;
}
make the open and close preview button more animated and live for the goin to the next or opening new window and after opening new window the icon will change accordingly i want beutiful styling if you can use button or not its okay its depends on you i want only the that open and close preview like good and guidence for the open and close the preview
