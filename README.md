<section className={styles.section}>
  <p className={styles.sectionHead}>Success Stories</p>
  {Object.keys(data.success_stories).length > 0 ? (
    <ul>
      {Object.keys(data.success_stories).map((storyKey, index) => (
        <li key={index} className={styles.successStoryCardWrapper}>
          <a
            href={data.success_stories[storyKey]?.pdf_link || '#'}
            target="_blank"
            rel="noopener noreferrer"
            className={styles.successStoryCard}
          >
            <div className={styles.maskEffect}>
              <strong>{storyKey}:</strong> {JSON.stringify(data.success_stories[storyKey]?.content)}
            </div>
          </a>
        </li>
      ))}
    </ul>
  ) : (
    <p>No success stories available</p>
  )}
</section>








/* Container for the success story cards */
.successStoryCardWrapper {
  position: relative;
  margin-bottom: 10px;
  cursor: pointer;
}

/* Mask effect that appears on hover */
.successStoryCard {
  position: relative;
  background: #f1f8ff;
  padding: 10px;
  border-radius: 8px;
  border-left: 4px solid #0f5fdc;
  transition: transform 0.3s ease;
  overflow: hidden;
}

.successStoryCard .maskEffect {
  position: relative;
  z-index: 1;
}

/* The overlay effect that covers the card */
.successStoryCard:hover::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.4);
  z-index: 2;
}

.successStoryCard:hover {
  transform: scale(1.05);
}

.successStoryCard a {
  text-decoration: none;
  color: inherit;
}

.successStoryCard a:hover {
  color: inherit;
}

/* Styling for the success story link */
.successStoryCard a {
  color: #0f5fdc;
  font-weight: bold;
  text-decoration: none;
  border: 1px solid #0f5fdc;
  border-radius: 6px;
  padding: 8px 12px;
  transition: background-color 0.3s, color 0.3s;
}

.successStoryCard a:hover {
  background-color: #0f5fdc;
  color: white;
}
