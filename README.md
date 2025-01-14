import React, { useState } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp, faCopy, faCode } from '@fortawesome/free-solid-svg-icons';

import './TracePanel.css';

const JSONViewer = ({ data }) => {
  const renderJSON = (obj, depth = 0) => {
    if (obj === null) return <span className="json-null">null</span>;
    
    if (typeof obj !== 'object') {
      return (
        <span className={`json-\${typeof obj}`}>
          {JSON.stringify(obj)}
        </span>
      );
    }

    if (Array.isArray(obj)) {
      return (
        <div className="json-array">
          [
          {obj.map((item, index) => (
            <div 
              key={index} 
              style={{ paddingLeft: `\${depth * 20}px` }} 
              className="json-array-item"
            >
              {renderJSON(item, depth + 1)}
              {index < obj.length - 1 && ','}
            </div>
          ))}
          ]
        </div>
      );
    }

    return (
      <div className="json-object">
        {'{'}
        {Object.entries(obj).map(([key, value]) => (
          <div 
            key={key} 
            style={{ paddingLeft: `\${depth * 20}px` }} 
            className="json-object-item"
          >
            <span className="json-key">"{key}":</span> 
            {renderJSON(value, depth + 1)}
          </div>
        ))}
        {'}'}
      </div>
    );
  };

  return (
    <div className="custom-json-viewer">
      {renderJSON(data)}
    </div>
  );
};

const TracePanel = ({ isLoading, traceData, isTraceEnabled }) => {
  const [expandedSteps, setExpandedSteps] = useState({});
  const [copiedSteps, setCopiedSteps] = useState({});

  if (!isTraceEnabled) return null;

  const toggleStepExpansion = (stepIndex) => {
    setExpandedSteps(prev => ({
      ...prev,
      [stepIndex]: !prev[stepIndex]
    }));
  };

  const copyTraceDetails = (details, stepIndex) => {
    const jsonString = JSON.stringify(details, null, 2);
    navigator.clipboard.writeText(jsonString).then(() => {
      setCopiedSteps(prev => ({
        ...prev,
        [stepIndex]: true
      }));
      
      // Reset copied state after 2 seconds
      setTimeout(() => {
        setCopiedSteps(prev => ({
          ...prev,
          [stepIndex]: false
        }));
      }, 2000);
    });
  };

  return (
    <div className="trace-panel">
      <div className="trace-header">
        <h3>Agent Trace Steps</h3>
      </div>
      <div className="trace-content">
        {isLoading ? (
          <div className="trace-loading">
            <BeatLoader color="#785ce5" size={30} />
          </div>
        ) : traceData.traces && traceData.traces.length > 0 ? (
          traceData.traces.map((step, index) => (
            <div key={index} className="trace-step-card">
              <div 
                className="trace-step-header"
                onClick={() => toggleStepExpansion(index)}
                style={{ cursor: 'pointer' }}
              >
                <div className="trace-step-header-content">
                  <span className="trace-step-number">Step {step.Trace_Step}</span>
                  <span className="trace-agent-name">{step.Agent_Name}</span>
                </div>
                <FontAwesomeIcon 
                  icon={expandedSteps[index] ? faChevronUp : faChevronDown} 
                  className="expand-icon" 
                />
              </div>

              <AnimatePresence>
                {expandedSteps[index] && (
                  <motion.div 
                    className="trace-step-details"
                    initial={{ opacity: 0, height: 0 }}
                    animate={{ opacity: 1, height: 'auto' }}
                    exit={{ opacity: 0, height: 0 }}
                    transition={{ duration: 0.3 }}
                  >
                    <div className="trace-step-metadata">
                      <div className="trace-step-id">
                        <strong>Step ID:</strong> {step.Step_ID}
                      </div>
                      <div className="trace-actions">
                        <button 
                          className={`copy-btn \${copiedSteps[index] ? 'copied' : ''}`}
                          onClick={(e) => {
                            e.stopPropagation();
                            copyTraceDetails(step.Trace_Details, index);
                          }}
                        >
                          <FontAwesomeIcon icon={faCopy} /> 
                          {copiedSteps[index] ? 'Copied!' : 'Copy'}
                        </button>
                      </div>
                    </div>

                    <div className="trace-details-section">
                      <h4>
                        <FontAwesomeIcon icon={faCode} /> 
                        Trace Details
                      </h4>
                      <div className="json-viewer-container">
                        <JSONViewer data={step.Trace_Details} />
                      </div>
                    </div>
                  </motion.div>
                )}
              </AnimatePresence>
            </div>
          ))
        ) : (
          <div className="no-trace-data">
            <p>No trace data available</p>
          </div>
        )}
      </div>
    </div>
  );
};

export default TracePanel;

:root {
  /* Color Palette */
  --primary-color: #785ce5;
  --secondary-color: #6a47c3;
  --background-light: #f5f5f5;
  --background-dark: #f4f4f4;
  --text-primary: #333;
  --text-secondary: #666;
  --border-color: #e0e0e0;
  
  /* Typography */
  --font-family-base: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  --font-family-mono: 'Courier New', Courier, monospace;
  
  /* Spacing */
  --space-xs: 0.5rem;
  --space-sm: 0.75rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  
  /* Transitions */
  --transition-speed: 0.3s;
  
  /* Shadows */
  --shadow-subtle: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.trace-panel {
  width: 100%;
  max-width: 600px;
  background-color: white;
  border-radius: 16px;
  box-shadow: var(--shadow-subtle);
  border: 1px solid var(--border-color);
  overflow: hidden;
  font-family: var(--font-family-base);
}

.trace-header {
  background: linear-gradient(
    90deg, 
    rgba(120, 92, 229, 0.1) 0%, 
    rgba(120, 92, 229, 0.05) 100%
  );
  padding: var(--space-md) var(--space-lg);
  border-bottom: 1px solid var(--border-color);
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
}

.trace-header::before {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(
    90deg,
    #7abef7 1.81%, 
    #4080f5 43.03%, 
    #7747d5 68.71%, 
    #572ac2 89.75%
  );
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease;
}

.trace-header:hover::before {
  transform: scaleX(1);
  transform-origin: left;
}

.trace-header h3 {
  margin: 0;
  position: relative;
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  
  /* Gradient Text */
  background: linear-gradient(
    7.52deg,
    #7abef7 1.81%,
    #4080f5 43.03%,
    #7747d5 68.71%,
    #572ac2 89.75%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  
  /* Subtle text shadow for depth */
  text-shadow: 
    1px 1px 2px rgba(0, 0, 0, 0.05),
    2px 2px 4px rgba(0, 0, 0, 0.03);
  
  /* Transition for subtle hover effect */
  transition: 
    transform 0.3s ease,
    text-shadow 0.3s ease;
}

.trace-header h3:hover {
  transform: scale(1.02);
  text-shadow: 
    2px 2px 4px rgba(0, 0, 0, 0.1),
    3px 3px 6px rgba(0, 0, 0, 0.05);
}

/* Icon Styling */
.trace-header h3 svg {
  color: #7747d5;
  margin-right: var(--space-sm);
  transition: transform 0.3s ease;
}

.trace-header h3:hover svg {
  transform: rotate(15deg) scale(1.1);
}

/* Optional: Subtle Backdrop Blur */
@supports (backdrop-filter: blur(10px)) {
  .trace-header {
    background: linear-gradient(
      90deg, 
      rgba(120, 92, 229, 0.05) 0%, 
      rgba(120, 92, 229, 0.025) 100%
    );
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }
}

/* Responsive Adjustments */
@media (max-width: 768px) {
  .trace-header {
    padding: var(--space-sm) var(--space-md);
  }
  
  .trace-header h3 {
    font-size: 1rem;
  }
}

.trace-content {
  max-height: 600px;
  overflow-y: auto;
  scrollbar-width: thin;
}

.trace-step-card {
  border-bottom: 1px solid var(--border-color);
  transition: background-color var(--transition-speed) ease;
   box-shadow: 
    0 1px 2px rgba(0, 0, 0, 0.04);
}


.trace-step-header {

  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--space-md) var(--space-lg);
  cursor: pointer;
  transition: background-color var(--transition-speed) ease;
}

.trace-step-header:hover {
  background-color: var(--background-light);
}

.trace-step-header-content {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.trace-step-number {
  font-weight: 600;
  
  color: var(--primary-color);
  font-size: 1rem;
}

.trace-agent-name {
  font-size: 0.8rem;
  color: var(--text-secondary);
  margin-top: var(--space-xs);
}

.expand-icon {
  color: var(--primary-color);
  transition: transform var(--transition-speed) ease;
}

.trace-step-details {
  background-color: white;
  padding: var(--space-lg);
}

.trace-step-metadata {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--space-md);
  padding-bottom: var(--space-md);
  border-bottom: 1px solid var(--border-color);
}

.copy-btn {
      display: flex
;
    align-items: center;
    gap: 4px;
    /* background: linear-gradient(304deg, #499dfd 3.03%, #2678f5 27.62%, #6628c5 76.39%, #5b0bb1 112.44%); */
    color: #785ce5;
    border: none;
    padding: 8px 12px;
    border-radius: 8px;
    font-size: 0.8rem;
    cursor: pointer;
    transition: background-color var(--transition-speed) ease;
}

.copy-btn:hover {
  background-color: var(--secondary-color);
}

.copy-btn.copied {
  background-color: #4CAF50;
}

.json-viewer-container {
  background-color: var(--background-dark);
  border-radius: 12px;
  padding: var(--space-md);
  max-height: 220px;
  overflow-y: auto;
}

.custom-json-viewer {
  font-family: var(--font-family-mono);
  font-size: 0.9rem;
  line-height: 1.6;
}

/* JSON Syntax Highlighting */
.json-key { color: var(--primary-color); }
.json-string { color: #2ea44f; }
.json-number { color: #0366d6; }
.json-boolean { color: #d73a49; }
.json-null { 
  color: #6a737d; 
  font-style: italic; 
}

/* Responsive Design */
@media (max-width: 768px) {
  .trace-panel {
    max-width: 100%;
    border-radius: 0;
  }

  .trace-step-header,
  .trace-step-details {
    padding: var(--space-sm) var(--space-md);
  }

  .trace-step-metadata {
    flex-direction: column;
    align-items: flex-start;
    gap: var(--space-sm);
  }
}

/* Accessibility and Focus States */
.trace-step-header:focus-visible,
.copy-btn:focus-visible {
  outline: 2px solid var(--primary-color);
  outline-offset: 2px;
}

/* Animation */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.trace-step-details {
  animation: fadeIn var(--transition-speed) ease-in-out;
}
