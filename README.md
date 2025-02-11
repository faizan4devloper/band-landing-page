import React from 'react';
import { BeatLoader } from 'react-spinners';
import styles from "./EmailGenerator.module.css";

const EmailGenerator = ({
  loadingLLM,
  errorLLM,
  llmResponse
}) => {
  // Helper function to format the email content
  const formatEmailContent = (rawResponse) => {
    // If the response is a simple string, try to parse it
    if (typeof rawResponse === 'string') {
      // Split the response into paragraphs
      const paragraphs = rawResponse.split('\n\n').filter(p => p.trim() !== '');
      
      return (
        <div className={styles.emailContent}>
          {paragraphs.map((paragraph, index) => {
            // Check if the paragraph looks like a header/section
            if (paragraph.match(/^[A-Z][a-z\s:]+\$/)) {
              return <h3 key={index}>{paragraph}</h3>;
            }
            
            // Check if the paragraph looks like a list item
            if (paragraph.startsWith('•') || paragraph.startsWith('-')) {
              return (
                <ul key={index}>
                  <li>{paragraph.replace(/^[•\-]\s*/, '')}</li>
                </ul>
              );
            }
            
            // Check for salutation
            if (paragraph.toLowerCase().includes('dear')) {
              return <p key={index} className={styles.salutation}>{paragraph}</p>;
            }
            
            // Check for closing/signature
            if (paragraph.toLowerCase().includes('sincerely') || paragraph.includes('Regards')) {
              return <p key={index} className={styles.closing}>{paragraph}</p>;
            }
            
            // Default paragraph
            return <p key={index}>{paragraph}</p>;
          })}
        </div>
      );
    }
    
    // If response is an object or more complex
    if (typeof rawResponse === 'object') {
      return (
        <div className={styles.emailContent}>
          {Object.entries(rawResponse).map(([key, value]) => (
            <div key={key}>
              <h3>{key}</h3>
              <p>{value}</p>
            </div>
          ))}
        </div>
      );
    }
    
    // Fallback
    return <p>{JSON.stringify(rawResponse)}</p>;
  };

  if (loadingLLM) {
    return (
      <div className={styles.sectionWindow}>
        <div className={styles.genEmail}>
          <h2>Generating Email...</h2>
          <div className={styles.loaderContainer}>
            <BeatLoader
              color="#0f5fdc"
              loading={loadingLLM}
              size={15}
            />
          </div>
        </div>
      </div>
    );
  }

  if (errorLLM) {
    return (
      <div className={styles.sectionWindow}>
        <div className={styles.genEmail}>
          <h2>Email Generation Error</h2>
          <p className={styles.errorText}>{errorLLM}</p>
        </div>
      </div>
    );
  }

  return (
    <div className={styles.sectionWindow}>
      <div className={styles.header}>
        <h2>Generated Email</h2>
        <div className={styles.headerUnderline}></div>
      </div>
      <div className={styles.genEmail}>
        <div className={styles.emailPreview}>
          {formatEmailContent(llmResponse)}
        </div>
      </div>
    </div>
  );
};

export default EmailGenerator;


/* Individual Window Styling */
.sectionWindow {
  background: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
  transition: transform 0.2s ease, box-shadow 0.2s ease, height 0.3s ease;
}

.sectionWindow {
  background-color: #f9f9f9;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
}

.header {
  margin-bottom: 15px;
}

.headerUnderline {
  height: 2px;
  background-color: #0f5fdc;
  width: 50px;
}

.genEmail {
  position: relative;
}

.emailTextarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  resize: vertical;
  font-family: Arial, sans-serif;
  line-height: 1.6;
}

.emailControls {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 10px;
}

.copyButton {
  background-color: #0f5fdc;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.copyButton:hover {
  background-color: #0d4fa0;
}

.loaderContainer {
  display: flex;
  justify-content: center;
  padding: 20px;
}

.sectionWindow:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2);
}

.sectionWindow h2 {
  margin-bottom: 10px;
  font-size: 20px;
  color: #333;
}

.sectionWindow p {
  color: #555;
  font-size: 14px;
  line-height: 1.5;
  
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
/* Buttons */
.generateButton {
      padding: 10px 22px;
    background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
    color: white;
    border: none;
    /*margin: auto 164px;*/
    /*margin-left:460px;*/
    /*align-content: right;*/
    border-radius: 4px;
    cursor: pointer;
    font-size: 1rem;
    transition: background-color 0.2s ease;
}

.genEmail{
  display: flex;
  justify-content: space-between;
  align-items: center;
  /*gap:15px;*/
  
}

.generateButton:hover {
  background-color: #0056b3;
}

.generateButton:disabled {
  background-color: #b0c4de;
  cursor: not-allowed;
}

.loaderContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}

/* Optional: Error text styling */
.errorText {
  color: #dc3545;
  font-weight: 500;
  text-align: center;
  margin-top: 10px;
}
.headerUnderline {
  height: 3px;
  width: 50px;
  background: linear-gradient(to right, #4a90e2, #50c878);
  margin-top: 8px;
  border-radius: 2px;
}
.header {
  margin-bottom: 20px;
}

.sectionWindow {
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-bottom: 20px;
}

.header {
  margin-bottom: 20px;
}

.header h2 {
  color: #333;
  margin-bottom: 10px;
}

.headerUnderline {
  height: 2px;
  background-color: #0f5fdc;
  width: 50px;
}

.genEmail {
  line-height: 1.6;
}

.emailContent {
  color: #333;
  font-size: 16px;
}

.salutation {
  font-weight: bold;
  margin-bottom: 15px;
  color: #0f5fdc;
}

.emailContent h3 {
  color: #0f5fdc;
  margin-top: 15px;
  margin-bottom: 10px;
  border-bottom: 1px solid #f0f0f0;
  padding-bottom: 5px;
}

.emailContent ul {
  padding-left: 20px;
  margin-bottom: 15px;
  list-style-type: disc;
}

.emailContent li {
  margin-bottom: 5px;
}

.closing {
  margin-top: 20px;
  font-style: italic;
  color: #666;
}

.errorText {
  color: #ff4d4f;
  text-align: center;
}

.loaderContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.emailPreview {
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 6px;
}
