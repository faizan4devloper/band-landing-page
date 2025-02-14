import React from "react";
import PropTypes from "prop-types";

const ClaimSummary = ({ summary }) => {
  // Ensure summary is a string; fallback to empty string if not
  const summaryText =
    typeof summary === "string"
      ? summary
      : summary?.briefSummary || "No summary available";

  return (
    <div className="claim-summary-container">
      <h3>Claim Summary</h3>
      <p>{summaryText}</p>
    </div>
  );
};

// Define prop types
ClaimSummary.propTypes = {
  summary: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.shape({
      briefSummary: PropTypes.string,
      claimType: PropTypes.string,
      claimStatus: PropTypes.string,
    }),
  ]),
};

export default ClaimSummary;
