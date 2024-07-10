<Carousel
        showArrows={false}
        showThumbs={false}
        showIndicators={false}
        infiniteLoop={true}
        autoPlay={true}
        interval={2000}
        stopOnHover={true}
        className={styles.customIndicator} // Apply the customIndicator class
      >
        <div
          className={styles.carouselItem}
          style={{
            background:
              "linear-gradient(90deg, #6F36CD 0%, rgba(31, 119, 246, 0.27) 100%)",
          }}
        >
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2>
              AWS<span>Gen AI Pilots</span>
            </h2>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel6} className={styles.carouselImage6} />
          <div className={styles.carouselOverlay6}></div> {/* Add overlay */}
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        
        <div className={styles.carouselItem}>
          <img src={imgCarousel3} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel4} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel5} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2>
              AWS EBU<span>Gen AI Pilots</span>
            </h2>
          </div>
        </div>
      </Carousel> they on after hover off
