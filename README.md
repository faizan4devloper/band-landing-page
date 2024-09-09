import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faStar, faTimes } from "@fortawesome/free-solid-svg-icons";
import styles from "./FeedbackForm.module.css";

const FeedbackForm = ({ closeModal, formType }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);
  const [rating, setRating] = useState(0);
  const [activeReview, setActiveReview] = useState('solution');
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    feedback: '',
    typee: formType || 'feedback' // Initialize typee based on formType prop
  });

  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleStarClick = (newRating) => {
    setRating(newRating);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      await axios.post('your-api-endpoint', {
        ...formData,
        rateus: rating,
        solution: selectedSolution ? selectedSolution.label : ''
      });

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting feedback:', error);
    }
  };

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%",
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      color: "#555",
      boxShadow: "none",
      "&:hover": {
        border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc"
      }
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : state.isFocused ? "#eee" : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px",
      cursor: "pointer",
      "&:hover": {
        backgroundColor: state.isSelected ? "#5f1ec1" : "#f0f0f0"
      }
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#999"
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
      overflowY: "auto",
      zIndex: 2
    }),
    menuList: (provided) => ({
      ...provided,
      maxHeight: "150px",
      padding: 0
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#555"
    })
  };

  return (
    <div className={styles.formContainer}>
      <button className={styles.closeButton} onClick={closeModal}>
        <FontAwesomeIcon icon={faTimes} />
      </button>

      {!isSubmitted ? (
        <>
          <h2 className={styles.feedbackHead}>Provide Your Feedback</h2>

          <div className={styles.toggleButtons}>
            <button
              className={activeReview === 'solution' ? styles.activeButton : styles.inactiveButton}
              onClick={() => setActiveReview('solution')}
            >
              About Solution
            </button>
            <button
              className={activeReview === 'portal' ? styles.activeButton : styles.inactiveButton}
              onClick={() => setActiveReview('portal')}
            >
              About Portal
            </button>
          </div>

          <form onSubmit={handleSubmit}>
            {activeReview === 'solution' && (
              <div>
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
                  <label>Select Solution</label>
                  <div className={styles.customSelect}>
                    <Select
                      styles={customStyles}
                      value={selectedSolution}
                      onChange={setSelectedSolution}
                      options={[
                        { value: "option1", label: "Intelligent Assist" },
                        { value: "option2", label: "Email EAR" },
                        { value: "option3", label: "Case Intelligence" },
                        // other options...
                      ]}
                    />
                  </div>
                </div>
                <div className={styles.formGroup}>
                  <label>Feedback</label>
                  <textarea
                    name="feedback"
                    value={formData.feedback}
                    onChange={handleInputChange}
                    rows="4"
                    required
                  />
                </div>
              </div>
            )}

            {activeReview === 'portal' && (
              <div>
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
                  <label>Feedback</label>
                  <textarea
                    name="feedback"
                    value={formData.feedback}
                    onChange={handleInputChange}
                    rows="4"
                    required
                  />
                </div>
              </div>
            )}

            <div className={styles.ratingGroup}>
              <label>Rate Us</label>
              <div className={styles.starRating}>
                {[1, 2, 3, 4, 5].map((star) => (
                  <FontAwesomeIcon
                    key={star}
                    icon={faStar}
                    className={star <= rating ? styles.activeStar : styles.inactiveStar}
                    onClick={() => handleStarClick(star)}
                  />
                ))}
              </div>
            </div>
            <button type="submit" className={styles.submitButton}>Submit</button>
          </form>
        </>
      ) : (
        <p className={styles.successMessage}>
          Thank you for your feedback!
        </p>
      )}
    </div>
  );
};

export default FeedbackForm;












import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faStar, faTimes } from "@fortawesome/free-solid-svg-icons";
import styles from "./FeedbackForm.module.css";

const FeedbackForm = ({ closeModal, formType }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);
  const [rating, setRating] = useState(0);
  const [activeReview, setActiveReview] = useState('solution');
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    feedback: '',
    typee: formType || 'feedback' // Initialize typee based on formType prop
  });

  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleStarClick = (newRating) => {
    setRating(newRating);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      await axios.post('your-api-endpoint', {
        ...formData,
        rateus: rating,
        solution: selectedSolution ? selectedSolution.label : ''
      });

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting feedback:', error);
    }
  };

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%",
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      color: "#555",
      boxShadow: "none",
      "&:hover": {
        border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc"
      }
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : state.isFocused ? "#eee" : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px",
      cursor: "pointer",
      "&:hover": {
        backgroundColor: state.isSelected ? "#5f1ec1" : "#f0f0f0"
      }
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#999"
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
      overflowY: "auto",
      zIndex: 2
    }),
    menuList: (provided) => ({
      ...provided,
      maxHeight: "150px",
      padding: 0
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#555"
    })
  };

  return (
    <div className={styles.formContainer}>
      <button className={styles.closeButton} onClick={closeModal}>
        <FontAwesomeIcon icon={faTimes} />
      </button>
      <h2 className={styles.feedbackHead}>Provide Your Feedback</h2>

      <div className={styles.toggleButtons}>
        <button
          className={activeReview === 'solution' ? styles.activeButton : styles.inactiveButton}
          onClick={() => setActiveReview('solution')}
        >
          About Solution
        </button>
        <button
          className={activeReview === 'portal' ? styles.activeButton : styles.inactiveButton}
          onClick={() => setActiveReview('portal')}
        >
          About Portal
        </button>
      </div>

      {!isSubmitted ? (
        <form onSubmit={handleSubmit}>
          {activeReview === 'solution' && (
            <div>
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
                <label>Select Solution</label>
                <div className={styles.customSelect}>
                  <Select
                    styles={customStyles}
                    value={selectedSolution}
                    onChange={setSelectedSolution}
                    options={[
                      { value: "option1", label: "Intelligent Assist" },
                      { value: "option2", label: "Email EAR" },
                      { value: "option3", label: "Case Intelligence" },
                      // other options...
                    ]}
                  />
                </div>
              </div>
              <div className={styles.formGroup}>
                <label>Feedback</label>
                <textarea
                  name="feedback"
                  value={formData.feedback}
                  onChange={handleInputChange}
                  rows="4"
                  required
                />
              </div>
            </div>
          )}

          {activeReview === 'portal' && (
            <div>
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
                <label>Feedback</label>
                <textarea
                  name="feedback"
                  value={formData.feedback}
                  onChange={handleInputChange}
                  rows="4"
                  required
                />
              </div>
            </div>
          )}

          <div className={styles.ratingGroup}>
            <label>Rate Us</label>
            <div className={styles.starRating}>
              {[1, 2, 3, 4, 5].map((star) => (
                <FontAwesomeIcon
                  key={star}
                  icon={faStar}
                  className={star <= rating ? styles.activeStar : styles.inactiveStar}
                  onClick={() => handleStarClick(star)}
                />
              ))}
            </div>
          </div>
          <button type="submit" className={styles.submitButton}>Submit</button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you for your feedback!
        </p>
      )}
    </div>
  );
};

export default FeedbackForm;









// this is my lambda code
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
        print("Event",event)
        form_data = json.loads(event['body'])
        #form_data = event #loca testing
        typee=form_data['typee']
        print(typee)
        if typee=="demo":
            response=demoform(form_data)
            return response
        else:
            response=fbform(form_data)
            return response
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
    print("hi fbform")
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
    print("hi before return")
    #return ("hi returned")
        
    return {
        'statusCode': 200,
        'headers': {
            'Access-Control-Allow-Origin': '*',  # Allow CORS for all origins
            'Access-Control-Allow-Methods': 'POST, OPTIONS',
            'Access-Control-Allow-Headers': 'Content-Type'
        },
        'body': json.dumps({'message': 'Form submitted and saved to DynamoDB successfully'})
    }


// this is my react code
import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faStar, faTimes } from "@fortawesome/free-solid-svg-icons";
import styles from "./FeedbackForm.module.css";

const FeedbackForm = ({ closeModal, formType }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);
  const [rating, setRating] = useState(0);
  const [activeReview, setActiveReview] = useState('solution'); // Track which section is active
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    feedback: '',
    typee: formType || 'feedback' // Initialize typee based on formType prop

  });

  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleStarClick = (newRating) => {
    setRating(newRating);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      await axios.post('xyz', {
        ...formData,
        rating,
        solution: selectedSolution ? selectedSolution.label : ''
      });

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting feedback:', error);
    }
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
      <button className={styles.closeButton} onClick={closeModal}>
        <FontAwesomeIcon icon={faTimes} />
      </button>
      <h2 className={styles.feedbackHead}>Provide Your Feedback</h2>

      <div className={styles.toggleButtons}>
        <button
          className={activeReview === 'solution' ? styles.activeButton : styles.inactiveButton}
          onClick={() => setActiveReview('solution')}
        >
         About Solution
        </button>
        <button
          className={activeReview === 'portal' ? styles.activeButton : styles.inactiveButton}
          onClick={() => setActiveReview('portal')}
        >
         About Portal
        </button>
      </div>

      {!isSubmitted ? (
        <form onSubmit={handleSubmit}>
          {activeReview === 'solution' && (
            <div>
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
                <label>Select Solution</label>
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
                <label>Feedback</label>
                <textarea
                  name="feedback"
                  value={formData.feedback}
                  onChange={handleInputChange}
                  rows="4"
                  required
                />
              </div>
            </div>
          )}

          {activeReview === 'portal' && (
            <div>
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
                <label>Feedback</label>
                <textarea
                  name="feedback"
                  value={formData.feedback}
                  onChange={handleInputChange}
                  rows="4"
                  required
                />
              </div>
            </div>
          )}

          <div className={styles.ratingGroup}>
            <label>Rate Us</label>
            <div className={styles.starRating}>
              {[1, 2, 3, 4, 5].map((star) => (
                <FontAwesomeIcon
                  key={star}
                  icon={faStar}
                  className={star <= rating ? styles.activeStar : styles.inactiveStar}
                  onClick={() => handleStarClick(star)}
                />
              ))}
            </div>
          </div>
          <button type="submit" className={styles.submitButton}>Submit</button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you for your feedback!
        </p>
      )}
    </div>
  );
};

export default FeedbackForm;
