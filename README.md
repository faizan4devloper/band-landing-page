import React, { useState } from "react";
import styles from "./CategorySidebar.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";

const CategorySidebar = ({ categories, onFilterChange }) => {
  const [openCategory, setOpenCategory] = useState(null);
  const [activeItems, setActiveItems] = useState({});

  const toggleCategory = (index) => {
    setOpenCategory(index === openCategory ? null : index);
  };

  const handleItemClick = (category, item) => {
    const isActive = activeItems[category]?.includes(item);
    const updatedActiveItems = {
      ...activeItems,
      [category]: isActive
        ? activeItems[category].filter(i => i !== item)
        : [...(activeItems[category] || []), item],
    };
    setActiveItems(updatedActiveItems);
    onFilterChange(category, updatedActiveItems[category]);
  };

  return (
    <div className={styles.sidebar}>
      <p className={styles.sideHead}>Explore by</p>
      {categories.map((category, index) => (
        <div key={index} className={styles.category}>
          <div
            className={`${styles.categoryHeader} ${openCategory === index ? styles.activeCategory : ""}`}
            onClick={() => toggleCategory(index)}
          >
            <img
              src={category.svgIcon}
              alt={`${category.name} icon`}
              className={`${styles.svgIcon} ${openCategory === index ? styles.activeIcon : ""}`}
            />
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
                  onClick={() => handleItemClick(category.name, item)}
                >
                  <input
                    type="checkbox"
                    checked={activeItems[category.name]?.includes(item)}
                    readOnly
                    className={styles.checkbox}
                  />
                  <span className={styles.itemText}>{item}</span>
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
  top: 140px;
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
  font-size: 14px;
  font-weight: 500;
  margin-top: 0;
  /*margin-left: 15px;*/
  color: #6f36cd; /* Adjust the color as desired */
  position: relative;
  display: inline-block;
  padding-bottom: 5px;
}

.sideHead::after {
  content: '';
  display: block;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  position: absolute;
  bottom: 0;
  left: 0;
}

.categoryHeader {
  display: flex;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  padding: 10px 15px;
  border-radius: 8px 0 0 8px;
  background-color: rgba(230, 235, 245, 1);
}

.categoryHeader img {
  margin-right: 8px; /* Adjust spacing between icon and text */
}

.chevronIcon {
  margin-left: auto; /* Pushes the chevron icon to the right */
}

.dropdown {
  padding: 10px;
  background-color: rgba(250, 250, 250, 1);
  border-radius: 4px;
  margin-top: 10px;
}

.dropdownItem {
  display: flex;
  align-items: center;
  font-size: 11px;
  padding: 5px;
  cursor: pointer;
  text-align: left;
}

.checkbox {
  margin-right: 10px;
  width: 16px;
  height: 16px;
}

.itemText {
  margin-left: 5px;
}

.dropdownItem:hover {
  background-color: rgba(220, 220, 220, 1);
  border-radius: 4px;
  color: #5F1EC1;
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

.svgIcon {
  width: 16px;
  height: 16px;
}

.svgIcon:hover {
  fill: #5F1EC1;
}

.activeIcon {
  filter: brightness(0) invert(1);
}
