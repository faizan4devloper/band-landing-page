import React, { useState } from "react";
import styles from "./CategorySidebar.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";

const CategorySidebar = ({ categories, onFilterChange }) => {
  const [openCategory, setOpenCategory] = useState(null);
  const [activeItem, setActiveItem] = useState({ category: null, item: null });

  const toggleCategory = (index) => {
    setOpenCategory(index === openCategory ? null : index);
  };

  const handleItemClick = (category, item) => {
    setActiveItem({ category, item });
    onFilterChange(category, item);
  };

  return (
    <div className={styles.sidebar}>
      <p className={styles.sideHead}>Explore By:</p>
      {categories.map((category, index) => (
        <div key={index} className={styles.category}>
          <div
            className={`${styles.categoryHeader} ${openCategory === index ? styles.activeCategory : ""}`}
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
                  className={`${styles.dropdownItem} ${activeItem.category === category.name && activeItem.item === item ? styles.activeItem : ""}`}
                  onClick={() => handleItemClick(category.name, item)}
                >
                  <label htmlFor={`${category.name}-${item}`}>{item}</label>
                  <input
                    type="checkbox"
                    id={`${category.name}-${item}`}
                    checked={activeItem.category === category.name && activeItem.item === item}
                    onChange={() => handleItemClick(category.name, item)}
                  />
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
  left: 135px;
  width: 200px;
  height: calc(100% - 75px);
  border-right: 1px solid rgba(219, 197, 255, 1);
  padding: 20px;
  padding-right: 0px;
  overflow-y: auto;
}

.category {
  margin-bottom: 20px;
}

.sideHead {
  margin-top: 0;
  font-weight: 500;
  margin-left: 15px;
}

.categoryHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  padding: 10px 15px;
  border-radius: 8px 0 0 8px;
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
  display: flex;
  align-items: center;
  justify-content: space-between; /* Align items and checkbox to the right */
}

.dropdownItem:hover {
  background-color: rgba(220, 220, 220, 1);
}

.categoryHeader:not(.activeCategory):hover {
  background-color: rgba(230, 235, 245, 1);
  border-radius: 8px 0 0 8px;
}

.activeCategory {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 8px 0 0 8px;
}

.activeItem {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 4px;
}

.dropdownItem input[type="checkbox"] {
  margin-left: 8px; /* Adjust spacing between label and checkbox */
}
