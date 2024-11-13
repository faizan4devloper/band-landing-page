/* Define color variables for consistent styling */
:root {
  --primary-color: #0f5fdc;
  --background-color: #f9f9f9;
  --section-background: #ffffff;
  --highlight-background: #f4f9ff;
  --badge-background: #e0f3ff;
  --text-color: #333;
  --border-radius: 8px;
  --shadow-light: 0 4px 8px rgba(0, 0, 0, 0.05);
  --shadow-medium: 0 8px 16px rgba(0, 0, 0, 0.1);
}

.mainContent {
  padding: 20px;
  background-color: var(--background-color);
  border-radius: var(--border-radius);
  max-width: 800px;
  margin: 0 auto;
  box-shadow: var(--shadow-medium);
}

/* Section Wrapper */
.section {
  margin-bottom: 20px;
  padding: 20px;
  background: var(--section-background);
  border-radius: var(--border-radius);
  box-shadow: var(--shadow-light);
  border-left: 4px solid var(--primary-color);
}

/* Section Heading */
.sectionHead {
  font-size: 1.3em;
  font-weight: bold;
  color: var(--text-color);
  margin-bottom: 15px;
  padding-bottom: 5px;
  border-bottom: 2px solid var(--primary-color);
}

/* Common List Styles */
.listContainer {
  margin-top: 10px;
}

/* Skills Badges (Reusable for Existing and Suggested Skills) */
.badge {
  display: inline-block;
  padding: 8px 12px;
  background-color: var(--badge-background);
  color: var(--primary-color);
  border-radius: 15px;
  font-size: 0.9em;
  font-weight: 500;
  transition: transform 0.2s;
  margin: 4px;
}

.badge:hover {
  transform: scale(1.05);
}

/* Job Posting Link */
.jobUrl {
  text-decoration: none;
  color: var(--primary-color);
  font-weight: bold;
  display: inline-flex;
  align-items: center;
  padding: 8px 12px;
  border: 2px solid var(--primary-color);
  border-radius: var(--border-radius);
  transition: background-color 0.3s, color 0.3s;
  font-size: 0.95em;
}

.jobUrl:hover {
  background-color: var(--primary-color);
  color: #ffffff;
}

.icon {
  margin-right: 8px;
}

/* Resume Highlights and Success Stories */
.highlight, .successStory {
  background-color: var(--highlight-background);
  padding: 15px;
  border-radius: var(--border-radius);
  color: var(--text-color);
  font-size: 0.95em;
  line-height: 1.5;
  margin-top: 10px;
}

/* Success Story List */
.successStoryList {
  margin-top: 10px;
}

/* Spinner Container */
.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
