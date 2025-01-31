const handlePageChange = (direction) => {
  setCurrentPage((prev) => {
    let newPage = direction === "next" ? prev + 3 : prev - 3;
    return Math.max(1, newPage); // Ensures it never goes below 1
  });
};



const fetchDocuments = async (isRefresh = false) => {
  setLoading(true);
  setError(null);

  try {
    let startPage = isRefresh ? 1 : currentPage;

    // Enforce multiples of 3 for pagination
    if (startPage > 1) {
      startPage = Math.floor((startPage - 1) / 3) * 3 + 1;
    }

    const payload = {
      action_type: "pagination",
      refresh: isRefresh, 
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
