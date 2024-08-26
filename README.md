return (
  <div
    className={`${styles.mainContent} ${
      isImageMaximized ? styles.laserCursorEnabled : ""
    }`}
  >
    {contentMap[activeTab] || <div>Content not available</div>}
    {maximizedImage && (
      <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
        <FontAwesomeIcon
          icon={faTimes}
          className={styles.closeIcon}
          onClick={() => setMaximizedImage(null)}
        />
        <img
          src={maximizedImage}
          alt="Maximized view"
          className={styles.maximizedImage}
        />
      </div>
    )}
    {/* Laser cursor is only visible when isImageMaximized is true */}
    {isImageMaximized && (
      <div
        className={styles.laserCursor}
        style={{ top: `${laserPos.y}px`, left: `${laserPos.x}px` }}
      />
    )}
  </div>
);


.laserCursorEnabled .laserCursor {
  display: block; /* Ensure the laser cursor is visible when enabled */
}

.laserCursor {
  position: fixed;
  width: 20px; /* Adjust size as needed */
  height: 20px; /* Adjust size as needed */
  background-color: red; /* Red color for the laser cursor */
  border-radius: 50%; /* Make it a circle */
  pointer-events: none;
  z-index: 1500;
}
