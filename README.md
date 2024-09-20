// reactSelectStyles.js

const customSelectStyles = {
    control: (provided, state) => ({
        ...provided,
        background: 'rgba(255, 255, 255, 0.1)',
        borderColor: state.isFocused ? '#FF005C' : '#1B98E0',
        boxShadow: state.isFocused ? '0 0 0 1px #FF005C' : 'none',
        '&:hover': {
            borderColor: '#FF005C',
        },
        color: '#fff',
    }),
    menu: (provided) => ({
        ...provided,
        background: '#1B98E0',
        borderRadius: '8px',
        boxShadow: '0 8px 16px rgba(0, 0, 0, 0.2)',
        marginTop: '0.5rem',
    }),
    option: (provided, state) => ({
        ...provided,
        background: state.isSelected
            ? '#FF005C'
            : state.isFocused
            ? 'rgba(255, 0, 92, 0.2)'
            : 'transparent',
        color: '#fff',
        cursor: 'pointer',
        '&:active': {
            background: '#FF005C',
        },
    }),
    singleValue: (provided) => ({
        ...provided,
        color: '#fff',
    }),
    placeholder: (provided) => ({
        ...provided,
        color: '#e2e2e2',
    }),
    dropdownIndicator: (provided) => ({
        ...provided,
        color: '#fff',
        '&:hover': {
            color: '#FF005C',
        },
    }),
    indicatorSeparator: (provided) => ({
        ...provided,
        background: '#fff',
    }),
};

export default customSelectStyles;


// Home.jsx
import React, { useState } from 'react';
import Select from 'react-select';
import { useNavigate } from 'react-router-dom';
import styles from './Home.module.css';
import customSelectStyles from './reactSelectStyles'; // Import custom styles

// Mock function to get documents for a given claim ID
const getDocumentsForClaim = (claimId) => {
    // Replace this with actual API call to get documents
    const documents = {
        'claim-123': [{ name: 'Document 1.pdf', url: 'https://gbihr.org/images/docs/test.pdf' }],
        'claim-456': [{ name: 'Document 2.pdf', url: 'https://tourism.gov.in/sites/default/files/2019-04/dummy-pdf_2.pdf' }],
    };
    return documents[claimId] || [];
};

const Home = () => {
    const [selectedCategory, setSelectedCategory] = useState(null);
    const [claimType, setClaimType] = useState('');
    const [claimIdVisible, setClaimIdVisible] = useState(false);
    const [selectedClaimId, setSelectedClaimId] = useState(null);
    const [uploadedFile, setUploadedFile] = useState(null); // State to hold the uploaded file
    const [documents, setDocuments] = useState([]); // State to hold documents for existing claims
    const navigate = useNavigate();

    // Options for the category select input
    const categoryOptions = [
        { value: 'Surrender Claim', label: 'Surrender Claim' },
        { value: 'Retirement Claim', label: 'Retirement Claim' },
        { value: 'Death Claim', label: 'Death Claim' },
    ];

    // Options for the claim ID select input
    const claimIdOptions = [
        { value: 'claim-123', label: 'Claim ID 123' },
        { value: 'claim-456', label: 'Claim ID 456' },
        // Add more claim IDs as needed
    ];

    const handleClaimTypeChange = (type) => {
        setClaimType(type);
        setClaimIdVisible(type === 'existing');
        if (type === 'new') {
            setDocuments([]); // Reset documents for new claims
            setSelectedClaimId(null);
        }
    };

    const handleClaimIdChange = (option) => {
        setSelectedClaimId(option);
        if (option) {
            // Fetch documents for the selected claim ID
            const fetchedDocuments = getDocumentsForClaim(option.value);
            setDocuments(fetchedDocuments);
        } else {
            setDocuments([]);
        }
    };

    const handleFileChange = (e) => {
        if (e.target.files.length > 0) {
            setUploadedFile(e.target.files[0]);
        }
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        // Make sure all fields are filled before proceeding
        if (!selectedCategory || !claimType || (claimType === 'existing' && !selectedClaimId) || (claimType === 'new' && !uploadedFile)) {
            alert('Please fill in all required fields and upload a document if applicable.');
            return;
        }

        // Navigate to the Upload Documents page with the form data
        navigate('/upload-documents', {
            state: {
                selectedCategory,
                claimType,
                selectedClaimId,
                uploadedFile,
                documents,
            },
        });
    };

    return (
        <div className={styles.home}>
            <form className={styles.claimForm} onSubmit={handleSubmit}>
                {/* Choose Claim Category */}
                <div className={styles.formGroup}>
                    <label htmlFor="category">Choose Claim Category:</label>
                    <Select
                        id="category"
                        options={categoryOptions}
                        onChange={(option) => setSelectedCategory(option)}
                        isClearable
                        className={styles.select}
                        styles={customSelectStyles} // Apply custom styles
                        placeholder="Select a category..."
                    />
                </div>

                {/* Select Type of Claim */}
                <div className={styles.formGroup}>
                    <label>Select Type of Claim:</label>
                    <div className={styles.checkboxGroup}>
                        <label>
                            <input
                                type="radio"
                                name="claimType"
                                value="new"
                                checked={claimType === 'new'}
                                onChange={() => handleClaimTypeChange('new')}
                            />
                            New
                        </label>
                        <label>
                            <input
                                type="radio"
                                name="claimType"
                                value="existing"
                                checked={claimType === 'existing'}
                                onChange={() => handleClaimTypeChange('existing')}
                            />
                            Existing
                        </label>
                    </div>
                </div>

                {/* Choose Claim ID (Visible only if "Existing" is selected) */}
                {claimIdVisible && (
                    <div className={styles.formGroup}>
                        <label htmlFor="claimId">Choose Claim ID:</label>
                        <Select
                            id="claimId"
                            options={claimIdOptions}
                            onChange={handleClaimIdChange}
                            isClearable
                            className={styles.select}
                            styles={customSelectStyles} // Apply custom styles
                            placeholder="Select a claim ID..."
                        />
                    </div>
                )}

                {/* Upload Claim Form (Visible only if "New" is selected) */}
                {claimType === 'new' && (
                    <div className={styles.formGroup}>
                        <label htmlFor="upload">Upload Claim Form:</label>
                        <input
                            type="file"
                            id="upload"
                            onChange={handleFileChange}
                            className={styles.fileInput}
                            accept=".pdf, .doc, .docx" // Restrict file types
                        />
                    </div>
                )}

                {/* Process Button */}
                <button type="submit" className={styles.processButton}>
                    Process
                </button>
            </form>
        </div>
    );
};

export default Home;
