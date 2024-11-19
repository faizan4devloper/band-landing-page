const handleReload = async (recNum) => {
  try {
    // Define the payload with the required properties
    const payload = {
      recNum, // Add the RecNum value dynamically
      fileType: file ? file.type : "application/json", // Example: dynamic or default type
      filename: file ? file.name : "example.txt", // Example: dynamic or default filename
    };

    // Make the API call with the payload
    const response = await axios.post(`dummy1`, payload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("Reload Data:", recNum, response.data);
    const data = response.data;

    // Update the row with the fetched data
    setRows((prevRows) =>
      prevRows.map((row) =>
        row.recNum === recNum
          ? {
              ...row,
              policyId: data.policyId,
              productSheetType: data.productSheetType,
              summary: data.summary,
              status: "Completed",
            }
          : row
      )
    );
  } catch (error) {
    setMessage("Failed to fetch data for RecNum: " + recNum);
    console.error("Reload Error:", error);
  }
};
