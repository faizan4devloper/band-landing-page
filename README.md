const handlePageChange = (direction) => {
  setCurrentPage((prev) => {
    if (direction === "next") return prev + 3;  // Moves to 4 → 7 → 10
    if (direction === "prev") return Math.max(1, prev - 3); // Moves back safely
    return prev;
  });
};


const handlePageChange = (direction) => {
  setCurrentPage((prev) => {
    if (direction === "next") return prev + 3;  // Moves to 4 → 7 → 10
    if (direction === "prev") return Math.max(1, prev - 3); // Moves back safely
    return prev;
  });
};



const fetchDocuments = async (isRefresh = false) => {
  setLoading(true);
  setError(null);

  try {
    let startPage = currentPage;

    // Adjust start_page logic to enforce multiples of 3 (1, 4, 7, ...)
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
    setCurrentPage(startPage); // Ensure state update
  } catch (err) {
    setError(err.message || "Failed to fetch documents");
    setDocuments([]);
  } finally {
    setLoading(false);
  }
};
