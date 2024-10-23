import React, { useState, useEffect } from 'react';
import axios from 'axios';

const MainContent = () => {
  const [contentData, setContentData] = useState(null);
  const [openQuestion, setOpenQuestion] = useState(false);

  // Fetch data from API using POST request
  const fetchData = async () => {
    try {
      const response = await axios.post('dummy', {
        question: 'what are the average class sizes and student-teacher ratios in the local schools react?'
      }, {
        headers: {
          'Content-Type': 'application/json'
        },
      });

      const parsedBody = JSON.parse(response.data.body);

      setContentData(parsedBody);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  const toggleAnswer = () => {
    setOpenQuestion(!openQuestion);
  };

  return (
    <div>
      {contentData ? (
        <div>
          {/* Question */}
          <div onClick={toggleAnswer} style={{ cursor: 'pointer', marginBottom: '10px', fontWeight: 'bold' }}>
            Question: What are the average class sizes and student-teacher ratios in the local schools?
          </div>

          {/* Answer (shown only when the question is expanded) */}
          {openQuestion && (
            <div>
              <p><strong>Textual Response:</strong> {contentData.answer?.textual || 'No Textual Response Available'}</p>
              <p><strong>Citizen Experience:</strong> {contentData.answer?.citizenExperience || 'No Citizen Experience Available'}</p>
              <p><strong>Factual Info:</strong> {contentData.answer?.factualInfo || 'No Factual Info Available'}</p>
              <p><strong>Contextual:</strong> {contentData.answer?.contextual || 'No Contextual Info Available'}</p>
            </div>
          )}
        </div>
      ) : (
        <div>No data available</div>
      )}
    </div>
  );
};

export default MainContent;
