 const testApiCall = async () => {
    try {
      const payload = {
        tasktype: "FETCH_TEST_DATA", // Replace with your API task type
        claimid: "TEST_ID", // Replace with your test claim ID
      };

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("Test API Response:", response.data); // Log the API response
      setTestResponse(response.data); // Save response for further debugging if needed
    } catch (error) {
      console.error("Test API call failed:", error);
    }
  };
