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

import React from 'react';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData = {} }) => {
    return (
        <div className={styles.formContainer}>
            <h3>Payment Instruction Form</h3>
            <p><strong>Statement Date:</strong> {formData.STATEMENT_DATE || 'N/A'}</p>
            <p><strong>Policy Number:</strong> {formData.POLICY_NUMBER || 'N/A'}</p>
            <p><strong>Policy On the Life Of:</strong> {formData.POLICY_ON_THE_LIFE_OF || 'N/A'}</p>
            <p><strong>Policy Owner:</strong> {formData.POLICY_OWNER || 'N/A'}</p>
        </div>
    );
};

export default PaymentInstructionForm;
