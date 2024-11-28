const handleVerify = (recNum) => {
  navigate("/verify", {
    state: { recNum }, // Pass recNum in navigation state
  });
};




<div className={styles.verifyButtonContainer}>
  {rows.length > 0 && (
    <button
      className={styles.verifyButton}
      onClick={() => handleVerify(rows[0].recNum)} // Pass the first row's recNum
    >
      Verify
    </button>
  )}
</div>




useEffect(() => {
  const fetchData = async (recNum) => {
    try {
      const payload = {
        tasktype: "VERIFY_CLAIM",
        claimid: recNum,
      };

      const headers = {
        "Content-Type": "application/json",
      };

      const response = await axios.post(
        "dummy", // Replace with your actual endpoint
        payload,
        { headers }
      );

      const responseBody = JSON.parse(response.data.verifyclaimactdata.body);
      const { CLAIM_FORM_BRIEF_SUMMARY, CLAIM_FORM_TYPE, CLAIM_STATUS, DETAILED_SUMMARY } =
        responseBody.SUMMARY_DETAILS;

      setSummary({
        briefSummary: CLAIM_FORM_BRIEF_SUMMARY,
        claimType: CLAIM_FORM_TYPE,
        claimStatus: CLAIM_STATUS,
      });
      setRecommendation(DETAILED_SUMMARY);
    } catch (error) {
      setError("Failed to load data");
      console.error(error);
    } finally {
      setLoading(false);
    }
  };

  if (location.state && location.state.recNum) {
    fetchData(location.state.recNum); // Use recNum from location.state
  } else {
    setError("No recNum provided");
    setLoading(false);
  }
}, [location.state]);
