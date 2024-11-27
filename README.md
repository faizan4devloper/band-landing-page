const handleVerify = async () => {
  setIsLoading(true);
  try {
    // Ensure the payload does not include any React or DOM elements.
    const payload = {
      tasktype: "FETCH_SINGLE_ACT_CLAIM",
      claimid: rows[0]?.recNum,  // Using recNum from the first row as an example
    };

    // Make the API call
    const response = await axios.post("dummy-api-endpoint", payload, {
      headers: { "Content-Type": "application/json" },
    });

    // Assuming the response has summary and recommendations data
    const { summary, recommendations } = response.data;

    // Navigate to Verify page and pass data
    navigate("/verify", {
      state: { summary, recommendations },
    });
  } catch (error) {
    setError("Failed to fetch data.");
    console.error(error);
  } finally {
    setIsLoading(false);
  }
};
