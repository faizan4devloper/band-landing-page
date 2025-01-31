const fetchDocuments = async () => {
  setLoading(true);
  setError(null);

  try {
    const isRefresh = currentPage + 1 <= 3; // Refresh only for the first 3 pages

    const payload = {
      action_type: "pagination",
      refresh: isRefresh, 
      start_page: currentPage + 1,
      page_size: pageSize || 20 // Ensure pageSize is always set
    };

    const response = await axios.post(API_ENDPOINT, payload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("All Data:", response.data);

    const parsedBody = parseResponse(response.data);

    if (!parsedBody || !parsedBody.pages) {
      throw new Error("No pages found in response");
    }

    // Extract transactions from the correct page structure
    const pageKey = `page_${currentPage + 1}`;
    const transactions = parsedBody.pages[pageKey] || [];

    const allTransactions = transactions.map((transaction) => ({
      Filename: transaction.Filename || "Unknown",
      DateTime: transaction.DateTime || new Date().toISOString(),
      TransactionID: transaction.TransactionID || "Unknown",
      Status: transaction.Status || "N/A",
      Metadata: transaction.Metadata || {},
    }));

    setDocuments(allTransactions);
  } catch (err) {
    setError(err.message || "Failed to fetch documents");
    setDocuments([]);
  } finally {
    setLoading(false);
  }
};
