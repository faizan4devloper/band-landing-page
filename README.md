const fetchData = async (payload, setData) => {
  try {
    const response = await axios.post(API_ENDPOINT, payload, {
      headers: { "Content-Type": "application/json" },
    });

    if (response.data?.body) {
      const parsedData = JSON.parse(response.data.body); // ✅ Parse JSON string
      setData(parsedData);
    }
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};



{/* Tile 2 - Previous Day Transactions */}
<div className={`${styles.tile} ${styles.tile2}`}>
  <div className={styles.tileHeader}>
    <FontAwesomeIcon icon={faClipboardList} className={styles.tileIcon} />
    <h4>Previous Day Transactions</h4>
  </div>
  <div className={styles.tileContent}>
    <div className={styles.statItem}>
      <span>Total</span>
      <span className={styles.statValue}>{tile2Data.total_count || 0}</span>
    </div>
    <div className={styles.statItem}>
      <span>Successful</span>
      <span className={styles.statValue}>{tile2Data.successful_count || 0}</span>
    </div>
    <div className={styles.statItem}>
      <span>Last Updated</span>
      <span className={styles.statValue}>
        {tile2Data.last_updated ? new Date(tile2Data.last_updated).toLocaleString() : "N/A"}
      </span>
    </div>
  </div>
</div>
