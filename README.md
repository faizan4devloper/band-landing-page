import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
// import Sidebar from './NewSidebar';
import FormDisplay from './NewFormDisplay';
import styles from './NewPage.module.css';

const NewPage = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    // No need to extract data here since FormDisplay uses the JSON directly.
    const [selectedForm, setSelectedForm] = useState(null);

    return (
        <div className={styles.container}>
            
            
            {/* Render the FormDisplay component, passing only the selectedForm */}

            {/* Render the Sidebar component, passing the setSelectedForm function */}
            <FormDisplay selectedForm={selectedForm} />
        </div>
    );
};

export default NewPage;



.container {
  display: flex;
  flex-direction: row;
  align-items: flex-end;
  max-width: 100%;
  height: 100%;
  padding: 20px;
  position: relative;
}
 
.uploadDocuments {
  flex: 2;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  transition: all 0.5s ease;
  display: none;
}
 
.previewVisible {
  display: block;
}
 
.documentHead {
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 20px;
  color: #fff;
  text-align: center;
}
 
.reviewSection {
  display: flex;
  flex-direction: column;
}
 
.noFile {
  color: #67748b;
  font-size: 1.2rem;
  text-align: center;
}
 
.preview {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
}
 
.preview > * {
  background-color: #e2e8f0;
  border-radius: 8px;
  padding: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}
 
.preview > *:hover {
  transform: scale(1.05);
}
 
/* Sidebar and FormDisplay shrinking effect */
.shrink {
  flex: 0.8;
  transition: all 0.5s ease;
}
 
.sidebar {
  flex: 1;
  max-width: 250px;
  height: 100%;
}
 
.formDisplay {
  flex: 3;
  height: 100%;
  max-width: 600px;
  overflow-y: auto;
}
 
/* Button to toggle document preview */
.togglePreviewButton {
  position: absolute;
  left: 0;
  top: 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease;
}
 
.togglePreviewButton:hover {
  background-color: #0056b3;
}



import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import {
    faChevronRight,
    faSignature,
    faIdCard,
    faFileAlt,
    faCalendarAlt,
    faQuestionCircle,
    faInfoCircle
} from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = () => {
    const [formData, setFormData] = useState(null);
    const [expanded, setExpanded] = useState({});

    useEffect(() => {
        setFormData(formDataJson);
    }, []);

    const toggleExpanded = (key) => {
        setExpanded((prev) => ({
            ...prev,
            [key]: !prev[key],
        }));
    };

    const chooseIcon = (key) => {
        switch (key.toLowerCase()) {
            case 'approver1_focuses_on':
                return faInfoCircle;
            case 'approver2_focuses_on':
                return faSignature;
            case 'additional_information':
                return faFileAlt;
            case 'suggested_action_items':
                return faQuestionCircle;
            case 'detailed_summary':
                return faIdCard;
            default:
                return faCalendarAlt;
        }
    };

    const renderFormData = (formKey, isExpanded) => {
        if (!formData || !formData[formKey]) return null;

        const selectedData = formData[formKey];
        const entries = Object.entries(selectedData);

        const displayedEntries = isExpanded ? entries : entries.slice(0, 2);

        return (
            <div className={styles.subCardContainer}>
                {displayedEntries.map(([key, value]) => (
                    <div key={key} className={styles.subCard}>
                        <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                        <div>
                            <h2 className={styles.subCardTitle}>{key.replace(/_/g, ' ')}</h2>
                            <p className={styles.subCardContent}>
                                {Array.isArray(value) ? value.join(', ') : value}
                            </p>
                        </div>
                    </div>
                ))}
            </div>
        );
    };

    return (
        <div className={styles.dashboardContainer}>
            {['Verification Status', 'Reviewer Insights', 'Checklist'].map((section) => (
                <div className={styles.card} key={section}>
                    <h3>{section}</h3>
                    {renderFormData('claimassist-history-lambda', expanded[section])}
                    <button className={styles.viewAllButton} onClick={() => toggleExpanded(section)}>
                        {expanded[section] ? 'View Less' : 'View All'}
                    </button>
                </div>
            ))}
        </div>
    );
};

export default NewFormDisplay;




/* Container */
.dashboardContainer {
    display: grid;
    grid-template-columns: 1fr;
    gap: 15px;
    padding: 20px;
    background: linear-gradient(135deg, #f2f2f2, #e5e7eb);
    max-width: 900px;
    margin: 0 auto;
}

/* Card styling */
.card {
    background-color: #fff;
    border: 1px solid #e5e7eb;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    padding: 20px;
    border-radius: 10px;
}

/* Sub-card styling */
.subCardContainer {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.subCard {
    display: flex;
    align-items: flex-start;
    background: #f1f5f9;
    border-radius: 8px;
    padding: 10px;
}

.subCardTitle {
    font-size: 1rem;
    font-weight: 600;
}

.subCardContent {
    font-size: 0.875rem;
    margin-top: 5px;
}

/* Icon styling */
.cardIcon {
    margin-right: 10px;
    color: #3b82f6;
    font-size: 1.25rem;
}

/* Button styling */
.viewAllButton {
    background-color: transparent;
    border: none;
    color: #3b82f6;
    font-size: 0.875rem;
    cursor: pointer;
    margin-top: 10px;
}

.viewAllButton:hover {
    text-decoration: underline;
}

/* Responsive Layout */
@media (min-width: 768px) {
    .dashboardContainer {
        grid-template-columns: repeat(2, 1fr);
    }
}
