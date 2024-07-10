.carousel .slide {
  min-width: 100%;
  margin: 0;
  height: 352px !important;
  position: relative;
  text-align: center;
}

.carouselItem {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 280px;
  margin: 70px 0;
  overflow: hidden;
  width: 85%;
  border-radius: 10px;
}

.carouselImage {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensure the image covers the entire area */
  object-position: center; /* Center the image */
}

.carouselImage6 {
  width: 100%;
  height: 100%;
  object-fit: fill; /* Ensure the image covers the entire area */
  object-position: center; /* Center the image */
}

.carouselOverlay {
  position: absolute;
  border-radius: 6px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);
  z-index: 1 !important;
}

.carouselOverlay6 {
  position: absolute;
  border-radius: 6px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 0%);
  z-index: 1 !important;
}

.carouselCaption {
  position: absolute;
  top: 50%; /* Position the caption vertically in the middle */
  left: 50%; /* Position the caption horizontally in the middle */
  transform: translate(-50%, -50%); /* Center the caption both vertically and horizontally */
  text-align: center; /* Center the text within the caption */
  color: white;
  font-family: "Poppins", sans-serif; /* Apply Poppins font family */
  z-index: 2;
}

.carouselCaption h2 {
  display: flex;
  font-weight: 600;
  font-size: 38px;
  margin: 0;
  padding: 15px;
}

.carouselCaption span {
  align-items: center;
  font-size: 16px;
  font-weight: 600;
  border-left: 3px solid rgba(255, 255, 255, 1);
  padding: 18px 10px;
  margin-left: 15px;
}

.carousel .control-dots {
  position: absolute !important;
  bottom: 0 !important;
  margin: 10px 0 !important;
  padding: 0 !important;
  text-align: center !important;
  width: 100% !important;
  z-index: 10 !important;
}

.carousel .control-dots .dot {
  cursor: pointer;
}



.formContainer {
  padding: 20px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 450px;
  margin: 0 auto;
  animation: slideIn 0.5s ease-out;
  max-height: 80vh; /* Set a max-height for the form container */
  overflow-y: auto; /* Enable vertical scrolling */
  z-index: 1000;
}

.demoHead {
  color: #5f1ec1;
  margin-bottom: 10px;
  text-align: center;
  margin-top: -25px;
  font-size: 18px;
}

.formGroup {
  margin-bottom: 10px;
  position: relative;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: #000;
  font-size: 12px;
  transition: color 0.3s;
}

input::placeholder, textarea::placeholder {
  color: #999999; /* Light gray */
  opacity: 1;
}

input, textarea {
  width: 90%;
  padding: 0px;
  border: none;
  border-bottom: 1px solid #ccc; /* Only bottom border */
  border-radius: 0;
  font-size: 12px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif; /* Apply Google Font */
}

input:focus, textarea:focus {
  border-color: #5f1ec1;
  outline: none;
}

.submitButton {
  background-color: #5f1ec1;
  margin-top: 0px;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  margin-top: 10px;
  transition: background-color 0.3s;
  margin-left: 30px;
  width: 78%; /* Make the button take the full width */
}

.closeButton {
  position: absolute;
  top: 25px;
  right: 25px;
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 24px;
  color: #aaa;
}

.closeButton:hover {
  color: #555;
}

.successMessage {
  text-align: center;
  color: #5f1ec1;
  font-size: 16px;
}

/* Custom scrollbar styling */
.formContainer::-webkit-scrollbar {
  width: 6px;
}

.formContainer::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 8px;
}

.formContainer::-webkit-scrollbar-thumb {
  background: #5f1ec1;
  border-radius: 8px;
}

.formContainer::-webkit-scrollbar-thumb:hover {
  background: #555;
}


import React, { useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css"; // Import CSS module

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./carousel3.jpg";
import imgCarousel4 from "./carousel4.jpg";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./banner-1.png";

const MyCarousel = ({ isModalOpen }) => {
  useEffect(() => {
    // Create a style tag and append it to the head
    const style = document.createElement("style");
    style.innerHTML = `
      .carousel .control-dots {
        z-index: 10 !important;
      }
      .${styles.carouselOverlay}, .${styles.carouselOverlay6} {
        z-index: 1 !important;
      }
    `;
    document.head.appendChild(style);
    return () => {
      document.head.removeChild(style);
    };
  }, []);

  return (
    <div className={styles.carouselContainer}>
      <Carousel
        showArrows={false}
        showThumbs={false}
        showIndicators={true}
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
      </Carousel>
    </div>
  );
};

export default MyCarousel;


import React, { useState } from "react";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

const RequestDemoForm = ({ closeModal }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    // Handle form submission logic here
    setIsSubmitted(true);
  };

  const handleCloseModal = () => {
    setIsSubmitted(false); // Reset submission state
    closeModal();
  };

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%", // Control width
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      color: "#555",
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px", // Option font size
      width: "100%", // Option width
      cursor: "pointer",
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px", // Placeholder font size
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
    }),
  };

  return (
    <div className={styles.formContainer}>
      <button className={styles.closeButton} onClick={handleCloseModal}>
        <FontAwesomeIcon icon={faTimes} />
      </button>
      <h2 className={styles.demoHead}>Request for a Live Demo</h2>
      {!isSubmitted ? (
        <form onSubmit={handleSubmit}>
          <div className={styles.formGroup}>
            <label>*User Name</label>
            <input type="text" required />
          </div>
          <div className={styles.formGroup}>
            <label>*Email Address</label>
            <input type="email" required />
          </div>
          <div className={styles.formGroup}>
            <label>GenAI Solution Name</label>
            <div className={styles.customSelect}>
              <Select
                styles={customStyles}
                value={selectedSolution}
                onChange={setSelectedSolution}
                options={[
                  { value: "option1", label: "Email EAR" },
                  { value: "option2", label: "Code GReat" },
                  { value: "option3", label: "Case Intelligence" },
                ]}
              />
            </div>
          </div>
          <div className={styles.formGroup}>
            <label>Domain</label>
            <input type="text" required />
          </div>
          <div className={styles.formGroup}>
            <label>Customer Name</label>
            <input type="text" required />
          </div>
          <div className={styles.formGroup}>
            <label>More Details</label>
            <textarea
              placeholder="Enter your business details and scope of this demo in your usecase."
              rows="4"
              required
            ></textarea>
          </div>
          <button type="submit" className={styles.submitButton}>
            Submit
          </button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you! Your request for a live demo has been submitted
          successfully.
        </p>
      )}
    </div>
  );
};

export default RequestDemoForm;
