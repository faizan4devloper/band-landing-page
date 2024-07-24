import React, { useRef } from "react";
import styles from "./Video.module.css";

const Video = ({ src }) => {
  const videoRef = useRef(null);
  return (
    <div className={styles.videoContainer}>
      <video ref={videoRef} className={styles.video} controls>
        <source src={src} type="video/mp4" />
        Your browser does not support the video tag.
      </video>
         </div>
  );
};

export default Video;
