import React, { useState, useCallback } from 'react';
import Select from 'react-select';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload, faTimes } from '@fortawesome/free-solid-svg-icons';
import styles from './Home.module.css';
import { useDropzone } from 'react-dropzone';
import customSelectStyles from './reactSelectStyles'; // Import custom styles

// Mock function to get documents for a given claim ID
const getDocumentsForClaim = (claimId) => {
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
    const [uploadedFile, setUploadedFile] = useState(null);
    const [documents, setDocuments] = useState([]);
    const [feedbackMessage, setFeedbackMessage] = useState({ message: '', type: '' }); // Feedback message state
    const navigate = useNavigate();
    
    const onDrop = useCallback((acceptedFiles) => {
        setUploadedFile(acceptedFiles[0]);
    }, []);

    const { getRootProps, getInputProps, isDragActive } = useDropzone({ onDrop, accept: 'image/jpeg, image/png', maxSize: 500000 });

    const categoryOptions = [
        { value: 'Surrender Claim', label: 'Surrender Claim' },
        { value: 'Retirement Claim', label: 'Retirement Claim' },
        { value: 'Death Claim', label: 'Death Claim' },
    ];

    const claimIdOptions = [
        { value: 'claim-123', label: 'Claim ID 123' },
        { value: 'claim-456', label: 'Claim ID 456' },
    ];

    const handleClaimTypeChange = (type) => {
        setClaimType(type);
        setClaimIdVisible(type === 'existing');
        if (type === 'new') {
            setDocuments([]);
            setSelectedClaimId(null);
        }
    };

    const handleClaimIdChange = (option) => {
        setSelectedClaimId(option);
        if (option) {
            const fetchedDocuments = getDocumentsForClaim(option.value);
            setDocuments(fetchedDocuments);
        } else {
            setDocuments([]);
        }
    };

    const removeUploadedFile = () => {
        setUploadedFile(null);
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        // Validate required fields
        if (!selectedCategory || !claimType || (claimType === 'existing' && !selectedClaimId) || (claimType === 'new' && !uploadedFile)) {
            setFeedbackMessage({ message: 'Please fill in all required fields and upload a document if applicable.', type: 'error' });
            return;
        }

        // Success message before navigating
        setFeedbackMessage({ message: 'Form submitted successfully! Redirecting...', type: 'success' });

        setTimeout(() => {
            navigate('/Upload-documents', {
                state: {
                    selectedCategory,
                    claimType,
                    selectedClaimId,
                    uploadedFile,
                    documents,
                },
            });
        }, 1500); // Small delay for user to see the success message
    };

    return (
        <div className={styles.home}>
            <form className={styles.claimForm} onSubmit={handleSubmit}>
                {/* Feedback Message */}
                {feedbackMessage.message && (
                    <div className={`${styles.feedback} ${styles[feedbackMessage.type]}`}>
                        {feedbackMessage.message}
                    </div>
                )}

                {/* Choose Claim Category */}
                <div className={styles.formGroup}>
                    <label htmlFor="category">Choose Claim Category:</label>
                    <Select
                        id="category"
                        options={categoryOptions}
                        onChange={(option) => setSelectedCategory(option)}
                        isClearable
                        className={styles.select}
                        styles={customSelectStyles}
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

                {/* Claim ID Dropdown */}
                {claimIdVisible && (
                    <div className={styles.formGroup}>
                        <label htmlFor="claimId">Choose Claim ID:</label>
                        <Select
                            id="claimId"
                            options={claimIdOptions}
                            onChange={handleClaimIdChange}
                            isClearable
                            className={styles.select}
                            styles={customSelectStyles}
                        />
                    </div>
                )}

                {/* Upload Area (Drag and Drop) */}
                {claimType === 'new' && (
                    <div className={styles.formGroup}>
                        <label>Upload Claim Form:</label>
                        <div {...getRootProps({ className: styles.dropzone })}>
                            <input {...getInputProps()} />
                            {isDragActive ? (
                                <p>Drop the files here...</p>
                            ) : (
                                <div>
                                    <p>
                                        <FontAwesomeIcon icon={faUpload} />
                                    </p>
                                    <p>Drag & Drop or Choose File to Upload</p>
                                </div>
                            )}
                        </div>
                        {uploadedFile && (
                            <div className={styles.uploadedFileContainer}>
                                <FontAwesomeIcon
                                    icon={faTimes}
                                    className={styles.removeIcon}
                                    onClick={removeUploadedFile}
                                />
                                <span className={styles.fileUploadedText}>{uploadedFile.name}</span>
                            </div>
                        )}
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






.feedback {
    margin-bottom: 1rem;
    padding: 0.75rem;
    border-radius: 5px;
    font-size: 1rem;
    font-weight: bold;
    text-align: center;
}

.success {
    background-color: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
}

.error {
    background-color: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
}
