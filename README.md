import React, { useState } from "react";
import styles from "./CategorySidebar.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";

const CategorySidebar = ({ categories, onFilterChange }) => {
  const [openCategory, setOpenCategory] = useState(null);

  const toggleCategory = (index) => {
    setOpenCategory(index === openCategory ? null : index);
  };

  return (
    <div className={styles.sidebar}>
      {categories.map((category, index) => (
        <div key={index} className={styles.category}>
          <div 
            className={styles.categoryHeader} 
            onClick={() => toggleCategory(index)}
          >
            {category.name}
            <FontAwesomeIcon 
              icon={openCategory === index ? faChevronUp : faChevronDown} 
              className={styles.chevronIcon} 
            />
          </div>
          {openCategory === index && (
            <div className={styles.dropdown}>
              {category.items.map((item, itemIndex) => (
                <div 
                  key={itemIndex} 
                  className={styles.dropdownItem} 
                  onClick={() => onFilterChange(category.name, item)}
                >
                  {item}
                </div>
              ))}
            </div>
          )}
        </div>
      ))}
    </div>
  );
};

export default CategorySidebar;

.sidebar {
  position: fixed;
  top: 110px;
  left: 115px;
  width: 200px;
  height: calc(100% - 75px);
  /*background-color: rgba(240, 240, 240, 1);*/
  border-right: 1px solid rgba(219, 197, 255, 1);
  /*box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);*/
  padding: 20px;
  overflow-y: auto;
}

.category {
  margin-bottom: 20px;
}

.categoryHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  padding: 10px 15px;
  background-color: rgba(230, 230, 230, 1);
  border-radius: 4px;
}

.chevronIcon {
  margin-left: 10px;
}

.dropdown {
  padding: 10px;
  background-color: rgba(250, 250, 250, 1);
  border-radius: 4px;
  margin-top: 10px;
}

.dropdownItem {
  font-size: 11px;
  padding: 5px;
  cursor: pointer;
}

.dropdownItem:hover {
  background-color: rgba(220, 220, 220, 1);
}


