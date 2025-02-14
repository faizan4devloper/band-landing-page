import { useState, useEffect } from "react";
import axios from "axios";

const useVerifyData = (recNum) => {
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const cachedSummary = localStorage.getItem(`summary_${recNum}`);
    const cachedRecommendation = localStorage.getItem(`recommendation_${recNum}`);

    if (cachedSummary && cachedRecommendation) {
      setSummary(JSON.parse(cachedSummary));
      setRecommendation(cachedRecommendation);
      setLoading(false);
      return; // Skip API call if cached data exists
    }

    const fetchData = async () => {
      try {
        const payload = { claimid: recNum, recnumber: "PS12345" };
        const response = await axios.post("https://verify", payload, { headers: { "Content-Type": "application/json" } });

        const responseBody = response.data.body ? JSON.parse(response.data.body) : response.data;
        const summaryDetails = responseBody.SUMMARY_DETAILS || {};

        const summaryData = {
          briefSummary: summaryDetails.CLAIM_FORM_BRIEF_SUMMARY || "No Brief Summary",
          claimType: summaryDetails.CLAIM_FORM_TYPE || "Unknown Type",
          claimStatus: summaryDetails.CLAIM_STATUS || "Pending",
        };

        const recommendationData = summaryDetails.DETAILED_SUMMARY || "No Detailed Summary";

        // Save data in localStorage
        localStorage.setItem(`summary_${recNum}`, JSON.stringify(summaryData));
        localStorage.setItem(`recommendation_${recNum}`, recommendationData);

        setSummary(summaryData);
        setRecommendation(recommendationData);
      } catch (error) {
        setError(error.response?.data?.message || "Failed to load data");
      } finally {
        setLoading(false);
      }
    };

    if (recNum) {
      fetchData();
    } else {
      setError("No claim ID provided");
      setLoading(false);
    }
  }, [recNum]);

  return { summary, recommendation, loading, error };
};

export default useVerifyData;


const { summary, recommendation, loading, error } = useVerifyData(
  recNum || localStorage.getItem("currentClaimId")
);

useEffect(() => {
  if (summary) localStorage.setItem(`summary_${recNum}`, JSON.stringify(summary));
  if (recommendation) localStorage.setItem(`recommendation_${recNum}`, recommendation);
}, [summary, recommendation, recNum]);
