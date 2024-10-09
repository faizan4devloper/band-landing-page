// **************************************************************************************
// ************************Lambda Name: claimassist-history-lambda*******************
// **************************************************************************************

{
    "Approver1_Focuses_On": [
        "reason for lost policy",
        "date of loss",
        "police report or official documentation to support lost policy claim",
        "details about circumstances of policy loss",
        "copy of government issued photo ID"
    ],
    "Approver2_Focuses_On": [
        "wet signature on form",
        "matching signatures",
        "specification of date of loss",
        "confirmation of bank account ownership",
        "clarity in circumstances of policy loss",
        "matching account details"
    ],
    "Additional_Information": [
        "Reason for lost policy with details",
        "Date of loss",
        "Police report for lost policy",
        "Details on circumstances of policy loss",
        "Photo identification of policyholder"
    ],
    "Suggested_Action_Items": [
        "Resubmit form with wet signature",
        "Resign form if signature mismatch",
        "Provide date of loss",
        "Confirm bank account details",
        "Submit additional supporting documents"
    ],
    "Detailed_Summary": "The verification officers focus on different details to process the claims. Approver1 focuses more on reasons, dates and documents related to lost policies while Approver2 focuses on details related to signatures, dates and account details in forms. Additional details on reasons, dates, documents and identities would help process similar claims along with suggested actions to resubmit forms with corrections."
}

// **************************************************************************************
// ************************Lambda Name: claimassist-docextract-lambda*******************
// **************************************************************************************


// Input Payload

filename=claimassist%2Fclaimforms%2FCL17254323%2F1.Filled_L2065777+PIF+and+LPF.pdf&claimid=CL17254323

// Output Response

{
    "ExtractedData": {
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
    "rawtext": [
      {
        "claimassist/claimforms/CL17254323/1": "6/25/24, 2:23 PM\nabout:blank\nRetirement\nInvestments\n17 JUN 2024\nInsurance\nAVIVA\nHealth\nPayment Instruction Form\nStatement date: 12 June 2024\nPolicy Number:\nL2065777\nPolicy on the life of:\nMr JC Mcglynn\nPolicy Owner:\nMr JC Mcglynn\nBl/We confirm:\nI/\nWe request Aviva Life & Pensions UK Limited to pay me/ the cash value of this policy as provided for in the\nPolicy Provisions and Conditions.\n1/the certify that I/WO are entitled to the proceeds of the Policy and that the policy has not been assigned or\ntransferred, nor has any other person rights to the Policy, or proceeds thereof.\nIn return for this payment of the policy by Aviva Life & Pensions UK Limited, I / AGG.\ndischarge Aviva Life & Pensions UK Limited from all liability under the policy.\nindemnify Aviva Life & Pensions UK Limited against all future liability under the policy.\nI/W return/have already returned the policy to Aviva Life & Pensions UK\nLimited.\nPayment details:\nBank name and address\nBARCLAYS BANK\nAccount holder's name\nAccount number\nBank sort code\nJ.C.MC Ghywon\n50614866\n20-57-40\nOr\nNOT\nThe address the cheque is to be sent to:\nThe cheque is to be made payable to:\n5 INSTONE\nBRADWELL COMMON MILTON\nBucks MK 13 8BA\nJCMSGinya\n(please print name)\nPlease ensure the section entitled \"Signed\" is fully completed or we will not be able to accept this form.\nSigned\nFull, Name/s\nSignature\nDate\nMr Mcglynn, James Christopher\nAlues\n13/6/24\n(please provide Position & Office stamp if assigned)\nNotes on filling in the form\nIf the policy is assigned the assignee must sign this form.\nIf the policy is in Trust, each Trustee must sign this form.\nIf the policy is jointly owned, both policy owners must sign this form.\nAviva Life & Pensions UK Limited, PO Box 2617, Romford RM1 3WF Telephone: 0345 070 7099 Fax: 0345 602 6035 calls may be recorded.\nRegistered in England No. 3253947. Registered office: Aviva, Wellington Row. York, YO90 1WR.\nAuthorised by the Prudential Regulation Authority and regulated by the Financial Conduct Authority and the Prudential Regulation Authority. Firm Reference Number 185896.\n17Jun24_19_LB0872_Image187\nabout:blank\n1/4\n6/25/24, 2:23 PM\nabout:blank\n17Jun24_19_LB0872_Image\n188\nabout:blank\n2/4\n6/25/24, 2:23 PM\nabout:blank\nRetirement\nInvestments\nInsurance\nAVIVA\nHealth\nLost Policy Form\nStatement date: 12 June 2024\nPolicy Number:\nL2065777\nPolicy on the life of:\nMr J C Mcglynn\nPolicy Owner:\nMr JC Mcglynn\nH We confirm:\n/\nwe\nhave searched for this policy document\n1 the have asked others who might have the policy document - such as solicitors, accountants and bankers\nconfirm that the policy has not been assigned or deposited with any other party who can claim an\nentitlement to the proceeds of the policy\n14th believe that the policy must be lost or destroyed\nIf it is found in the future, I / the will return it straight away\nSigned\nDate\njungs- Signature\nFull Name\nMr Mcglynn, James Christopher\n13/6/24\nWitnessed by: (Please see attached list of Acceptable Signature Witnesses)\nSignature of Witness (Cannot\nFull Name , Position and Profession of\nDate\nbe a retired person)\nWitness !\n&Passield\nSOPHIE PASSFIELD\n13/6/24\nAddress of Witness\n53 ORNE GARDENS, BOLBECIC PARK, MILTON KEYNES\nMKIS 8PG\nOfficial Stamp\nDay-time telephone number of Witness\nOccupation of Witness\n07732883 700\nTeacher\nI confirm the above named client has signed this form in front of me and I can certify it is their true signature.\nAviva Life & Pensions UK Limited, PO Box 2617, Romford RM1 3WF Telephone: 0345 070 7099 Fax: 0345 602 6035 calls may\nbe\nrecorded.\nRegistered in England No. 3253947. Registered office: Aviva, Wellington Row. York, Y090 1WR\nAuthorised by the Prudential Regulation Authority and regulated by the Financial Conduct Authority and the Prudential Regulation Authority. Firm Reference Number 185896.\nRef: L01 Page 4\n17Jun24_19_LB0872_Image 189\nabout:blank\n3/4\n6/25/24, 2:23 PM\nabout:blank\n17Jun24_19_LB0872_Image\n190\nabout:blank\n4/4\n"
      }
    ],
    "keyvaluesText": "[{'claimassist/claimforms/CL17254323/1': ['Key: (please print name), Value: None', \"Key: Account holder's name, Value: J.C.MC Ghywon\", 'Key: PO, Value: Box', 'Key: Account number, Value: 50614866', 'Key: Full, Name/s, Value: Mr Mcglynn, James Christopher', 'Key: Policy Number:, Value: L2065777', 'Key: Bank name and address, Value: BARCLAYS BANK', 'Key: Statement date:, Value: 12 June 2024', 'Key: The cheque is to be made payable to:, Value: JCMSGinya', 'Key: Policy Owner:, Value: Mr JC Mcglynn', 'Key: The address the cheque is to be sent to:, Value: 5 INSTONE BRADWELL COMMON MILTON Bucks MK 13 8BA', 'Key: Signature, Value: Alues (please provide Position & Office stamp', 'Key: Bank sort code, Value: 20-57-40', 'Key: Date, Value: 13/6/24', 'Key: Policy on the life of:, Value: Mr JC Mcglynn', 'Key: Fax:, Value: 0345 602 6035', 'Key: I/W return/have already returned the policy to Aviva Life & Pensions UK Limited., Value: NOT_SELECTED', 'Key: Notes on filling in the form, Value: If the policy is assigned the assignee must sign this form. If the policy is in Trust, each Trustee must sign this form. If the policy is jointly owned, both policy owners must sign this form.', 'Key: Row., Value: York, YO90 1WR.', 'Key: 17, Value: JUN 2024', 'Key: Telephone:, Value: 0345 070 7099', 'Key: calls may be, Value: recorded.', 'Key: Firm Reference Number, Value: 185896.', 'Key: about:blank, Value: None', 'Key: about:blank, Value: None', 'Key: 14th believe that the policy must be lost or destroyed If it is found in the future, I / the will return it straight away, Value: None', 'Key: Full Name , Position and Profession of Witness !, Value: SOPHIE PASSFIELD', 'Key: Full Name, Value: Mr Mcglynn, James Christopher', 'Key: Signature of Witness (Cannot be a retired person), Value: &Passield', 'Key: Date, Value: 13/6/24', 'Key: Address of Witness, Value: 53 ORNE GARDENS, BOLBECIC PARK, MILTON KEYNES MKIS 8PG', 'Key: Day-time telephone number of Witness, Value: 07732883 700', 'Key: Date, Value: 13/6/24', 'Key: Occupation of Witness, Value: Teacher', 'Key: Statement date:, Value: 12 June 2024', 'Key: Official Stamp, Value: None', 'Key: Telephone:, Value: 0345 070 7099', 'Key: Fax:, Value: 0345 602 6035', 'Key: Policy on the life of:, Value: Mr J C Mcglynn', 'Key: Signature, Value: jungs-', 'Key: Policy Owner:, Value: Mr JC Mcglynn', 'Key: Policy Number:, Value: L2065777', 'Key: Row. York,, Value: Y090 1WR', 'Key: Firm Reference Number, Value: 185896.', 'Key: Ref:, Value: L01', 'Key: H We confirm:, Value: / we have searched for this policy document 1 the have asked others who might have the policy document - such as solicitors, accountants and bankers confirm that the policy has not been assigned or deposited with any other party who can claim an entitlement to the proceeds of the policy', 'Key: about:blank, Value: None', 'Key: about:blank, Value: 4/4']}]",
    "tbltxt": "[{'claimassist/claimforms/CL17254323/1': [{'0': 'Tabluar Data Processing ;Policy Number:  Policy on the life of: ,L2065777  Mr JC Mcglynn ,;Policy Number:  Policy Owner: ,L2065777  Mr JC Mcglynn ,;'}, {'1': \"Tabluar Data Processing ;Account holder's name  J.C.MC Ghywon ,Account number  50614866 ,Bank sort code  20-57-40 ,;\"}, {'2': 'Tabluar Data Processing ;Policy Number:  Policy on the life of: ,L2065777  Mr J C Mcglynn ,;Policy Number:  Policy Owner: ,L2065777  Mr JC Mcglynn ,;'}, {'3': 'Tabluar Data Processing ;Signature  jungs- ,Full Name  Mr Mcglynn, James Christopher ,description:- ,Date  13/6/24 ,;'}, {'4': 'Tabluar Data Processing ;Signature of Witness (Cannot be a retired person)  &Passield ,Full Name , Position and Profession of Witness !  SOPHIE PASSFIELD ,Date  13/6/24 ,;'}]}]"
  }



  import React, { useState } from 'react';
import styles from './Sidebar.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onSelectForm }) => {
    const [activeItem, setActiveItem] = useState('')
    
    const handleFormSelect = (formName) => {
        setActiveItem(formName);
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Select a Form</h3>
            <ul className={styles.formList}>
                <li
                    className={`${styles.formItem} ${activeItem === 'PAYMENT_INSTRUCTION_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_INSTRUCTION_FORM')}
                >
                    <span className={styles.formText}>Verification Status</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_DETAILS')}
                >
                    <span className={styles.formText}>Reviewer Insights</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('LOST_POLICY_FORM')}
                >
                    <span className={styles.formText}>Checklist</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                
            </ul>
        </div>
    );
};

export default Sidebar;





import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom'; // Import useNavigate for navigation
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/DocumentEntities.json'; // Import the JSON file
import styles from './FormDisplay.module.css';

const FormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate(); // Create navigate function

    useEffect(() => {
        // Simulate fetching data from a JSON file
        setFormData(formDataJson.extracted_data);
    }, []);

    // Function to handle "Next" button click
    const handleNextClick = () => {
        // Navigate to the Verification page
        navigate('/New-page');
    };

    // Render form data based on selected form in a table format
    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.tableContainer}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>

                <button className={styles.nextBtn} onClick={handleNextClick}> {/* Add onClick */}
                    Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon}/>
                </button>

                <table className={styles.dataTable}>
                    <thead>
                        <tr>
                            <th className={styles.tableHeader}>Field</th>
                            <th className={styles.tableHeader}>Value</th>
                        </tr>
                    </thead>
                    <tbody>
                        {Object.entries(selectedData).map(([key, value]) => (
                            <tr key={key} className={styles.tableRow}>
                                <td className={styles.tableCell}>
                                    {key.replace(/_/g, ' ')}
                                </td>
                                <td className={styles.tableCell}>
                                    {value}
                                </td>
                            </tr>
                        ))}
                    </tbody>
                </table>
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default FormDisplay;
