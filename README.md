import json
import boto3
from botocore.exceptions import ClientError

# Initialize the DynamoDB client
dynamodb = boto3.resource('dynamodb')

# Replace 'RequestDemoSubmissions' with your actual table name
table = dynamodb.Table('RequestDemoSubmissions')

def lambda_handler(event, context):
    try:
        # Extract the form data from the event object
        form_data = json.loads(event['body'])
        userName = form_data['userName']
        email = form_data['email']
        solution = form_data['solution']
        domain = form_data['domain']
        customerName = form_data['customerName']
        details = form_data['details']

        # Save the form data to DynamoDB
        response = table.put_item(
            Item={
                'email': email,  # Primary Key
                'userName': userName,
                'solution': solution,
                'domain': domain,
                'customerName': customerName,
                'details': details
            }
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




import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

const RequestDemoForm = ({ closeModal }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    solution: '',
    domain: '',
    customerName: '',
    details: '',
  });

  const [selectedSolution, setSelectedSolution] = useState(null);

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setFormData({
      ...formData,
      [name]: value,
    });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    // Prepare the data to be sent to the Lambda function
    const dataToSend = {
      ...formData,
      solution: selectedSolution ? selectedSolution.label : '',
    };

    try {
      // Send the form data to the Lambda function through the API Gateway
      await axios.post('https://75x831r7ea.execute-api.us-east-1.amazonaws.com/v1', dataToSend);

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting form:', error);
      // Handle error case, you can display an error message here
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
            <label>Details</label>
            <textarea
              name="details"
              value={formData.details}
              onChange={handleInputChange}
              rows="4"
              placeholder="Enter additional details here..."
            />
          </div>
          <div className={styles.formGroup}>
            <button type="submit" className={styles.submitButton}>
              Submit
            </button>
          </div>
        </form>
      ) : (
        <div className={styles.thankYouMessage}>
          <h3>Thank you!</h3>
          <p>Your request has been submitted successfully.</p>
          <button className={styles.closeButton} onClick={handleCloseModal}>
            Close
          </button>
        </div>
      )}
    </div>
  );
};

export default RequestDemoForm;







Access to XMLHttpRequest at 'https://75x831r7ea.execute-api.us-east-1.amazonaws.com/v1' from origin 'https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
RequestDemoForm.js:38 Error submitting form: AxiosError
handleSubmit @ RequestDemoForm.js:38
75x831r7ea.execute-api.us-east-1.amazonaws.com/v1:1 
        
        
       Failed to load resource: net::ERR_FAILED


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
    details: ''
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
      await axios.post('https://75x831r7ea.execute-api.us-east-1.amazonaws.com/v1', {
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


{
  "Access-Control-Allow-Origin": "*",
  "Access-Control-Allow-Methods": "POST, GET, OPTIONS",
  "Access-Control-Allow-Headers": "Content-Type, Authorization"
}




import json

def lambda_handler(event, context):
    try:
        # Extract the form data from the event object
        if 'body' in event:
            form_data = json.loads(event['body'])
            userName = form_data.get('userName')
            email = form_data.get('email')
            solution = form_data.get('solution')
            domain = form_data.get('domain')
            customerName = form_data.get('customerName')
            details = form_data.get('details')

            # Process the form data as needed
            print('Form data:', {
                'userName': userName,
                'email': email,
                'solution': solution,
                'domain': domain,
                'customerName': customerName,
                'details': details
            })

            # Return a successful response
            return {
                'statusCode': 200,
                'headers': {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*',
                },
                'body': json.dumps({'message': 'Form submitted successfully'})
            }
        else:
            raise ValueError("No body in event")
            
    except Exception as e:
        print('Error processing form:', e)

        # Return an error response
        return {
            'statusCode': 500,
            'headers': {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
            },
            'body': json.dumps({'message': 'Error submitting form'})
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
    userName: "",
    email: "",
    domain: "",
    customerName: "",
    details: "",
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prevData) => ({
      ...prevData,
      [name]: value,
    }));
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    // Prepare the data for submission
    const submissionData = {
      ...formData,
      solution: selectedSolution ? selectedSolution.label : "",
    };

    try {
      // Send the form data to the Lambda function through the API Gateway
      await axios.post('https://75x831r7ea.execute-api.us-east-1.amazonaws.com/v1', submissionData);
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
        border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      },
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected
        ? "#5f1ec1"
        : state.isFocused
        ? "#eee"
        : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px", // Option font size
      cursor: "pointer",
      "&:hover": {
        backgroundColor: state.isSelected ? "#5f1ec1" : "#f0f0f0", // Change hover background color
      },
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px", // Placeholder font size
      color: "#999", // Placeholder color
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
      overflowY: "auto", // Enable vertical scrolling when necessary
      zIndex: 2, // Ensure it is above other elements
    }),
    menuList: (provided) => ({
      ...provided,
      maxHeight: "150px",
      padding: 0, // Remove padding to avoid additional scrolling issues
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#555", // Single value color
    }),
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
              onChange={handleChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>*Email Address</label>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleChange}
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
              onChange={handleChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>Customer Name</label>
            <input
              type="text"
              name="customerName"
              value={formData.customerName}
              onChange={handleChange}
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
              onChange={handleChange}
              required
            ></textarea>
          </div>
          <button type="submit" className={styles.submitButton}>
            Submit
          </button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you! Your request for a live demo has been submitted
          successfully.
        </p>
      )}
    </div>
  );
};

export default RequestDemoForm;
import json

def lambda_handler(event, context):
    try:
        # Extract the form data from the event object
        form_data = json.loads(event['body'])
        userName = form_data['userName']
        email = form_data['email']
        solution = form_data['solution']
        domain = form_data['domain']
        customerName = form_data['customerName']
        details = form_data['details']

        # Process the form data as needed
        print('Form data:', {
            'userName': userName,
            'email': email,
            'solution': solution,
            'domain': domain,
            'customerName': customerName,
            'details': details
        })

        # Return a successful response
        return {
            'statusCode': 200,
            'body': json.dumps({'message': 'Form submitted successfully'})
        }
    except Exception as e:
        print('Error processing form:', e)

        # Return an error response
        return {
            'statusCode': 500,
            'body': json.dumps({'message': 'Error submitting form'})
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
  
  
const handleSubmit = async (e) => {
  e.preventDefault();

  // Get the form data
  const formData = {
    userName: 'John Doe', // Replace with the actual user name from the form
    email: 'johndoe@example.com', // Replace with the actual email from the form
    solution: selectedSolution.label, // Get the selected solution label
    domain: 'Example Domain', // Replace with the actual domain from the form
    customerName: 'Example Customer', // Replace with the actual customer name from the form
    details: 'This is the additional details provided in the form.' // Replace with the actual details from the form
  };

  try {
    // Send the form data to the Lambda function through the API Gateway
    await axios.post('https://75x831r7ea.execute-api.us-east-1.amazonaws.com/v1', formData);
    
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
            <input type="text" required />
          </div>
          <div className={styles.formGroup}>
            <label>*Email Address</label>
            <input type="email" required />
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
            <input type="text" required />
          </div>
          <div className={styles.formGroup}>
            <label>Customer Name</label>
            <input type="text" required />
          </div>
          <div className={styles.formGroup}>
            <label>More Details</label>
            <textarea
              placeholder="Enter your business details and scope of this demo in your use case."
              rows="4"
              required
            ></textarea>
          </div>
          <button type="submit" className={styles.submitButton}>
            Submit
          </button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you! Your request for a live demo has been submitted
          successfully.
        </p>
      )}
    </div>
  );
};

export default RequestDemoForm;
