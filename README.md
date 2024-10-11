/* Main container */
.formDisplay {
    background: linear-gradient(135deg, #1e293b, #334155);
    padding: 30px;
    height: 100%;
    width: 100%;
    overflow-y: auto;
    font-family: 'Poppins', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 40px;
}

/* Form selection section */
.formSelection {
    display: flex;
    gap: 15px;
    justify-content: center;
    flex-wrap: wrap;
    padding-bottom: 20px;
}

/* Individual form button */
.formItem {
    background-color: #1f2937;
    color: #f8fafc;
    padding: 12px 20px;
    border-radius: 8px;
    font-size: 0.9rem;
    font-weight: bold;
    text-transform: uppercase;
    cursor: pointer;
    transition: background 0.3s ease, transform 0.4s ease;
    display: flex;
    align-items: center;
    /*gap: 10px;*/
}

.formItem:hover, .formItem.active {
    background: linear-gradient(135deg, #f2f2f2 -20%, #7ca2e1);
    color: #1f2937;
}

.chevronIcon.active{
    color: #f8fafc;
}

.chevronIcon {
    color: #f8fafc;
    transition: transform 0.3s ease;
    margin-left: 10px;
}

.cardIcon{
    margin-right: 10px;
}

.formItem:hover .chevronIcon {
    transform: translateX(5px);
}

/* Consistent layout for form data */
.formDataSection {
    background-color: #f1f5f9;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 800px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    color: #374151;
}

/* Titles for each section in form data */
.formDataTitle {
    font-size: 1rem;
    font-weight: 600;
    color: #2563eb;
}

/* Content in form data */
.formDataContent {
    font-size: .9rem;
    color: #1f2937;
    padding-left: 15px;
    line-height: 1.6;
}

/* Hover effect */
.formDataSection:hover {
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.2);
}

/* Loading and select messages */
.loadingMessage,
.selectMessage {
    font-size: 1.5rem;
    color: #e2e8f0;
    text-align: center;
    margin-top: 20px;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
    }

    .formDataTitle {
        font-size: 1rem;
    }
    
    .formDataContent {
        font-size: 0.9rem;
    }
}

