<section className={styles.section}>
  <p className={styles.sectionHead}>Success Stories</p>
  {Object.keys(data.success_stories).length > 0 ? (
    <ul>
      {Object.keys(data.success_stories).map((storyKey, index) => {
        const storyValue = data.success_stories[storyKey];
        
        // Check if the story value is a valid URL
        const isUrl = (str) => {
          const pattern = /^(https?:\/\/[^\s]+)$/;
          return pattern.test(str);
        };

        return (
          <li key={index}>
            <strong>{storyKey}:</strong>{" "}
            {isUrl(storyValue) ? (
              // If it's a URL, make it clickable
              <a href={storyValue} target="_blank" rel="noopener noreferrer" className={styles.link}>
                {storyValue}
              </a>
            ) : (
              // If it's not a URL, just display the text
              storyValue
            )}
          </li>
        );
      })}
    </ul>
  ) : (
    <p>No success stories available</p>
  )}
</section>




/* Style for success story links */
.link {
  text-decoration: none;
  color: #0f5fdc;
  font-weight: bold;
  padding: 4px 8px;
  border: 1px solid #0f5fdc;
  border-radius: 6px;
  transition: background-color 0.3s, color 0.3s;
}

.link:hover {
  background-color: #0f5fdc;
  color: #fff;
}
