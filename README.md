import React, { useState } from 'react';
import Select from 'react-select';
import { useNavigate } from 'react-router-dom';
import styles from './Home.module.css';

const Home = () => {
    const [selectedCategory, setSelectedCategory] = useState(null);
    const [claimType, setClaimType] = useState('');
    const [claimIdVisible, setClaimIdVisible] = useState(false);
    const navigate = useNavigate(); // Hook to navigate programmatically


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

    const handleSubmit = (e) => {
        e.preventDefault();
        // Add form submission logic here
        console.log('Form Submitted');
    };
    
    const handleProceedToUpload = () => {
        // Navigate to the Upload Documents page
        navigate('/upload-documents');
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
                            onChange={(option) => console.log(option)}
                            isClearable
                            className={styles.select}
                        />
                    </div>
                )}

                {/* Upload Claim Form */}
                <div className={styles.formGroup}>
                    <label htmlFor="upload">Upload Claim Form:</label>
                    <input type="file" id="upload" />
                </div>

                {/* Process Button */}
                <button type="submit" className={styles.processButton} onClick={handleProceedToUpload}>
                    Process
                </button>
            </form>
        </div>
    );
};

export default Home;

import React from 'react';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    return (
        <div className={styles.uploadDocuments}>
            
        </div>
    );
};

export default UploadDocuments;
