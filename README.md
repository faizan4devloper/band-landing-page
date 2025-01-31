const fetchDocuments = async (isRefresh = false, pageNumber = currentPage) => {
  setLoading(true);
  setError(null);

  try {
    let startPage = pageNumber;

    // Ensure pagination moves in multiples of 3 (1, 4, 7, ...)
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

    const pageKey = `page_${startPage}`;
    const transactions = parsedBody.pages[pageKey] || [];

    setDocuments(transactions);
    setCurrentPage(startPage); // Ensure state update
  } catch (err) {
    setError(err.message || "Failed to fetch documents");
    setDocuments([]);
  } finally {
    setLoading(false);
  }
};
