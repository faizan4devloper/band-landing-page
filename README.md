.questionBlock {
  display: grid;
  grid-template-areas:
    "question question"
    "leftSection rightSection"
    "bottomLeftSection bottomRightSection";
  grid-gap: 20px;
  position: relative;
  margin: 20px 0;
  border-radius: 8px;
  padding: 15px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.question {
  grid-area: question;
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

.responseSection {
  padding: 15px;
  font-size: 1rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  position: relative;
  overflow: hidden;
  transition: max-height 0.4s ease, opacity 0.4s ease;
  max-height: 180px; /* Adjusted collapsed height */
  width: 100%; /* Ensures consistent width across sections */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  opacity: 1;
}

.responseSection.expanded {
  max-height: 400px; /* Adjusted expanded height for better readability */
  overflow-y: auto;
  opacity: 1;
}

.responseList {
  margin: 10px 0;
  padding-left: 20px;
}

.responseItem {
  margin-bottom: 8px;
  font-size: 0.9rem;
  line-height: 1.6;
}

.link {
  color: #0073e6;
  text-decoration: underline;
  transition: color 0.2s;
}

.link:hover {
  color: #005bb5;
}

.seeMoreButton {
  display: inline-block;
  margin-top: 10px;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background-color 0.2s;
}

.seeMoreButton:hover {
  background-color: #005bb5;
}

/* Optional: Customize scrollbar appearance */
.responseSection.expanded::-webkit-scrollbar {
  width: 6px;
}

.responseSection.expanded::-webkit-scrollbar-thumb {
  background-color: rgba(15, 95, 220, 1);
  border-radius: 10px;
}
