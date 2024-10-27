.faqDropdown {
  border: 1px solid #ddd;
  margin-top: 20px;
  border-radius: 8px;
  overflow: hidden; /* Ensures children don't overflow the parent */
  transition: box-shadow 0.3s ease; /* Smooth transition for shadow */
}

.faqDropdown:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Elevate on hover */
}

.dropdownHeader {
  display: flex;
  justify-content: space-between;
  align-items: center; /* Center align items vertically */
  padding: 15px 20px;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%); /* Gradient background */
  color: #fff; /* White text color */
  cursor: pointer;
  font-weight: bold;
  border-radius: 8px 8px 0 0; /* Rounded corners on top */
  transition: background-color 0.3s ease; /* Smooth background color transition */
}

.dropdownHeader:hover {
  background: linear-gradient(90deg, rgb(75, 20, 173) 0%, rgb(5, 75, 200) 100%); /* Darken gradient on hover */
}

.dropdownContent {
  padding: 10px 20px;
  border-top: 1px solid #ddd; /* Separate header from content */
  background-color: #fff; /* White background for content */
  max-height: 0; /* Collapsed height */
  overflow: hidden; /* Prevent overflow */
  transition: max-height 0.5s ease, opacity 0.5s ease; /* Smooth max-height transition */
  opacity: 0; /* Initially hidden */
}

.show {
  max-height: 500px; /* Arbitrary value for max height */
  opacity: 1; /* Fully visible */
}

.hide {
  max-height: 0; /* Collapsed height */
  opacity: 0; /* Hidden */
}

.questionBlock {
  padding: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease; /* Smooth transitions for background and transform */
  border-radius: 4px; /* Rounded corners */
}

.questionBlock:hover {
  background-color: rgba(95, 30, 193, 0.1); /* Light purple on hover */
  transform: translateY(-2px); /* Slight lift effect */
}

.activeQuestion {
  background-color: rgba(15, 95, 220, 0.2); /* Highlight active question */
  font-weight: bold; /* Emphasize selected question */
}

.selectedQuestionBlock {
  padding: 20px;
  margin-top: 20px; /* Increased space between dropdown and selected question */
  border: 1px solid rgb(95, 30, 193); /* Use main theme color */
  border-radius: 5px;
  background-color: #f8f9fa; /* Light background for selected question */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Added shadow for depth */
}

