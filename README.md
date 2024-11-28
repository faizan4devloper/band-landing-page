useEffect(() => {
  const fetchData = async (recNum, psid = "PS485817") => {
    try {
      // Prepare your payload
      const payload = {
        tasktype: "VERIFY_CLAIM",
        claimid: recNum, // Ensure recNum is passed correctly
        psid: psid,
      };

      // Custom headers
      const headers = {
        "Content-Type": "application/json",
      };

      // API call
      const response = await axios.post(
        "dummy", // Replace with your actual endpoint
        payload,
        { headers }
      );

      console.log("Summary:", response.data);

      // Parse response body
      const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

      // Extract and set data
      const { CLAIM_FORM_BRIEF_SUMMARY, CLAIM_FORM_TYPE, CLAIM_STATUS, DETAILED_SUMMARY } = responseBody.SUMMARY_DETAILS;

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

  // Extract recNum from location.state or set a default value
  const recNum = location.state?.recNum || "defaultRecNum"; // Replace with actual default value

  // Fetch data if location.state is not set
  if (!location.state) {
    fetchData(recNum);
  } else {
    const { summary, recommendation } = location.state;
    setSummary(summary);
    setRecommendation(recommendation);
    setLoading(false);
  }
}, [location.state]);
