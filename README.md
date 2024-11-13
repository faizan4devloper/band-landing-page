 <section className={styles.section}>
        <h3>Job URL</h3>
        {data.joburl ? (
          <a href={data.joburl} target="_blank" rel="noopener noreferrer">{data.joburl}</a>
        ) : (
          <p>No job URL available</p>
        )}
      </section>
