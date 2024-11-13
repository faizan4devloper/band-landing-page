<section className={styles.section}>
  <h3>Job URL</h3>
  {data.joburl ? (
    <ol className={styles.jobUrlList}>
      <li>
        <a href={data.joburl} target="_blank" rel="noopener noreferrer">{data.joburl}</a>
      </li>
    </ol>
  ) : (
    <p>No job URL available</p>
  )}
</section>
