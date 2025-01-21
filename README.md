<div className="chat-header">
  <FontAwesomeIcon icon={faRobot} className="header-icon" />
  <h1 className="header-title">Insurance Claim Assist</h1>
  <span className="powered-by">Powered by Agentic AI</span>
</div>

<div className="trace-header">
  <h2 className="trace-title">Agentic Orchestration - Routing & Tracing</h2>
</div>




.chat-header {    
  background: linear-gradient(304deg, #499dfd 3.03%, #2678f5 27.62%, #6628c5 76.39%, #5b0bb1 112.44%);
  color: #ffffff;
  padding: 15px;
  text-align: center;
  font-weight: 700;
  border-radius: 12px;
  margin-bottom: 20px;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.header-icon {
  font-size: 24px;
  color: #ffffff;
}

.header-title {
  font-size: 22px;
  font-weight: 700;
  margin: 0;
}

.powered-by {
  font-size: 14px;
  font-weight: 500;
  color: #f0f0f0;
  margin-left: auto;
  text-align: right;
}





.trace-step-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 15px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  border-bottom: 1px solid #e0e0e0;
}

.trace-step-header:hover {
  background-color: #f7faff;
}

.trace-step-header-content {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  gap: 5px;
}

.trace-step-header-content h3 {
  font-size: 18px;
  color: #2678f5;
  margin: 0;
}

.trace-step-header-content p {
  font-size: 14px;
  color: #7a7a7a;
  margin: 0;
}
