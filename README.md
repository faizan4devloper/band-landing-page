const renderContent = (content, altText) => {
  // Check if the content exists and is a valid URL (image URL)
  if (content && content.startsWith('http')) {
    return (
      <div className={styles.imageWrapper}>
        <img
          src={content}
          alt={altText}
          className={styles.responseImage}
          onClick={() => handleImageClick(content)} // Open modal on image click
        />
        <span className={styles.tooltip}>Click to View Larger</span>
      </div>
    );
  } else {
    // If no image, display a beautiful message instead of an error or blank space
    return (
      <div className={styles.noImageMessage}>
        <p>{content ? "No image available for this content." : "No content available."}</p>
      </div>
    );
  }
};









.noImageMessage {
  padding: 10px;
  margin-top: 10px;
  background-color: #f2f2f2; /* Light background to highlight the message */
  border-radius: 5px;
  color: #333;
  font-size: 14px;
  text-align: center;
}

.noImageMessage p {
  margin: 0;
  font-weight: 500;
}
