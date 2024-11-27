import React, { useState } from "react";
import axios from "axios";

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [responseData, setResponseData] = useState(null);

  const handleGenerateEmail = async () => {
    // Define the payload to be sent
    const payload = {
      claimid: "CL123456",
      recnumber: "PS391481",
      tasktype: "GENERATE_EMAIL"
    };

    // Start the loading state
    setLoading(true);
    setError(null);

    try {
      // Make the POST request with the payload
      const response = await axios.post("https://your-api-endpoint.com/api/task", payload, {
        headers: {
          "Content-Type": "application/json",
        },
      });

      // Handle the response data
      setResponseData(response.data);
      console.log("Response Data:", response.data);
    } catch (err) {
      // Handle error
      setError("Failed to generate email");
      console.error("Error:", err);
    } finally {
      // Stop the loading state
      setLoading(false);
    }
  };

  return (
    <div>
      <button onClick={handleGenerateEmail} disabled={loading}>
        {loading ? "Generating Email..." : "Generate Email"}
      </button>

      {error && <p style={{ color: "red" }}>{error}</p>}
      {responseData && <p>Email Generated Successfully!</p>}
    </div>
  );
};

export default GenerateEmail;



button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

button:hover {
  background-color: #0056b3;
}
