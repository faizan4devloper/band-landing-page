const handleReload = async (recNum) => {
  try {
    const payload = {
      tasktype: "FETCH_SINGLE_CLAIM",
      claimid: recNum,
    };

    const response = await axios.post("dummy", payload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("API Response:", response.data); // To see the full structure

    const singleclaimdata = response.data; // Assuming the data is directly in response.data

    // Check if singleclaimdata has data and the first element exists
    if (singleclaimdata && singleclaimdata[0]) {
      const claimData = singleclaimdata[0]; // Get the first element

      // Log the claim data to verify
      console.log("Claim Data:", claimData);

      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policyid: claimData.policy_id,
                prod_sheet_type: claimData.prod_sheet_type,
                summary: claimData.summary,
                status: claimData.status,
                previewLink: claimData.file_name || row.previewLink, // Assuming previewLink is file_name
              }
            : row
        )
      );
    } else {
      console.error("No data found for recNum:", recNum);
    }
  } catch (error) {
    console.error("Error in handleReload:", error);
    setMessage("Failed to fetch data for RecNum: " + recNum);
  }
};
