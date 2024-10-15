Uncaught Error: Objects are not valid as a React child (found: object with keys {ExtractedData, rawtext, keyvaluesText}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:13144:1)
    at reconcileChildFibers (react-dom.development.js:14085:1)
    at reconcileChildren (react-dom.development.js:19203:1)
    at updateHostComponent (react-dom.development.js:19972:1)
    at beginWork (react-dom.development.js:21681:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4181:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4233:1)
    at invokeGuardedCallback (react-dom.development.js:4294:1)
    at beginWork$1 (react-dom.development.js:27511:1)
    at performUnitOfWork (react-dom.development.js:26616:1)
2react-dom.development.js:18727 The above error occurred in the <div> component:

    at div
    at div
    at div
    at div
    at div
    at NewFormDisplay (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1384:82)
    at div
    at NewPage (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1564:81)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47602:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:48304:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:48238:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:46179:5)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:49:80)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.


import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import {
    faChevronRight,
    faSignature,
    faIdCard,
    faFileAlt,
    faCalendarAlt,
    faQuestionCircle,
    faInfoCircle
} from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json'; // Make sure the path is correct
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = () => {
    const [formData, setFormData] = useState(null);

    useEffect(() => {
        setFormData(formDataJson);
    }, []);

    const chooseIcon = (key) => {
        switch (key.toLowerCase()) {
            case 'approver1_focuses_on':
                return faInfoCircle;
            case 'approver2_focuses_on':
                return faSignature;
            case 'additional_information':
                return faFileAlt;
            case 'suggested_action_items':
                return faQuestionCircle;
            case 'detailed_summary':
                return faIdCard;
            default:
                return faCalendarAlt;
        }
    };

    const renderFormData = (formKey) => {
        if (!formData || !formData[formKey]) return null;

        const selectedData = formData[formKey];

        return (
            <div className={styles.formDataSection}>
                {Object.entries(selectedData).map(([key, value]) => (
                    <div key={key} className={styles.dataRow}>
                        <h2 className={styles.formDataTitle}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </h2>
                        <p className={styles.formDataContent}>
                            {Array.isArray(value) ? value.join(', ') : value}
                        </p>
                    </div>
                ))}
            </div>
        );
    };

    return (
        <div className={styles.dashboardContainer}>
            <div className={styles.card}>
                <h3>Verification Status</h3>
                {renderFormData('claimassist-history-lambda')}
            </div>

            <div className={styles.card}>
                <h3>Reviewer Insights</h3>
                {renderFormData('claimassist-docextract-lambda')}
            </div>

            {/* Removed Checklist section as it was not present in the JSON */}
        </div>
    );
};

export default NewFormDisplay;




{
  "claimassist-history-lambda": {
    "approver1_focuses_on": [
      "reason for lost policy",
      "date of loss",
      "police report or official documentation to support lost policy claim",
      "details about circumstances of policy loss",
      "copy of government issued photo ID"
    ],
    "approver2_focuses_on": [
      "wet signature on form",
      "matching signatures",
      "specification of date of loss",
      "confirmation of bank account ownership",
      "clarity in circumstances of policy loss",
      "matching account details"
    ],
    "additional_information": [
      "Reason for lost policy with details",
      "Date of loss",
      "Police report for lost policy",
      "Details on circumstances of policy loss",
      "Photo identification of policyholder"
    ],
    "suggested_action_items": [
      "Resubmit form with wet signature",
      "Resign form if signature mismatch",
      "Provide date of loss",
      "Confirm bank account details",
      "Submit additional supporting documents"
    ],
    "detailed_summary": "The verification officers focus on different details to process the claims. Approver1 focuses more on reasons, dates, and documents related to lost policies, while Approver2 focuses on details related to signatures, dates, and account details in forms. Additional details on reasons, dates, documents, and identities would help process similar claims along with suggested actions to resubmit forms with corrections."
  },
  "claimassist-docextract-lambda": {
    "input_payload": {
      "filename": "claimassist/claimforms/CL17254323/1.Filled_L2065777_PIF_and_LPF.pdf",
      "claimid": "CL17254323"
    },
    "output_response": {
      "extracted_data": {
        "payment_instruction_form": {
          "statement_date": "12/06/2024",
          "policy_number": "L2065777",
          "policy_on_the_life_of": "Mr JC Mcglynn",
          "policy_owner": "Mr JC Mcglynn"
        },
        "payment_details": {
          "bank_name_and_address": "BARCLAYS BANK",
          "account_holders_name": "J.C. MC Ghywon",
          "account_number": "50614866",
          "bank_sort_code": "20-57-40",
          "signed_full_name": "Mr Mcglynn, James Christopher",
          "signed_date": "13/06/2024"
        },
        "lost_policy_form": {
          "statement_date": "12/06/2024",
          "policy_number": "L2065777",
          "policy_on_the_life_of": "Mr J C Mcglynn",
          "policy_owner": "Mr JC Mcglynn"
        },
        "lost_policy_form_signed": {
          "full_name": "Mr Mcglynn, James Christopher",
          "date": "13/06/2024"
        },
        "lost_policy_form_witnessed_by": {
          "full_name_of_witness": "SOPHIE PASSFIELD",
          "date": "13/06/2024",
          "address_of_witness": "53 ORNE GARDENS, BOLBECIC PARK, MILTON KEYNES MK18 8PG",
          "official_stamp": "",
          "day_time_telephone_number_of_witness": "07732883 700",
          "occupation_of_witness": "Teacher"
        }
      },
      "raw_text": [
        "claimassist/claimforms/CL17254323/1: 'Retirement, Investments, Insurance, AVIVA... etc.'"
      ],
      "key_values_text": [
        "claimassist/claimforms/CL17254323/1: ['Key: Account holder's name, Value: J.C. MC Ghywon', 'Key: Policy Number:, Value: L2065777', ...etc.]"
      ]
    }
  }
}
