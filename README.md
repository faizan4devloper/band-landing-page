import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt, faQuestionCircle, faInfoCircle } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate();

    useEffect(() => {
        setFormData(formDataJson); // Set the entire JSON structure for now
    }, []);

    const handleNextClick = () => {
        navigate('/New-page');
    };

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

    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.gridContainer}>
                {Object.entries(selectedData).map(([key, value]) => (
                    <div key={key} className={styles.card}>
                        <div className={styles.cardHeader}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </div>
                        <div className={styles.cardContent}>
                            {Array.isArray(value) ? value.join(', ') : value}
                        </div>
                    </div>
                ))}

                
            </div>
        );
    };

    return <div className={styles.formDisplay}>{renderFormData()}</div>;
};

export default NewFormDisplay;












// src/components/Dashboard.js
import React from 'react';
import ClaimHistory from './ClaimHistory';
import DocExtract from './DocExtract';
import SuggestedActions from './SuggestedActions';
import Summary from './Summary';
import styles from './Dashboard.module.css';

const Dashboard = ({ claimData }) => {
  return (
    <div className={styles.dashboard}>
      <h1>Claim Assist Dashboard</h1>
      <ClaimHistory data={claimData['claimassist-history-lambda']} />
      <DocExtract data={claimData['claimassist-docextract-lambda']} />
      <SuggestedActions data={claimData['claimassist-history-lambda'].Suggested_Action_Items} />
      <Summary summary={claimData['claimassist-history-lambda'].Detailed_Summary} />
    </div>
  );
};

export default Dashboard;


// src/components/ClaimHistory.js
import React from 'react';
import styles from './ClaimHistory.module.css';

const ClaimHistory = ({ data }) => {
  return (
    <div className={styles.claimHistory}>
      <h2>Claim History</h2>
      <div>
        <h3>Approver 1 Focuses On:</h3>
        <ul>
          {data.Approver1_Focuses_On.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
      </div>
      <div>
        <h3>Approver 2 Focuses On:</h3>
        <ul>
          {data.Approver2_Focuses_On.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default ClaimHistory;


// src/components/DocExtract.js
import React from 'react';
import styles from './DocExtract.module.css';

const DocExtract = ({ data }) => {
  const { ExtractedData } = data.Output_Response;

  return (
    <div className={styles.docExtract}>
      <h2>Document Extraction</h2>
      <h3>Payment Instruction Form</h3>
      <p>Policy Number: {ExtractedData.PAYMENT_INSTRUCTION_FORM.POLICY_NUMBER}</p>
      <p>Policy Owner: {ExtractedData.PAYMENT_INSTRUCTION_FORM.POLICY_OWNER}</p>
      <h3>Payment Details</h3>
      <p>Bank Name: {ExtractedData.PAYMENT_DETAILS.BANK_NAME_AND_ADDRESS}</p>
      <p>Account Holder's Name: {ExtractedData.PAYMENT_DETAILS.ACCOUNT_HOLDERS_NAME}</p>
    </div>
  );
};

export default DocExtract;


// src/components/SuggestedActions.js
import React from 'react';
import styles from './SuggestedActions.module.css';

const SuggestedActions = ({ data }) => {
  return (
    <div className={styles.suggestedActions}>
      <h2>Suggested Actions</h2>
      <ul>
        {data.map((action, index) => (
          <li key={index}>{action}</li>
        ))}
      </ul>
    </div>
  );
};

export default SuggestedActions;


// src/components/Summary.js
import React from 'react';
import styles from './Summary.module.css';

const Summary = ({ summary }) => {
  return (
    <div className={styles.summary}>
      <h2>Detailed Summary</h2>
      <p>{summary}</p>
    </div>
  );
};

export default Summary;


.dashboard {
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}




.claimHistory {
  margin-bottom: 20px;
  padding: 10px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
}

.claimHistory h2 {
  color: #333;
}

.claimHistory ul {
  list-style-type: disc;
  padding-left: 20px;
}



.docExtract {
  margin-bottom: 20px;
  padding: 10px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
}



.suggestedActions {
  margin-bottom: 20px;
  padding: 10px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
}



.summary {
  margin-bottom: 20px;
  padding: 10px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
}


// src/App.js
import React from 'react';
import Dashboard from './components/Dashboard';

const claimData = {
  "claimassist-history-lambda": { /* your data */ },
  "claimassist-docextract-lambda": { /* your data */ }
};

const App = () => {
  return (
    <div>
      <Dashboard claimData={claimData} />
    </div>
  );
};

export default App;
