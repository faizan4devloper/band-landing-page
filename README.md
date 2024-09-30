// FormDisplay.js
import React from 'react';
import PaymentInstructionForm from '../Entities/PaymentInstructionForm';
import PaymentDetails from '../Entities/PaymentDetails';
import LostPolicyForm from '../Entities/LostPolicyForm';
import WitnessDetails from '../Entities/WitnessDetails';
import styles from './FormDisplay.module.css';

const FormDisplay = ({ selectedForm, formData, paymentData, lostPolicyFormData, witnessData }) => {
    const renderFormData = () => {
        switch (selectedForm) {
            case 'PaymentInstructionForm':
                return <PaymentInstructionForm formData={formData} />;
            case 'PaymentDetails':
                return <PaymentDetails paymentData={paymentData} />;
            case 'LostPolicyForm':
                return <LostPolicyForm lostPolicyFormData={lostPolicyFormData} />;
            case 'WitnessDetails':
                return <WitnessDetails witnessData={witnessData} />;
            default:
                return <p>Please select a form to view its data.</p>;
        }
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default FormDisplay;
