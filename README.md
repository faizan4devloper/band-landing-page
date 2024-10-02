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
                return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default FormDisplay;


{
    "category": "Payment Instruction Form",
    "extracted_data": {
        "PAYMENT_INSTRUCTION_FORM": {
            "STATEMENT_DATE": "12/06/2024",
            "POLICY_NUMBER": "L2065777",
            "POLICY_ON_THE_LIFE_OF": "Mr JC Mcglynn",
            "POLICY_OWNER": "Mr JC Mcglynn"
        },
        "PAYMENT_DETAILS": {
            "BANK_NAME_AND_ADDRESS": "BARCLAYS BANK",
            "ACCOUNT_HOLDERS_NAME": "J.C.MC Ghywon",
            "ACCOUNT_NUMBER": "50614866",
            "BANK_SORT_CODE": "20-57-40",
            "SIGNED_FULL_NAME": "Mr Mcglynn, James Christopher",
            "SIGNED_DATE": "13/06/2024"
        },
        "LOST_POLICY_FORM": {
            "STATEMENT_DATE": "12/06/2024",
            "POLICY_NUMBER": "L2065777",
            "POLICY_ON_THE_LIFE_OF": "Mr J C Mcglynn",
            "POLICY_OWNER": "Mr JC Mcglynn"
        },
        "LOST_POLICY_FORM_SIGNED": {
            "FULL_NAME": "Mr Mcglynn, James Christopher",
            "DATE": "13/06/2024"
        },
        "LOST_POLICY_FORM_WITNESSED_BY": {
            "FULL_NAME_OF_WITNESS": "SOPHIE PASSFIELD",
            "DATE": "13/06/2024",
            "ADDRESS_OF_WITNESS": "53 ORNE GARDENS, BOLBECIC PARK, MILTON KEYNES MK18 8PG",
            "OFFICIAL_STAMP": "",
            "DAY-TIME_TELEPHONE_NUMBER_OF_WITNESS": "07732883 700",
            "OCCUPATION_OF_WITNESS": "Teacher"
        }
    },
    "unfilledpercent": "4%"
}
