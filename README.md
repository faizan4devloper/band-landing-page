const renderResponsePoints = (responseArray) => {
  const arrayToRender = Array.isArray(responseArray) ? responseArray : [responseArray];
  
  return (
    <ul className={styles.responseList}>
      {arrayToRender.map((item, index) => {
        // Check if the item is a valid URL (specifically for PDFs)
        const isPdfLink = item.startsWith('http') && item.endsWith('.pdf');
        
        return (
          <li key={index} className={styles.responseItem}>
            {isPdfLink ? (
              // Mask the PDF link with "View PDF" text
              <a href={item} target="_blank" rel="noopener noreferrer" className={styles.link}>
                View PDF
              </a>
            ) : (
              parseLinks(item)  // Use the existing parseLinks for other URLs
            )}
          </li>
        );
      })}
    </ul>
  );
};
