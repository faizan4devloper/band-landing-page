emailbody
: 
claimid
: 
"CL1234567"
emailbody
: 
"    \n    Dear l c k @LCL\n    Individual Critical Illness Claim - Claim Decision (Our Ref: ref_no_PS391481)\n    Policy Number : PI4610352\n    Plan : Critical Care Plan\n    Life Assured : 25 Years\n    \n    Thank you for your patience and we'd like to inform you that we've approved the Early and Intermediate Stage Critical Illness Benefit (Layer 1, Pot 1) of the MultiPay Critical Illness Cover.\n    \n    We will pay 100% of the MultiPay Critical Illness Benefit Sum Assured. The payout from this benefit does not reduce the Basic Benefit Sum Assured. The Monthly premium of SGD 75.95 remain payable.\n    \n    The amount payable is computed as follows:\n\n    Early and Intermediate Stage Critical Illness Benefit (Layer 1, Pot 1): SGD 50,000.00\n    \n    Total Amount Payable: SGD 50,000.00\n    \n    We've arranged to direct credit the payment for the said Benefit to your stipulated account. The acceptance of the payment shall serve as a final discharge to Singapore Life Ltd. and this claim shall terminate all other rights, values and benefits of the said Benefit under the Policy.\n\n    To access more policy and payment details, view all policy-related correspondence and update your contact information conveniently, please log in to your MySinglife account or sign up for one at https://mysinglife.singlife.com.\n    \n    If you've any questions regarding this claim, you can reach to us at SGlifeclaims_support@singlife.com. Alternatively, you can contact your Financial Adviser Representative.\n    \n    Yours sincerely\n    Head of Individual Life Claims\n    Customer Service\n    \n    "
[[Prototype]]
: 
Object
[[Prototype]]
: 
Object



import React, { useState } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css";

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [draftLoading, setDraftLoading] = useState(false);
  const [error, setError] = useState(null);
  const [draftError, setDraftError] = useState(null);
  const [responseData, setResponseData] = useState(null);
  const [draftEmail, setDraftEmail] = useState("");

  // API payloads
  const draftPayload = {
    claimid : "CL1234567", 
      recnumber: "PS391481",
      tasktype : "FETCH_EMAIL",


};

  const generatePayload = {
    claimid: "CL123456",
    recnumber: "PS391481",
    tasktype: "GENERATE_EMAIL",
  };

  // Fetch draft email
  const handleFetchDraftEmail = async () => {
    setDraftLoading(true);
    setDraftError(null);

    try {
      const Emailresponse = await axios.post("", draftPayload, {
        headers: { "Content-Type": "application/json" },
      });
      

      

      console.log("Draft Email:", Emailresponse.data);
    } catch (err) {
      setDraftError("Failed to fetch draft email");
      console.error("Draft Email Error:", err);
    } finally {
      setDraftLoading(false);
    }
  };

  // Generate email with LLM
  const handleGenerateEmail = async () => {
    setLoading(true);
    setError(null);

    try {
      const LLMresponse = await axios.post("your-generate-email-api-endpoint", generatePayload, {
        headers: { "Content-Type": "application/json" },
      });

      setResponseData(LLMresponse.data);
      console.log("LLMresponse Email Response:", LLMresponse.data);
    } catch (err) {
      setError("Failed to generate email");
      console.error("Generate Email Error:", err);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className={styles.container}>
      {/* Left Section */}
      <div className={styles.leftSection}>
        <div className={styles.sectionWindow}>
          <h2>Claim Summary</h2>
          <p>Details about the claim go here.</p>
        </div>
        <div className={styles.sectionWindow}>
          <h2>Recommendations</h2>
          <p>Suggestions based on the claim details go here.</p>
        </div>
      </div>

      {/* Right Section */}
      <div className={styles.rightSection}>
        <div className={`${styles.sectionWindow} ${styles.draftEmail}`}>
          <h2>Draft Email</h2>
          {draftLoading ? (
            <p>Loading draft email...</p>
          ) : draftError ? (
            <p className={styles.errorText}>{draftError}</p>
          ) : (
            <p>{draftEmail}</p>
          )}
          <button
            className={styles.fetchButton}
            onClick={handleFetchDraftEmail}
            disabled={draftLoading}
          >
            {draftLoading ? "Fetching Draft..." : "Generate Draft Email"}
          </button>
        </div>
        <div
          className={`${styles.sectionWindow} ${responseData ? styles.expanded : ""}`}
        >
          <h2>LLM Response</h2>
          <button
            className={styles.generateButton}
            onClick={handleGenerateEmail}
            disabled={loading}
          >
            {loading ? "LLMresponse Email..." : "LLMresponse Email"}
          </button>
          {error && <p className={styles.errorText}>{error}</p>}
          {responseData && (
            <p className={styles.successText}>
              Response: {JSON.stringify(responseData)}
            </p>
          )}
        </div>
        <button className={styles.submitButton}>Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;
