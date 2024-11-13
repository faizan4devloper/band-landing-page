.mainContent {
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  max-width: 800px;
  margin: 0 auto;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
}

.section {
  margin-bottom: 20px;
  padding: 15px;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #0f5fdc;
}

.sectionHead {
  font-size: 1.3em;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
  border-bottom: 2px solid #0f5fdc;
  padding-bottom: 5px;
}

.skillsList, .suggestedSkillsList {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}

.skillBadge, .suggestedSkillBadge {
  padding: 8px 12px;
  background-color: #e0f3ff;
  color: #0f5fdc;
  border-radius: 15px;
  font-size: 0.9em;
  font-weight: 500;
  transition: transform 0.2s;
}

.skillBadge:hover, .suggestedSkillBadge:hover {
  transform: scale(1.1);
}

.jobUrlList {
  list-style: none;
  padding: 0;
  margin-top: 10px;
}

.jobUrlList li a {
  text-decoration: none;
  color: #0f5fdc;
  font-weight: bold;
  display: inline-block;
  padding: 8px 12px;
  border: 2px solid #0f5fdc;
  border-radius: 8px;
  transition: background-color 0.3s, color 0.3s;
  font-size: 0.95em;
}

.jobUrlList li a:hover {
  background-color: #0f5fdc;
  color: #ffffff;
}

.resumeHighlights {
  background-color: #f4f9ff;
  padding: 10px;
  border-radius: 6px;
  color: #333;
  font-size: 0.95em;
  line-height: 1.5;
}

.successStoriesList {
  margin-top: 10px;
}

.successStoryCard {
  background: #f1f8ff;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 10px;
  border-left: 4px solid #0f5fdc;
  color: #333;
  line-height: 1.5;
  font-size: 0.95em;
}

.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.icon {
  margin-right: 8px;
  color: #0f5fdc;
}
