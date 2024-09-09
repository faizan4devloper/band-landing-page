import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

const RequestDemoForm = ({ closeModal, formType }) => { // Added formType to differentiate between demo and feedback
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    domain: '',
    customerName: '',
    details: '',
    typee: formType || 'demo' // Initialize typee based on formType prop
  });

  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      // Send the form data to the Lambda function through the API Gateway
      await axios.post('xyz', {
        ...formData,
        solution: selectedSolution ? selectedSolution.label : ''
      });

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting form:', error);
      // Handle error case
    }
  };

  const handleCloseModal = () => {
    setIsSubmitted(false); // Reset submission state
    closeModal();
  };

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%", // Control width
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      color: "#555",
      boxShadow: "none", // Remove the default box-shadow
      "&:hover": {
        border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc"
      }
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : state.isFocused ? "#eee" : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px", // Option font size
      cursor: "pointer",
      "&:hover": {
        backgroundColor: state.isSelected ? "#5f1ec1" : "#f0f0f0" // Change hover background color
      }
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px", // Placeholder font size
      color: "#999" // Placeholder color
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
       // Limit the height of the menu to 150px
      overflowY: "auto", // Enable vertical scrolling when necessary
      zIndex: 2 // Ensure it is above other elements
    }),
    menuList: (provided) => ({
      ...provided,
      maxHeight: "150px",
      padding: 0 // Remove padding to avoid additional scrolling issues
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#555" // Single value color
    })
  };

  return (
    <div className={styles.formContainer}>
      <button className={styles.closeButton} onClick={handleCloseModal}>
        <FontAwesomeIcon icon={faTimes} />
      </button>
      <h2 className={styles.demoHead}>
        {formData.typee === 'demo' ? 'Request for a Live Demo' : 'Feedback Form'}
      </h2>
      {!isSubmitted ? (
        <form onSubmit={handleSubmit}>
          <div className={styles.formGroup}>
            <label>*User Name</label>
            <input
              type="text"
              name="userName"
              value={formData.userName}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>*Email Address</label>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleInputChange}
              required
            />
          </div>
          {formData.typee === 'demo' && (
            <>
              <div className={styles.formGroup}>
                <label>GenAI Solution Name</label>
                <div className={styles.customSelect}>
                  <Select
                    styles={customStyles}
                    value={selectedSolution}
                    onChange={setSelectedSolution}
                    options={[
                      { value: "option1", label: "Intelligent Assist" },
                      { value: "option2", label: "Email EAR" },
                      { value: "option3", label: "Case Intelligence" },
                      { value: "option4", label: "Smart Recruit" },
                      { value: "option5", label: "iAssure Claim" },
                      { value: "option6", label: "Assistant For EVs" },
                      { value: "option7", label: "AutoWise Companion" },
                      { value: "option8", label: "Citizen Advisor" },
                      { value: "option9", label: "Fin Competitor Summary Gen" },
                      { value: "option10", label: "Signature Extraction & Verification" },
                      { value: "option11", label: "AI Force" },
                      { value: "option12", label: "API based Test Case Generation" },
                      { value: "option13", label: "AMS Support Automation" },
                      { value: "option14", label: "SOP Assistance" },
                      { value: "option15", label: "Code GReat" },
                      { value: "option16", label: "AAIG-API Analyzer & Insight Generator" },
                      { value: "option17", label: "Responsible Gen AI with Llama-13 B" },
                      { value: "option18", label: "Graph data Interpretation using Gen AI" },
                      { value: "option19", label: "Predictive Asset Maintenance​(PAM)​" },
                    ]}
                  />
                </div>
              </div>
              <div className={styles.formGroup}>
                <label>Domain</label>
                <input
                  type="text"
                  name="domain"
                  value={formData.domain}
                  onChange={handleInputChange}
                  required
                />
              </div>
              <div className={styles.formGroup}>
                <label>Customer Name</label>
                <input
                  type="text"
                  name="customerName"
                  value={formData.customerName}
                  onChange={handleInputChange}
                  required
                />
              </div>
              <div className={styles.formGroup}>
                <label>More Details</label>
                <textarea
                  name="details"
                  placeholder="Enter your business details and scope of this demo in your use case."
                  rows="4"
                  value={formData.details}
                  onChange={handleInputChange}
                  required
                ></textarea>
              </div>
            </>
          )}
          {formData.typee === 'feedback' && (
            <>
              <div className={styles.formGroup}>
                <label>Feedback</label>
                <textarea
                  name="feedback"
                  placeholder="Enter your feedback here."
                  rows="4"
                  value={formData.feedback}
                  onChange={handleInputChange}
                  required
                ></textarea>
              </div>
              <div className={styles.formGroup}>
                <label>Rate Us</label>
                <input
                  type="number"
                  name="rateus"
                  min="1"
                  max="5"
                  value={formData.rateus}
                  onChange={handleInputChange}
                  required
                />
              </div>
            </>
          )}
          <button type="submit" className={styles.submitButton}>
            Submit
          </button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you! Your request has been submitted successfully.
        </p>
      )}
    </div>
  );
};

export default RequestDemoForm;










// This is my lambda code
import json
import boto3
from botocore.exceptions import ClientError

# Initialize the DynamoDB and SNS clients
dynamodb = boto3.resource('dynamodb')
sns = boto3.client('sns')

# Replace 'RequestDemoSubmissions' with your actual table name
table = dynamodb.Table('RequestDemoSubmissions')

# Replace 'your-sns-topic-arn' with your actual SNS topic ARN
sns_topic_arn = 'arn:aws:sns:us-east-1:040504913362:Portalalerts'

def lambda_handler(event, context):
    try:
        # Extract the form data from the event object
        form_data = json.loads(event['body'])
        #form_data = event #loca testing
        typee=form_data['typee']
        print(typee)
        if typee=="demo":
            demoform(form_data)
        else:
            fbform(form_data)
    except ClientError as e:
        print('DynamoDB error:', e)
        # Return an error response
        return {
            'statusCode': 500,
            'headers': {
                'Access-Control-Allow-Origin': '*',  # Allow CORS for all origins
                'Access-Control-Allow-Methods': 'POST, OPTIONS',
                'Access-Control-Allow-Headers': 'Content-Type'
            },
            'body': json.dumps({'message': 'Error saving form data to DynamoDB'})
        }
    except Exception as e:
        print('Error processing form:', e)
        # Return an error response
        return {
            'statusCode': 500,
            'headers': {
                'Access-Control-Allow-Origin': '*',  # Allow CORS for all origins
                'Access-Control-Allow-Methods': 'POST, OPTIONS',
                'Access-Control-Allow-Headers': 'Content-Type'
            },
            'body': json.dumps({'message': 'Error submitting form'})
        }
def demoform(form_data):
    userName = form_data['userName']
    email = form_data['email']
    solution = form_data['solution']
    domain = form_data['domain']
    customerName = form_data['customerName']
    details = form_data['details']
    typee=form_data['typee']

    # Save the form data to DynamoDB
    response = table.put_item(
        Item={
            'email': email,  # Primary Key
            'userName': userName,
            'solution': solution,
            'domain': domain,
            'customerName': customerName,
            'details': details,
            'typee':typee
        }
    )

    # Publish an SNS message
    sns_message = f"New form submission:\n\nName: {userName}\nEmail: {email}\nSolution: {solution}\nDomain: {domain}\nCustomer Name: {customerName}\nDetails: {details}"
    sns_response = sns.publish(
        TopicArn=sns_topic_arn,
        Message=sns_message,
        Subject='New Form Submission'
    )

    # Return a successful response
    return {
        'statusCode': 200,
        'headers': {
            'Access-Control-Allow-Origin': '*',  # Allow CORS for all origins
            'Access-Control-Allow-Methods': 'POST, OPTIONS',
            'Access-Control-Allow-Headers': 'Content-Type'
        },
        'body': json.dumps({'message': 'Form submitted and saved to DynamoDB successfully'})
    }
    
def fbform(form_data):
    userName = form_data['userName']
    email = form_data['email']
    solution = form_data['solution']
    feedback = form_data['feedback']
    typee=form_data['typee']
    rateus = form_data['rateus']

    # Save the form data to DynamoDB
    response = table.put_item(
        Item={
            'email': email,  # Primary Key
            'userName': userName,
            'solution': solution,
            'feedback': feedback,
            'typee': typee,
            'rateus': rateus
        }
    )

    # Publish an SNS message
    sns_message = f"New form submission:\n\nName: {userName}\nEmail: {email}\nSolution: {solution}\nfeedback: {feedback}"
    sns_response = sns.publish(
        TopicArn=sns_topic_arn,
        Message=sns_message,
        Subject='New Form Submission'
    )

    # Return a successful response
    return {
        'statusCode': 200,
        'headers': {
            'Access-Control-Allow-Origin': '*',  # Allow CORS for all origins
            'Access-Control-Allow-Methods': 'POST, OPTIONS',
            'Access-Control-Allow-Headers': 'Content-Type'
        },
        'body': json.dumps({'message': 'Form submitted and saved to DynamoDB successfully'})
    }



import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

const RequestDemoForm = ({ closeModal }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    domain: '',
    customerName: '',
    details: '',
  });

  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      // Send the form data to the Lambda function through the API Gateway
      await axios.post('xyz', {
        ...formData,
        solution: selectedSolution ? selectedSolution.label : ''
      });

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting form:', error);
      // Handle error case
    }
  };

  const handleCloseModal = () => {
    setIsSubmitted(false); // Reset submission state
    closeModal();
  };

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%", // Control width
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      color: "#555",
      boxShadow: "none", // Remove the default box-shadow
      "&:hover": {
        border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc"
      }
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : state.isFocused ? "#eee" : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px", // Option font size
      cursor: "pointer",
      "&:hover": {
        backgroundColor: state.isSelected ? "#5f1ec1" : "#f0f0f0" // Change hover background color
      }
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px", // Placeholder font size
      color: "#999" // Placeholder color
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
       // Limit the height of the menu to 150px
      overflowY: "auto", // Enable vertical scrolling when necessary
      zIndex: 2 // Ensure it is above other elements
    }),
    menuList: (provided) => ({
      ...provided,
      maxHeight: "150px",
      padding: 0 // Remove padding to avoid additional scrolling issues
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#555" // Single value color
    })
  };

  return (
    <div className={styles.formContainer}>
      <button className={styles.closeButton} onClick={handleCloseModal}>
        <FontAwesomeIcon icon={faTimes} />
      </button>
      <h2 className={styles.demoHead}>Request for a Live Demo</h2>
      {!isSubmitted ? (
        <form onSubmit={handleSubmit}>
          <div className={styles.formGroup}>
            <label>*User Name</label>
            <input
              type="text"
              name="userName"
              value={formData.userName}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>*Email Address</label>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>GenAI Solution Name</label>
            <div className={styles.customSelect}>
              <Select
                styles={customStyles}
                value={selectedSolution}
                onChange={setSelectedSolution}
                options={[
                  { value: "option1", label: "Intelligent Assist" },
                  { value: "option2", label: "Email EAR" },
                  { value: "option3", label: "Case Intelligence" },
                  { value: "option4", label: "Smart Recruit" },
                  { value: "option5", label: "iAssure Claim" },
                  { value: "option6", label: "Assistant For EVs" },
                  { value: "option7", label: "AutoWise Companion" },
                  { value: "option8", label: "Citizen Advisor" },
                  { value: "option9", label: "Fin Competitor Summary Gen" },
                  { value: "option10", label: "Signature Extraction & Verification" },
                  { value: "option11", label: "AI Force" },
                  { value: "option12", label: "API based Test Case Generation" },
                  { value: "option13", label: "AMS Support Automation" },
                  { value: "option14", label: "SOP Assistance" },
                  { value: "option15", label: "Code GReat" },
                  { value: "option16", label: "AAIG-API Analyzer & Insight Generator" },
                  { value: "option17", label: "Responsible Gen AI with Llama-13 B" },
                  { value: "option18", label: "Graph data Interpretation using Gen AI" },
                  { value: "option19", label: "Predictive Asset Maintenance​(PAM)​" },
                ]}
              />
            </div>
          </div>
          <div className={styles.formGroup}>
            <label>Domain</label>
            <input
              type="text"
              name="domain"
              value={formData.domain}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>Customer Name</label>
            <input
              type="text"
              name="customerName"
              value={formData.customerName}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>More Details</label>
            <textarea
              name="details"
              placeholder="Enter your business details and scope of this demo in your use case."
              rows="4"
              value={formData.details}
              onChange={handleInputChange}
              required
            ></textarea>
          </div>
          <button type="submit" className={styles.submitButton}>
            Submit
          </button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you! Your request for a live demo has been submitted successfully.
        </p>
      )}
    </div>
  );
};

export default RequestDemoForm;

