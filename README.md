useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await axios.post("dummy1", {
        tasktype: "FETCH_ALL_ACT_CLAIMS",
      });

      console.log("API Response:", response.data);

      // Validate and set the data
      if (response.data.allclaimactdata) {
        setClaimsData(response.data.allclaimactdata);
        console.log("Setting claimsData:", response.data.allclaimactdata);
      } else {
        throw new Error("Invalid API response structure");
      }
    } catch (err) {
      console.error("Error fetching data:", err);
      setError("Failed to fetch claims data.");
    } finally {
      setLoading(false);
    }
  };

  fetchData();
}, []);




const AllDataTable = ({ data = [] }) => {
  console.log("Received data in AllDataTable:", data);

  if (!data.length) {
    return <p>No claims data available.</p>;
  }

  return (
    <table>
      <thead>
        <tr>
          <th>Claim ID</th>
          <th>Claim Type</th>
          <th>Summary</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        {data.map((claim, index) => (
          <tr key={index}>
            <td>{claim.claimid}</td>
            <td>{claim.claimtype}</td>
            <td>{claim.briefsummary}</td>
            <td>{claim.status}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
};
