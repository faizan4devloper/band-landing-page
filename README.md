const handleReload = async (rec_number) => {
  try {
    const payload = {
      tasktype: "FETCH_SINGLE_CLAIM",
      claimid: rec_number,
    };

    const response = await axios.post(`dummy`, payload, {
      headers: { "Content-Type": "application/json" },
    });

    const singleclaimdata = response.data;
    console.log("Reload Data:", singleclaimdata);

    setRows((prevRows) =>
      prevRows.map((row) =>
        row.rec_number === rec_number
          ? {
              ...row,
              ...singleclaimdata, // Spread singleclaimdata to update row
            }
          : row
      )
    );
  } catch (error) {
    console.error("Error fetching data for:", rec_number, error);
  }
};




<tbody>
  {rows.map((row, index) => (
    <tr key={index}>
      <td>{row.rec_number}</td>
      <td>{row.policy_id || "Pending"}</td>
      <td>{row.prod_sheet_type || "Pending"}</td>
      <td>{row.summary || "Pending"}</td>
      <td>{row.file_name ? <a href={row.file_name}>Download</a> : "N/A"}</td>
      <td>{row.status || "Pending"}</td>
    </tr>
  ))}
</tbody>
