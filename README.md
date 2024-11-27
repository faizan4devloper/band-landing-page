const handleVerify = async (recNum) => {
  setIsLoading(true);
  try {
    const payload = {
      tasktype: "VERIFY_CLAIM",
      claimid: recNum,
      PSID: "PS391481", // Using recNum from the first row as an example
    };

    const response = await axios.post("dummy", payload, {
      headers: { "Content-Type": "application/json" },
    });

    // Parse the 'body' from the response data
    const body = JSON.parse(response.data.verifyclaimactdata.body);

    // Extract SUMMARY_DETAILS and DETAILED_SUMMARY
    const summaryDetails = body.SUMMARY_DETAILS;
    const detailedSummary = summaryDetails.DETAILED_SUMMARY;

    // Extracting other summary details if needed
    const briefSummary = summaryDetails.CLAIM_FORM_BRIEF_SUMMARY;
    const claimType = summaryDetails.CLAIM_FORM_TYPE;
    const claimStatus = summaryDetails.CLAIM_STATUS;

    // Navigate to Verify page and pass summary and recommendation (detailed summary)
    navigate("/verify", {
      state: {
        summary: {
          briefSummary,
          claimType,
          claimStatus,
        },
        recommendation: detailedSummary,
      },
    });
  } catch (error) {
    setError("Failed to fetch data.");
    console.error(error);
  } finally {
    setIsLoading(false);
  }
};




import { useLocation } from "react-router-dom";

const Verify = () => {
  const location = useLocation();
  const { summary, recommendation } = location.state;

  return (
    <div>
      {/* Left side: Summary */}
      <div>
        <h3>Claim Summary</h3>
        <p><strong>Brief Summary:</strong> {summary.briefSummary}</p>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>

      {/* Right side: Recommendations */}
      <div>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>
    </div>
  );
};
