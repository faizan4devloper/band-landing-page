const fetchDocuments = async () => {
  setLoading(true);
  setError(null);

  try {
    const isRefresh = currentPage < 3; // First three pages should refresh

    const payload = {
      action_type: "pagination",
      refresh: isRefresh,
      start_page: currentPage + 1,
      page_size: pageSize
    };

    const response = await axios.post(API_ENDPOINT, payload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("All Data:", response);

    const parsedBody = parseResponse(response.data);

    if (!parsedBody || !parsedBody.data) {
      throw new Error("No data in response");
    }

    const allTransactions = parsedBody.data.flatMap((page) =>
      page.transactions
        .map((transaction) => {
          try {
            return {
              Filename: transaction.Filename?.S || "Unknown",
              Category: transaction.Category?.S || "Unknown",
              DateTime: transaction.DateTime?.S || new Date().toISOString(),
              TransactionID: transaction.TransactionID?.S || "Unknown",
              Metadata: Object.fromEntries(
                Object.entries(transaction.Metadata?.M || {}).map(
                  ([key, value]) => [key, value.S || "N/A"]
                )
              ),
            };
          } catch {
            return null;
          }
        })
        .filter(Boolean)
    );

    setDocuments(allTransactions);
    setNextStartKey(parsedBody.next_start_key || null);
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



useEffect(() => {
  fetchDocuments();
}, [currentPage]);
