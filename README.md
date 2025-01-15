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
        <span className={`json-${typeof obj}`}>
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
              style={{ paddingLeft: `${depth * 20}px` }} 
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
            style={{ paddingLeft: `${depth * 20}px` }} 
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
                          className={`copy-btn ${copiedSteps[index] ? 'copied' : ''}`}
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
  --primary-color: #785ce5;
  --secondary-color: #6a47c3;
  --background-light: #f5f5f5;
  --text-primary: #333;
  --text-secondary: #666;
  --border-color: #e0e0e0;
  --font-family-base: 'Segoe UI', Roboto, sans-serif;
  --space-sm: 0.75rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  --transition-speed: 0.3s;
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
}

.trace-header h3 {
  margin: 0;
  background: linear-gradient(
    90deg, 
    #7abef7 0%, 
    #4080f5 50%, 
    #7747d5 100%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.trace-content {
  max-height: 600px;
  overflow-y: auto;
}

.trace-step-card {
  border-bottom: 1px solid var(--border-color);
  padding: var(--space-md) var(--space-lg);
}

.trace-step-header {
  display: flex;
  justify-content: space-between;
  cursor: pointer;
}

.trace-step-header-content {
  display: flex;
  gap: var(--space-sm);
}

.trace-step-details {
  padding: var(--space-md) var(--space-lg);
}

.json-viewer-container {
  background: var(--background-light);
  padding: var(--space-md);
  border-radius: 8px;
  font-family: var(--font-family-base);
}

.json-key {
  color: var(--primary-color);
  font-weight: bold;
}

.copy-btn {
  background: var(--primary-color);
  color: white;
  border: none;
  padding: var(--space-sm) var(--space-md);
  cursor: pointer;
  border-radius: 4px;
  transition: background var(--transition-speed);
}

.copy-btn.copied {
  background: var(--secondary-color);
}
