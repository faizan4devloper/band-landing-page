import React, { useState, useCallback } from 'react';
import Select from 'react-select';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload, faTimesCircle } from '@fortawesome/free-solid-svg-icons'; // Add faTimesCircle for the X icon
import styles from './Home.module.css';
import { useDropzone } from 'react-dropzone';
import customSelectStyles from './reactSelectStyles'; // Import custom styles

const Home = () => {
    const [selectedCategory, setSelectedCategory] = useState(null);
    const [claimType, setClaimType] = useState('');
    const [claimIdVisible, setClaimIdVisible] = useState(false);
    const [selectedClaimId, setSelectedClaimId] = useState(null);
    const [uploadedFile, setUploadedFile] = useState(null); // State to hold the uploaded file
    const [documents, setDocuments] = useState([]); // State to hold documents for existing claims
    const navigate = useNavigate();

    const onDrop = useCallback((acceptedFiles) => {
        setUploadedFile(acceptedFiles[0]); // Assuming only one file is uploaded
    }, []);

    const { getRootProps, getInputProps, isDragActive } = useDropzone({ onDrop, accept: 'image/jpeg, image/png', maxSize: 500000 });

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
            const fetchedDocuments = getDocumentsForClaim(option.value);
            setDocuments(fetchedDocuments);
        } else {
            setDocuments([]);
        }
    };

    const removeUploadedFile = () => {
        setUploadedFile(null); // Clear the uploaded file
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        if (!selectedCategory || !claimType || (claimType === 'existing' && !selectedClaimId) || (claimType === 'new' && !uploadedFile)) {
            alert('Please fill in all required fields and upload a document if applicable.');
            return;
        }

        navigate('/Upload-documents', {
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
                                    icon={faTimesCircle}
                                    className={styles.removeIcon}
                                    onClick={removeUploadedFile}
                                />
                                <span className={styles.fileUploadedText}>File Uploaded</span>
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
