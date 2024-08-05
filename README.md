const renderContent = (content) => {
  if (!content || content.length === 0) {
    return <div>No content available</div>;
  }
  
  if (typeof content === 'string') {
    return <div>{content}</div>;
  }

  return content.length > 1 ? renderCarousel(content) : (
    <img
      src={content[0]}
      alt="Single Image"
      className={maximizedImage === content[0] ? styles.maximized : ""}
      onClick={() => toggleMaximize(content[0])}
    />
  );
};

const renderImageOrCarousel = (images) => {
  if (!images || images.length === 0) {
    return <div>No images available</div>;
  }
  
  return renderContent(images);
};
