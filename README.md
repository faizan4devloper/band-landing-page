const handleReload = async (recNum) => {
  try {
    // Fetch the data for the specific `recNum`
    const response = await fetch(`/api/getClaimData?recNum=${recNum}`);
    const data = await response.json();

    // Log the response to check its structure
    console.log("Reload Data:", data);

    // Assume data structure matches: singleclaimdata: { ...fields }
    const { singleclaimdata } = data;

    // Update the rows state
    setRows((prevRows) =>
      prevRows.map((row) =>
        row.recNum === recNum
          ? {
              ...row,
              policyid: singleclaimdata.policy_id || row.policyid,
              type: singleclaimdata.prod_sheet_type || row.type,
              summary: singleclaimdata.summary || row.summary,
              status: singleclaimdata.status || row.status,
            }
          : row
      )
    );
  } catch (error) {
    console.error("Error reloading data:", error);
  }
};
