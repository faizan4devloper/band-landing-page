/* Main Content Layout */
.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(1, 1fr); /* Single column for smaller screens, adjust for larger */
  gap: 20px;
  overflow-y: auto;
  scrollbar-width: thin; /* For Firefox */
  scrollbar-color: #0073e6 #f1f1f1; /* For Firefox */
}

@media (min-width: 768px) {
  .mainContent {
    grid-template-columns: repeat(2, 1fr); /* Two columns on larger screens */
  }
}

/* Section Styling */
.section {
  padding: 20px;
  font-size: 1rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 8px;
  height: auto; /* Flexibility to grow with content */
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  overflow: hidden;
  position: relative; /* For smooth transitions */
}

.section:hover {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
  transform: scale(1.02);
}

.sectionHead {
  font-size: 1.1rem;
  font-weight: bold;
  color: #572ac2;
  text-align: center;
  margin-bottom: 15px;
  text-transform: uppercase;
}

.section p {
  color: #333;
  font-size: 1rem;
  line-height: 1.5;
}

.section ul {
  padding-left: 0;
  list-style-type: none;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.section ul li {
  display: flex;
  align-items: center;
  gap: 8px;
}

.icon {
  margin-right: 8px;
}

.successStoryCard {
  background: #f1f8ff;
  padding: 12px;
  border-radius: 8px;
  margin-bottom: 10px;
  border-left: 4px solid #0f5fdc;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.storyNo {
  font-size: 14px;
  font-weight: bold;
  color: #572ac2;
  text-align: center;
  margin-bottom: 10px;
}

/* Success Stories */
.successStoriesList li {
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 8px;
  margin-bottom: 15px;
  border-left: 4px solid #0073e6;
}

.pdfLinkWrapper {
  margin-top: 12px;
}

.pdfLink {
  text-decoration: none;
  background-color: #0073e6;
  color: #fff;
  padding: 10px 15px;
  border-radius: 5px;
  display: inline-block;
  font-weight: bold;
  transition: background-color 0.3s, color 0.3s;
}

.pdfLink:hover {
  background-color: #005bb5;
}

.pdfLink:focus {
  outline: none;
  box-shadow: 0 0 5px #005bb5;
}

/* Skills and Job Links */
.skillsList, .suggestedSkillsList {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.skillBadge, .suggestedSkillBadge {
  padding: 8px 12px;
  background-color: #e0f3ff;
  color: #0f5fdc;
  border-radius: 20px;
  font-size: 0.9rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.jobUrlList li a {
  text-decoration: none;
  color: #0f5fdc;
  font-weight: bold;
  padding: 8px 12px;
  border: 1px solid #0f5fdc;
  border-radius: 8px;
  transition: background-color 0.3s, color 0.3s;
  display: inline-flex;
  align-items: center;
  gap: 5px;
}

.jobUrlList li a:hover {
  background-color: #0f5fdc;
  color: #fff;
}

/* Loader */
.spinnerContainer {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100%;
  z-index: 999;
}

/* Custom Scrollbar */
.mainContent::-webkit-scrollbar {
  width: 8px;
}

.mainContent::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: #0073e6;
  border-radius: 10px;
  border: 2px solid #f1f1f1;
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: #005bb5;
}

/* Custom Scrollbar for Firefox */
.scrollbarCustom {
  scrollbar-width: thin;
  scrollbar-color: #0073e6 #f1f1f1;
}

