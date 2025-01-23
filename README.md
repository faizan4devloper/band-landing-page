import React, { useState } from 'react';
import PropTypes from 'prop-types';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faChevronDown, 
  faFolder, 
  faCheck 
} from '@fortawesome/free-solid-svg-icons';
import styles from './DocumentCategorySelect.module.css';

function DocumentCategorySelect({ 
  selectedCategory, 
  onCategoryChange,
  fileTypeMappings 
}) {
  const [isOpen, setIsOpen] = useState(false);

  const defaultFileTypeMappings = {
    'aimlusecasesv1': { 
      code: 'AI', 
      description: 'AI Use Cases',
      color: '#6a11cb'
    },
    'umo-privilegestatement': { 
      code: 'PS', 
      description: 'Privilege Statement',
      color: '#2575fc'
    },
    'umo-invoice': { 
      code: 'CL', 
      description: 'Client Invoice',
      color: '#48bb78'
    },
    'umo-creditnote': { 
      code: 'MD', 
      description: 'Credit Note',
      color: '#ecc94b'
    },
    'umo-returnauthorisation': { 
      code: 'IN', 
      description: 'Return Authorization',
      color: '#ed64a6'
    },
    'umo-accountstatement': { 
      code: 'OT', 
      description: 'Account Statement',
      color: '#4299e1'
    }
  };

  const mappingsToUse = fileTypeMappings || defaultFileTypeMappings;

  const handleCategorySelect = (category) => {
    onCategoryChange(category);
    setIsOpen(false);
  };

  return (
    <div className={styles.documentCategorySelectContainer}>
      <div className={styles.selectWrapper}>
        <label className={styles.categoryLabel}>
          Document Category
        </label>
        
        <div 
          className={`${styles.categorySelect} ${isOpen ? styles.active : ''}`}
          onClick={() => setIsOpen(!isOpen)}
        >
          {selectedCategory ? (
            <div className={styles.selectedCategory}>
              <FontAwesomeIcon 
                icon={faFolder} 
                color={mappingsToUse[selectedCategory]?.color} 
                className={styles.categoryIcon}
              />
              <span className={styles.categoryName}>
                {mappingsToUse[selectedCategory]?.description || selectedCategory}
              </span>
              <span className={styles.categoryCode}>
                ({mappingsToUse[selectedCategory]?.code})
              </span>
            </div>
          ) : (
            <span className={styles.placeholder}>
              Select Document Category
            </span>
          )}
          <FontAwesomeIcon 
            icon={faChevronDown} 
            className={styles.dropdownIcon} 
          />
        </div>

        {isOpen && (
          <div className={styles.categoryDropdown}>
            {Object.entries(mappingsToUse).map(([category, details]) => (
              <div 
                key={category}
                className={`${styles.categoryOption} ${selectedCategory === category ? styles.selected : ''}`}
                onClick={() => handleCategorySelect(category)}
              >
                <FontAwesomeIcon 
                  icon={faFolder} 
                  color={details.color} 
                  className={styles.categoryIcon}
                />
                <div className={styles.categoryDetails}>
                  <span className={styles.categoryName}>
                    {details.description}
                  </span>
                  <span className={styles.categoryCode}>
                    ({details.code})
                  </span>
                </div>
                {selectedCategory === category && (
                  <FontAwesomeIcon 
                    icon={faCheck} 
                    className={styles.checkIcon} 
                  />
                )}
              </div>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}

DocumentCategorySelect.propTypes = {
  selectedCategory: PropTypes.string.isRequired,
  onCategoryChange: PropTypes.func.isRequired,
  fileTypeMappings: PropTypes.objectOf(
    PropTypes.shape({
      code: PropTypes.string.isRequired,
      description: PropTypes.string.isRequired,
      color: PropTypes.string
    })
  )
};

export default DocumentCategorySelect;



.documentCategorySelectContainer {
  max-width: 400px;
  margin: 20px auto;
  position: relative;
}

.selectWrapper {
  width: 100%;
}

.categoryLabel {
  display: block;
  font-size: 14px;
  font-weight: 600;
  color: #4a5568;
  margin-bottom: 8px;
}

.categorySelect {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: 12px 16px;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  background-color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
}

.categorySelect:hover {
  border-color: #4299e1;
  box-shadow: 0 4px 6px rgba(66, 153, 225, 0.1);
}

.categorySelect.active {
  border-color: #6a11cb;
}

.selectedCategory {
  display: flex;
  align-items: center;
  gap: 10px;
}

.categoryIcon {
  font-size: 20px;
}

.categoryName {
  font-weight: 500;
  color: #2d3748;
}

.categoryCode {
  color: #718096;
  margin-left: 5px;
  font-size: 0.9em;
}

.placeholder {
  color: #a0aec0;
}

.dropdownIcon {
  color: #a0aec0;
  transition: transform 0.3s ease;
}

.categorySelect.active .dropdownIcon {
  transform: rotate(180deg);
}

.categoryDropdown {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  max-height: 300px;
  overflow-y: auto;
  background-color: #fff;
  border: 2px solid #e2e8f0;
  border-top: none;
  border-radius: 0 0 10px 10px;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.05);
  z-index: 10;
  margin-top: 5px;
}

.categoryOption {
  display: flex;
  align-items: center;
  padding: 12px 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.categoryOption:hover {
  background-color: #f7fafc;
}

.categoryOption.selected {
  background-color: #e6f2ff;
}

.categoryDetails {
  flex-grow: 1;
  margin-left: 10px;
}

.checkIcon {
  color: #48bb78;
  margin-left: auto;
}

