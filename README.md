import React, { useState } from 'react';
import Select from 'react-select';
import { useNavigate } from 'react-router-dom';
import styles from './Home.module.css';

const Home = () => {
    const [selectedCategory, setSelectedCategory] = useState(null);
    const [claimType, setClaimType] = useState('');
    const [claimIdVisible, setClaimIdVisible] = useState(false);
    const [selectedClaimId, setSelectedClaimId] = useState(null);
    const [uploadedFile, setUploadedFile] = useState(null); // State to hold the uploaded file
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
    };

    const handleFileChange = (e) => {
        if (e.target.files.length > 0) {
            setUploadedFile(e.target.files[0]);
        }
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        // Make sure all fields are filled before proceeding
        if (!selectedCategory || !claimType || (claimType === 'existing' && !selectedClaimId) || !uploadedFile) {
            alert('Please fill in all required fields and upload a document.');
            return;
        }

        // Navigate to the Upload Documents page with the form data
        navigate('/upload-documents', {
            state: {
                selectedCategory,
                claimType,
                selectedClaimId,
                uploadedFile,
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
                            onChange={(option) => setSelectedClaimId(option)}
                            isClearable
                            className={styles.select}
                        />
                    </div>
                )}

                {/* Upload Claim Form */}
                <div className={styles.formGroup}>
                    <label htmlFor="upload">Upload Claim Form:</label>
                    <input type="file" id="upload" onChange={handleFileChange} />
                </div>

                {/* Process Button */}
                <button type="submit" className={styles.processButton}>
                    Process
                </button>
            </form>
        </div>
    );
};

export default Home;


import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { selectedCategory, claimType, selectedClaimId, uploadedFile } = location.state || {};

    return (
        <div className={styles.uploadDocuments}>
            <h2>Upload Documents - Review</h2>
            <div className={styles.reviewSection}>
                {/* Preview the selected options */}
                {selectedCategory && (
                    <div className={styles.reviewItem}>
                        <strong>Category:</strong> {selectedCategory.label}
                    </div>
                )}
                {claimType && (
                    <div className={styles.reviewItem}>
                        <strong>Claim Type:</strong> {claimType === 'new' ? 'New' : 'Existing'}
                    </div>
                )}
                {selectedClaimId && (
                    <div className={styles.reviewItem}>
                        <strong>Claim ID:</strong> {selectedClaimId.label}
                    </div>
                )}
                {uploadedFile ? (
                    <div className={styles.reviewItem}>
                        <strong>Uploaded Document:</strong> {uploadedFile.name}
                        <div className={styles.preview}>
                            {/* Show image preview if it's an image */}
                            {uploadedFile.type.startsWith('image/') && (
                                <img src={URL.createObjectURL(uploadedFile)} alt="Document Preview" />
                            )}
                            {/* Show a link for other document types */}
                            {!uploadedFile.type.startsWith('image/') && (
                                <a
                                    href={URL.createObjectURL(uploadedFile)}
                                    download={uploadedFile.name}
                                    target="_blank"
                                    rel="noopener noreferrer"
                                >
                                    View Document
                                </a>
                            )}
                        </div>
                    </div>
                ) : (
                    <div className={styles.reviewItem}>
                        <strong>No Document Uploaded</strong>
                        <p className={styles.noFile}>Please upload a document to preview it here.</p>
                    </div>
                )}
            </div>
        </div>
    );
};

export default UploadDocuments;


/* UploadDocuments.module.css */

.uploadDocuments {
    max-width: 800px;
    margin: 2rem auto;
    padding: 2rem;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

h2 {
    text-align: center;
    font-size: 1.8rem;
    margin-bottom: 1.5rem;
    color: #333;
}

.reviewSection {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

.reviewItem {
    padding: 1rem;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
}

.reviewItem strong {
    font-size: 1.1rem;
    margin-bottom: 0.5rem;
    color: #555;
}

.preview {
    margin-top: 1rem;
}

.preview img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.preview a {
    display: inline-block;
    margin-top: 0.5rem;
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border-radius: 4px;
    text-decoration: none;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease;
}

.preview a:hover {
    background-color: #0056b3;
}

.noFile {
    color: #888;
    font-size: 1rem;
}

@media (max-width: 768px) {
    .uploadDocuments {
        padding: 1rem;
    }

    .reviewItem {
        padding: 0.5rem;
    }

    h2 {
        font-size: 1.5rem;
    }
}
