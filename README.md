const handleVerify = () => {
  if (rows.length > 0) {
    navigate("/verify", {
      state: { recNum: rows[0]?.recNum }, // Dynamically pass recNum
    });
  } else {
    console.error("No rows available to verify.");
  }
};



useEffect(() => {
  const fetchData = async () => {
    const claimId = location.state?.recNum;
    if (!claimId) {
      console.error("recNum is missing, cannot fetch data.");
      return;
    }

    try {
      const payload = { tasktype: "VERIFY_CLAIM", claimid: claimId };
      const response = await axios.post("dummy", payload);
      console.log("Verify API Response:", response.data);
      // Process response as needed...
    } catch (error) {
      console.error("Error fetching claim data:", error);
    }
  };

  fetchData();
}, [location.state?.recNum]);
