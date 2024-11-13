/* Styles for the main content container */
.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  overflow-y: auto;
}

/* Section styling */
.section {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  max-height: 200px;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

/* Hover effect for each section */
.section:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

/* Styling for headers in each section */
h3 {
  margin-bottom: 15px;
  font-size: 1.2rem;
  color: #333;
}

/* Styles for job URL list */
.jobUrlList {
  list-style-type: decimal;
  padding-left: 20px;
  margin: 0;
}

/* Styles for job URL link */
.jobUrlList a {
  color: #0073e6;
  text-decoration: none;
  transition: color 0.3s;
}

.jobUrlList a:hover {
  color: #005bb5;
}
