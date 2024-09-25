import React, { useState, useCallback } from 'react';
import { useNavigate } from 'react-router-dom';
import Select from 'react-select';
import { useDropzone } from 'react-dropzone'; // Import dropzone
import styles from './Home.module.css';
import customSelectStyles from './reactSelectStyles';

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
    const navigate = useNavigate();

    const onDrop = useCallback((acceptedFiles) => {
        setUploadedFile(acceptedFiles[0]); // Assuming only one file is uploaded
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
                                <p>Drag & drop your files here, or click to select files</p>
                            )}
                        </div>
                        {uploadedFile && <p>{uploadedFile.name}</p>}
                    </div>
                )}

                <button type="submit" className={styles.processButton}>
                    Process
                </button>
            </form>
        </div>
    );
};






.home {
    padding: 2rem;
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 8px;
    color: #fff;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    max-width: 430px;
    margin: 2rem auto;
}

/* Dropzone Styling */
.dropzone {
    border: 2px dashed #7ca2e1;
    padding: 2rem;
    text-align: center;
    cursor: pointer;
    border-radius: 8px;
    background-color: #e6f5ff;
    color: #333;
    transition: background-color 0.3s ease;
}

.dropzone p {
    margin: 0;
    font-size: 1rem;
    color: #6c757d;
}

.dropzone:hover {
    background-color: #d0ebff;
}

/* Form Group */
.formGroup {
    margin-bottom: 1.5rem;
}

/* Process Button */
.processButton {
    display: inline-flex;
    align-items: center;
    padding: 0.75rem 1.5rem;
    font-size: 1.1rem;
    color: #fff;
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    position: relative;
    overflow: hidden;
}

.processButton::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: rgba(255, 255, 255, 0.1);
    transform: rotate(45deg) scale(0);
    transition: transform 0.5s ease;
}

.processButton:hover::before {
    transform: rotate(45deg) scale(1);
}

export default Home;
