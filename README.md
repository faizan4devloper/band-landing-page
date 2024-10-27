.faqDropdown {
  border: 1px solid #ddd;
  margin-top: 20px;
  border-radius: 5px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
}

.dropdownHeader {
  display: flex;
  width: 100%;
  justify-content: space-between;
  padding: 15px 20px; /* Added extra padding */
  background-color: #f5f5f5;
  cursor: pointer;
  font-weight: bold;
  font-size: 1.2rem; /* Increased font size */
  color: #333; /* Darker text color */
  border-bottom: 1px solid #ddd;
  transition: background-color 0.3s;
}

.dropdownHeader:hover {
  background-color: #eaeaea; /* Background color on hover */
}

.dropdownContent {
  padding: 10px 20px; /* Updated padding */
  max-height: 300px; /* Optional: Set a max height for dropdown */
  overflow-y: auto; /* Scroll if content exceeds max height */
  border-top: 1px solid #ddd; /* Optional: Aesthetic separation */
}

.questionBlock {
  padding: 12px;
  border-bottom: 1px solid #ddd;
  cursor: pointer;
  transition: background-color 0.3s;
  font-size: 1rem; /* Adjust font size */
  color: #555; /* Slightly lighter color for questions */
}

.questionBlock:hover {
  background-color: #f0f0f0; /* Highlight on hover */
}

.questionBlock.active {
  background-color: #d0e8ff; /* Color for active question */
  font-weight: bold; /* Make active question bold */
}

