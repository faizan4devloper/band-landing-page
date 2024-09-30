    import React from 'react';
    import styles from './LostPolicyForm.module.css';
    
    const LostPolicyForm = ({ lostPolicyFormData = {} }) => {
        return (
            <div className={styles.lostPolicyContainer}>
               <div className={styles.formContainer}>
            <h3>Lost Policy Form</h3>
            {Object.keys(lostPolicyFormData).length > 0 ? (
                Object.entries(lostPolicyFormData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
            </div>
        );
    };
    
    export default LostPolicyForm;

    .lostPolicyContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    /*border-radius: 10px;*/
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px;
}

.lostPolicyContainer h3 {
    color: #333;
    font-size: 1.6rem;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
}

.lostPolicyContainer p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
}

strong {
    color: #7ca2e1;
}


    import React from 'react';
    import styles from './PaymentDetails.module.css';
    
    const PaymentDetails = ({ paymentData = {}}) => {
        return (
            <div className={styles.paymentContainer}>
                <div className={styles.formContainer}>
            <h3>Payment Details Form</h3>
            {Object.keys(paymentData).length > 0 ? (
                Object.entries(paymentData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
            </div>
        );
    };
    
    export default PaymentDetails;


    .paymentContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    /*border-radius: 10px;*/
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px;
}

.paymentContainer h3 {
    color: #333;
    font-size: 1.6rem;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
}

.paymentContainer p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
}

strong {
    color: #7ca2e1;
}



import React from 'react';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData = {} }) => {
    // Dynamically generate form fields from the formData object
    return (
        <div className={styles.formContainer}>
            <h3>Payment Instruction Form</h3>
            {Object.keys(formData).length > 0 ? (
                Object.entries(formData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
    );
};

export default PaymentInstructionForm;

.formContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    /*border-radius: 10px;*/
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px;
}

.formContainer h3 {
    color: #333;
    font-size: 1.6rem;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
}

p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
}

strong {
    color: #7ca2e1;
}

import React from 'react';
import styles from './WitnessDetails.module.css';

const WitnessDetails = ({ witnessData ={} }) => {
    return (
        <div className={styles.witnessContainer}>
            <div className={styles.formContainer}>
            <h3>Witness Details</h3>
            {Object.keys(witnessData).length > 0 ? (
                Object.entries(witnessData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
        </div>
    );
};

export default WitnessDetails;

.witnessContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    /*border-radius: 10px;*/
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px;
}

.witnessContainer h3 {
    color: #333;
    font-size: 1.6rem;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
}

.witnessContainer p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
}

strong {
    color: #7ca2e1;
}
