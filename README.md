.noResultsContainer {
  grid-column: span 4;
  text-align: center;
  padding: 40px;
  background-color: #f4f4f4; /* Lighter background for contrast */
  border: 2px dashed #ddd; /* Dashed border to highlight the section */
  border-radius: 8px; /* Rounded corners for a modern look */
  color: #666; /* Slightly darker text color for better readability */
  font-size: 16px; /* Adjust font size for balance */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
}

.noResultsImage {
  width: 100px; /* Slightly larger image */
  height: auto;
  margin-bottom: 20px; /* Space between image and text */
}

.noResults {
  font-size: 24px; /* Larger font size for emphasis */
  color: #333; /* Darker text color for contrast */
  margin: 0;
}

.noResults p {
  font-size: 18px; /* Slightly larger font size for additional message */
  color: #555; /* A softer color for additional message text */
  margin: 10px 0 0;
}

.noResults a {
  color: #007bff; /* Blue color for links */
  text-decoration: none; /* Remove underline */
  font-weight: bold; /* Bold text for visibility */
}

.noResults a:hover {
  text-decoration: underline; /* Underline on hover for better UX */
}




<div className={styles.noResultsContainer}>
  <img src={NoResultsImage} alt="No results" className={styles.noResultsImage} />
  <p className={styles.noResults}>No solutions found.</p>
  <p>Try adjusting your search or explore our <a href="/popular-categories">popular categories</a>.</p>
</div>
