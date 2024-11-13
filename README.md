{
  Object.keys(data.success_stories).length > 0 ? (
    <ul>
      {Object.keys(data.success_stories).map((storyKey, index) => {
        const story = data.success_stories[storyKey];

        return (
          <li key={index}>
            {/* Add Story number */}
            <h3>{`Story ${index + 1}: ${story.title}`}</h3>
            <p><strong>Outcome:</strong> {story.outcome}</p>
            <p><strong>Transition:</strong> {story.transition}</p>
            <p><strong>Skills:</strong> {story.skills.join(', ')}</p>
            {story.pdf_link ? (
              <div className={styles.pdfLinkWrapper}>
                {/* Display the URL link */}
                <a
                  href={story.pdf_link}
                  target="_blank"
                  rel="noopener noreferrer"
                  className={styles.pdfLink}
                >
                  Read More
                </a>
              </div>
            ) : (
              <p>No URL link available</p>
            )}
          </li>
        );
      })}
    </ul>
  ) : (
    <p>No success stories available</p>
  )
}
