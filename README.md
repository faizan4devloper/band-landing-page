/* Redesigned MainContent.module.css */

.mainContentContainer {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 20px;
  padding: 20px;
  background: #f9f9f9;
  border-radius: 12px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
}

.leftSection {
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.08);
}

.rightSection {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.sectionCard {
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.08);
  transition: transform 0.2s ease-in-out;
}

.sectionCard:hover {
  transform: translateY(-3px);
}

.headerTitle {
  font-size: 1.5rem;
  font-weight: 600;
  color: #333;
  margin-bottom: 10px;
}

.button {
  padding: 12px 18px;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 500;
  border: none;
  cursor: pointer;
  transition: background 0.3s ease;
}

.verifyButton {
  background: #007bff;
  color: white;
}

.verifyButton:hover {
  background: #0056b3;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .mainContentContainer {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .button {
    width: 100%;
    text-align: center;
  }
}
