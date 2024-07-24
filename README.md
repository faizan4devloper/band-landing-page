import React, { useState } from "react";
import styles from "./CategorySidebar.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp, faTimes } from "@fortawesome/free-solid-svg-icons";

const CategorySidebar = ({ categories, onFilterChange }) => {
  const [openCategory, setOpenCategory] = useState(null);
  const [activeItems, setActiveItems] = useState({});

  const toggleCategory = (index) => {
    setOpenCategory(index === openCategory ? null : index);
  };

  const handleItemClick = (category, item) => {
    let updatedActiveItems = { ...activeItems };

    if (item === "All") {
      updatedActiveItems[category] = ["All"];
    } else {
      const isActive = activeItems[category]?.includes(item);
      if (isActive) {
        updatedActiveItems[category] = activeItems[category].filter((i) => i !== item);
      } else {
        updatedActiveItems[category] = [
          ...(activeItems[category] || []).filter((i) => i !== "All"),
          item,
        ];
      }
    }

    if (updatedActiveItems[category].length === 0) {
      delete updatedActiveItems[category];
    }

    setActiveItems(updatedActiveItems);
    onFilterChange(updatedActiveItems);
  };

  const handleRemoveFilter = (category, item) => {
    const updatedActiveItems = {
      ...activeItems,
      [category]: activeItems[category].filter((i) => i !== item),
    };
    setActiveItems(updatedActiveItems);
    onFilterChange(updatedActiveItems);
  };

  return (
    <div className={styles.sidebar}>
      <p className={styles.sideHead}>Explore by</p>
      <div className={styles.selectedFilters}>
        {Object.entries(activeItems).flatMap(([category, items]) =>
          items.map((item) => (
            <div key={`${category}-${item}`} className={styles.selectedFilter}>
              <span>{item}</span>
              <FontAwesomeIcon
                icon={faTimes}
                className={styles.removeIcon}
                onClick={() => handleRemoveFilter(category, item)}
              />
            </div>
          ))
        )}
      </div>
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
              {category.items.includes("All") || (
                <div
                  className={styles.dropdownItem}
                  onClick={() => handleItemClick(category.name, "All")}
                >
                  <input
                    type="checkbox"
                    checked={activeItems[category.name]?.includes("All")}
                    readOnly
                    className={styles.checkbox}
                  />
                  <span className={styles.itemText}>All</span>
                </div>
              )}
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
