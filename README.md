const handleVerify = (recNum) => {
  const parsedData = data ? JSON.parse(data?.total_extracted_data || "{}") : {};
  const summary = parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY;

  navigate('/verify', {
    state: { recNum, summary },
  });
};
