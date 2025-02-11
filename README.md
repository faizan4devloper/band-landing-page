import React, { useRef, useState } from 'react';
import { BeatLoader } from 'react-spinners';
import styles from "./EmailGenerator.module.css";

const EmailGenerator = ({ loadingLLM, errorLLM, llmResponse }) => {
  const emailRef = useRef(null);
  const [copied, setCopied] = useState(false);

  // Helper function to format the email content
  const formatEmailContent = (rawResponse) => {
    if (typeof rawResponse === 'string') {
      const paragraphs = rawResponse.split('\n\n').filter(p => p.trim() !== '');

      return paragraphs.map((paragraph, index) => {
        if (paragraph.match(/^[A-Z][a-z\s:]+\$/)) {
          return <h3 key={index}>{paragraph}</h3>;
        }

        if (paragraph.startsWith('•') || paragraph.startsWith('-')) {
          return (
            <ul key={index}>
              <li>{paragraph.replace(/^[•\-]\s*/, '')}</li>
            </ul>
          );
        }

        if (paragraph.toLowerCase().includes('dear')) {
          return <p key={index} className={styles.salutation}>{paragraph}</p>;
        }

        if (paragraph.toLowerCase().includes('sincerely') || paragraph.includes('Regards')) {
          return <p key={index} className={styles.closing}>{paragraph}</p>;
        }

        return <p key={index}>{paragraph}</p>;
      });
    }

    if (typeof rawResponse === 'object') {
      return Object.entries(rawResponse).map(([key, value]) => (
        <div key={key}>
          <h3>{key}</h3>
          <p>{value}</p>
        </div>
      ));
    }

    return <p>{JSON.stringify(rawResponse)}</p>;
  };

  const copyToClipboard = () => {
    if (emailRef.current) {
      const text = emailRef.current.innerText;
      navigator.clipboard.writeText(text);
      setCopied(true);
      setTimeout(() => setCopied(false), 1500);
    }
  };

  if (loadingLLM) {
    return (
      <div className={styles.sectionWindow}>
        <h2>Generating Email...</h2>
        <div className={styles.loaderContainer}>
          <BeatLoader color="#0f5fdc" loading={loadingLLM} size={15} />
        </div>
      </div>
    );
  }

  if (errorLLM) {
    return (
      <div className={styles.sectionWindow}>
        <h2>Email Generation Error</h2>
        <p className={styles.errorText}>{errorLLM}</p>
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
        <div 
          ref={emailRef} 
          className={styles.emailPreview} 
          contentEditable={true} 
          suppressContentEditableWarning={true}
        >
          {formatEmailContent(llmResponse)}
        </div>
      </div>
      <div className={styles.buttonContainer}>
        <button className={styles.copyButton} onClick={copyToClipboard}>
          {copied ? "Copied!" : "Copy Email"}
        </button>
      </div>
    </div>
  );
};

export default EmailGenerator;





.sectionWindow {
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin: 20px 0;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.sectionWindow:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
}

.header {
  margin-bottom: 15px;
}

.headerUnderline {
  height: 3px;
  width: 50px;
  background: linear-gradient(to right, #4a90e2, #50c878);
  margin-top: 8px;
  border-radius: 2px;
}

.genEmail {
  display: flex;
  justify-content: center;
  align-items: center;
}

.emailPreview {
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 6px;
  min-height: 120px;
  width: 100%;
  border: 1px solid #ddd;
  font-size: 16px;
  line-height: 1.6;
  outline: none;
  transition: border 0.3s ease;
}

.emailPreview:focus {
  border-color: #0f5fdc;
}

.salutation {
  font-weight: bold;
  color: #0f5fdc;
}

.emailContent h3 {
  color: #0f5fdc;
  margin-top: 15px;
  border-bottom: 1px solid #f0f0f0;
  padding-bottom: 5px;
}

.emailContent ul {
  padding-left: 20px;
  margin-bottom: 15px;
  list-style-type: disc;
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

.buttonContainer {
  display: flex;
  justify-content: flex-end;
  margin-top: 10px;
}

.copyButton {
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background 0.3s ease;
}

.copyButton:hover {
  background: linear-gradient(90deg, #0d4fa0, #5a8bdc, #0d4fa0);
}

.copyButton:active {
  transform: scale(0.98);
}
