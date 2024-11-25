const handleReload = async (recNum) => {
  try {
    const payload = {
      tasktype: "FETCH_SINGLE_CLAIM",
      claimid: recNum,
    };

    const response = await axios.post("your-api-url", payload, {
      headers: { "Content-Type": "application/json" },
    });

    // Assuming response.data contains the data for a single claim
    const singleclaimdata = response.data;

    // Now update rows using this data
    setRows((prevRows) =>
      prevRows.map((row) =>
        row.recNum === recNum
          ? {
              ...row,
              policyid: singleclaimdata.policyid,
              prod_sheet_type: singleclaimdata.prod_sheet_type,
              summary: singleclaimdata.summary,
              status: singleclaimdata.status,
              previewLink: singleclaimdata.previewLink,
            }
          : row
      )
    );
  } catch (error) {
    setMessage("Failed to fetch data for RecNum: " + recNum);
  }
};



<tbody>
  {rows.map((row, index) => (
    <tr key={index}>
      <td>{row.recNum}</td>
      <td>{row.policyid || <span className={styles.loader}>Pending</span>}</td>
      <td>{row.prod_sheet_type || <span className={styles.loader}>Pending</span>}</td>
      <td>{row.summary || <span className={styles.loader}>Pending</span>}</td>
      <td>
        {row.previewLink ? (
          <button
            className={styles.previewButton}
            onClick={() => openModal(row.previewLink)}
          >
            Preview
          </button>
        ) : (
          "Pending"
        )}
      </td>
      <td>
        {row.policyid && row.prod_sheet_type && row.summary ? (
          <span>Completed</span>
        ) : (
          <span>
            <button
              className={styles.reloadButton}
              onClick={() => handleReload(row.recNum)}
            >
              Reload
            </button>
          </span>
        )}
      </td>
    </tr>
  ))}
</tbody>
