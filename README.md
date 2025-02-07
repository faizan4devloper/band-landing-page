const fetchDocuments = async (isRefresh = false, newPage = null) => {
  setLoading(true);
  setError(null);

  try {
    let startPage = newPage !== null ? newPage : currentPage + 1;
    let refresh = startPage <= 3 ? true : false; // First 3 pages refresh, others don't

    const payload = {
      action_type: "pagination",
      refresh: refresh,
      start_page: startPage,
      page_size: 20,
    };

    console.log("Sending Payload:", payload);

    const response = await axios.post(API_ENDPOINT, payload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("API Response:", response.data);

    const parsedBody = parseResponse(response.data);
    if (!parsedBody || !parsedBody.pages) {
      throw new Error("No pages found in response");
    }

    // Extract transactions from the correct page
    const pageKey = `page_${startPage}`;
    const transactions = parsedBody.pages[pageKey] || [];

    setDocuments(transactions);
    setCurrentPage(startPage);
  } catch (err) {
    setError(err.message || "Failed to fetch documents");
    setDocuments([]);
  } finally {
    setLoading(false);
  }
};

const handlePageChange = (direction) => {
  setCurrentPage((prev) => {
    let newPage = direction === "next" ? prev + 1 : Math.max(1, prev - 1);
    fetchDocuments(false, newPage);
    return newPage;
  });
};

const handleRefresh = () => {
  setCurrentPage(1);
  fetchDocuments(true, 1); // Refresh with page 1
};
