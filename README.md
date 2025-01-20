import React from 'react';
import { BeatLoader } from 'react-spinners';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import './TraceToggle.css';

const TraceToggle = ({ index, toggleTraceData, isTraceLoading, showTracePanel }) => {
  return (
    <button
      onClick={() => toggleTraceData(index)}
      className={`trace-toggle-btn ${showTracePanel ? 'active' : ''}`}
      disabled={isTraceLoading}
    >
      {isTraceLoading && !showTracePanel ? (
        <BeatLoader color="#fff" size={10} />
      ) : showTracePanel ? (
        <>
          Hide Trace <FontAwesomeIcon icon={faChevronUp} className="icon" />
        </>
      ) : (
        <>
          Enable Trace <FontAwesomeIcon icon={faChevronDown} className="icon" />
        </>
      )}
    </button>
  );
};

export default TraceToggle;


.trace-toggle-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  background-color: #1f2937;
  color: #9ca3af;
  border: 1px solid #374151;
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.trace-toggle-btn:hover {
  background-color: #374151;
  color: #ffffff;
}

.trace-toggle-btn:disabled {
  background-color: #4b5563;
  color: #6b7280;
  cursor: not-allowed;
}

.trace-toggle-btn.active {
  background-color: #10b981;
  color: #ffffff;
  border-color: #10b981;
}

.trace-toggle-btn.active:hover {
  background-color: #059669;
}

.icon {
  font-size: 16px;
  transition: transform 0.3s ease;
}
