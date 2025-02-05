It sounds like the issue is that the percentage data is only displayed after the claim processing is done (i.e., after fetchPercentage completes). This could be because the state (percentage) is only set once the API finishes, but your component renders with percentage as null initially.

To fix this and ensure that the claim status (and percentage) are displayed correctly, we need to control the flow of the data display and loading states more effectively.

Possible Solutions

1. Conditionally Render Based on percentage and data

You can render the percentage only when it has been fetched, or show a loading state until the percentage is available.

Modify the section where you are rendering the ClaimProcessingStatus to handle the loading condition:

<div className={styles.percentageSection}>
  {isLoading ? (
    <p>Loading percentage...</p>  // Show loading text or a spinner
  ) : (
    <ClaimProcessingStatus 
      percentage={percentage} 
      isLoading={isLoading} 
    />
  )}
</div>

This ensures that ClaimProcessingStatus only renders when the isLoading state is false.

2. Show Claim Status After Both Data and Percentage Are Loaded

If you want to make sure both data and percentage are loaded before displaying the claim status, you can add a combined check for both data and percentage.

For example:

<div className={styles.percentageSection}>
  {(data && percentage !== null) ? (
    <ClaimProcessingStatus 
      percentage={percentage} 
      isLoading={isLoading} 
    />
  ) : (
    <p>Loading claim status...</p>
  )}
</div>

This way, ClaimProcessingStatus will only render when both the data and percentage are available, showing a loading message otherwise.

3. Use useEffect to Ensure Dependencies Are Loaded First

You can modify your useEffect hook to depend on both data and percentage before triggering the display of the claim processing status. Here’s an example of how you might structure it:

useEffect(() => {
  const fetchPercentage = async () => {
    if (rows.length === 0 || !rows[0]?.recNum) return;

    const currentClaimId = rows[0].recNum;
    setIsLoading(true);
    try {
      const response = await axios.post(
        "https://percentage",
        { claimid: currentClaimId },
        { headers: { "Content-Type": "application/json" } }
      );

      if (response.data && response.data.body) {
        let percentageValue = response.data.body.empty_key_perc 
          ? parseFloat(response.data.body.empty_key_perc.replace('%', ''))
          : 0;
          
        setPercentage(percentageValue);
      }
    } catch (error) {
      setPercentage(0);  // Set to 0 if error occurs
    } finally {
      setIsLoading(false);
    }
  };

  fetchPercentage();
}, [rows, data]);  // This ensures the effect runs when `rows` or `data` changes

4. Ensure Data Fetch Completes Before Rendering Other Components

Make sure handleReload (which fetches the claim data) is completed before triggering fetchPercentage. If the claim data (data) isn’t available, you might not get the correct claim ID for the percentage calculation. Adjust your logic in handleReload to ensure data is set first.

For example, inside handleReload, after the data is set, you can trigger fetchPercentage:

const handleReload = async (recNum) => {
  setLoading(true);
  try {
    const payload = {
      tasktype: "FETCH_SINGLE_ACT_CLAIM",
      claimid: recNum,
    };

    const response = await axios.post(
      "https://all",
      payload,
      { headers: { "Content-Type": "application/json" } }
    );

    setData(response.data.allclaimactdata);  // Set data first
    // Now that the data is set, fetch the percentage
    fetchPercentage();
  } catch (error) {
    console.error("Failed to fetch data for RecNum:", recNum, error);
  } finally {
    setLoading(false);
  }
};

This ensures that when handleReload is triggered, fetchPercentage won’t run until the data has been fetched and set.

Final Thoughts
	•	The issue likely stems from conditional rendering and loading state handling.
	•	By ensuring that the percentage and data are properly loaded before rendering the ClaimProcessingStatus, you can avoid showing incomplete or incorrect information.
	•	Use loading indicators or messages to guide the user through the process.

Let me know if these adjustments help!
