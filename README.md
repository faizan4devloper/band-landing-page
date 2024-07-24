import React, { useRef } from "react";
import LazyLoad from 'react-lazyload';
import styles from "./Video.module.css";

const Video = ({ src, poster }) => {
  const videoRef = useRef(null);

  return (
    <div className={styles.videoContainer}>
      <LazyLoad height={360} offset={100} placeholder={<div className={styles.placeholder}>Loading...</div>}>
        <video ref={videoRef} className={styles.video} controls poster={poster}>
          <source src={src} type="video/mp4" />
          Your browser does not support the video tag.
        </video>
      </LazyLoad>
    </div>
  );
};

export default Video;
