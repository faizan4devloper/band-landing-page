const handleVerify = () => {
  if (rows.length > 0) {
    navigate("/verify", {
      state: { recNum: rows[0]?.recNum }, // Dynamically pass recNum
    });
  } else {
    console.error("No rows available to verify.");
  }
};
