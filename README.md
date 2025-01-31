const fetchDocuments = async (isRefresh = false) => {
  setLoading(true);
  setError(null);

  try {
    let payload;

    if (isRefresh || currentPage < 3) {
      // Refresh should be true for the first three pages
      payload = {
        action_type: "pagination",
        refresh: true,
        start_page: currentPage + 1,
        page_size: 20,
      };
    } else {
      // Refresh should be false for pages 4, 5, 6, and onwards
      payload = {
        action_type: "pagination",
        refresh: false,
        start_page: currentPage + 1,
        page_size: 20,
      };
    }

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



const handlePageChange = (direction) => {
  if (direction === "next" && documents.length > 0) {
    setCurrentPage((prev) => prev + 1);
  } else if (direction === "prev" && currentPage > 0) {
    setCurrentPage((prev) => Math.max(0, prev - 1));
  }
};

const handleRefresh = () => {
  fetchDocuments(true); // Pass true to ensure the refresh logic applies
};
