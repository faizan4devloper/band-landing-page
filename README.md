.landing-content h1 {
  font-size: 3.5rem; /* Adjust font size for a sleek look */
  font-weight: 800;
  margin-bottom: 1rem;
  background: linear-gradient(
    45deg, 
    #ffffff, 
    #d4d4d4
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 3px 10px rgba(0, 0, 0, 0.3);
  font-family: 'Poppins', sans-serif;
  letter-spacing: -1.5px;
  position: relative;
  overflow: hidden;
}

.landing-content h1::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    120deg, 
    transparent, 
    rgba(255, 255, 255, 0.4), 
    transparent
  );
  transform: skewX(-45deg);
  animation: shine 2.5s infinite linear;
}

.landing-content p {
  font-size: 1.2rem; /* Make it slightly smaller */
  font-weight: 400;
  margin-bottom: 2rem;
  color: rgba(255, 255, 255, 0.85);
  line-height: 1.8;
  font-family: 'Roboto', sans-serif;
  letter-spacing: 0.4px;
  position: relative;
}

.landing-content p:first-of-type {
  font-size: 1.6rem; /* Emphasize the tagline */
  font-weight: 500;
  color: rgba(255, 255, 255, 0.9);
}

.get-started-btn {
  position: relative;
  padding: 15px 50px;
  font-size: 1rem; /* Make the text size consistent */
  font-weight: 700;
  color: #ffffff;
  background: linear-gradient(
    304deg,
    #499dfd 3.03%,
    #2678f5 27.62%,
    #6628c5 76.39%,
    #5b0bb1 112.44%
  );
  border: none;
  border-radius: 50px;
  cursor: pointer;
  overflow: hidden;
  transition: all 0.4s ease;
  text-transform: capitalize;
  letter-spacing: 0.5px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.2);
}

.get-started-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 16px 40px rgba(0, 0, 0, 0.3);
}








import React from 'react';
import { motion } from 'framer-motion';
import './LandingPage.css';

// Animated Background Component
const AnimatedBackground = () => {
  const bubbles = Array(10).fill(0).map((_, index) => (
    <motion.div
      key={index}
      className="bubble"
      style={{
        width: `${Math.random() * 100 + 50}px`,
        height: `${Math.random() * 100 + 50}px`,
        left: `${Math.random() * 100}%`,
        top: `${Math.random() * 100}%`,
        opacity: Math.random() * 0.5 + 0.2
      }}
      animate={{
        x: [0, Math.random() * 100 - 50, 0],
        y: [0, Math.random() * 100 - 50, 0],
        rotate: 360
      }}
      transition={{
        duration: Math.random() * 5 + 5,
        repeat: Infinity,
        repeatType: "reverse",
        ease: "easeInOut"
      }}
    />
  ));

  return <div className="background-bubbles">{bubbles}</div>;
};

const LandingPage = ({ onGetStarted }) => {
  const pageVariants = {
    initial: { opacity: 0, scale: 0.9 },
    in: { opacity: 1, scale: 1 },
    out: { opacity: 0, scale: 1.1 }
  };

  const pageTransition = {
    type: "tween",
    ease: "anticipate",
    duration: 0.5
  };

  return (
    <motion.div 
      className="landing-page"
      initial="initial"
      animate="in"
      exit="out"
      variants={pageVariants}
      transition={pageTransition}
    >
      <div className="landing-content">
        <motion.h1
          initial={{ y: -50, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ delay: 0.2 }}
          style={{ fontFamily: 'Poppins, sans-serif' }}
        >
          Reimagine Claim Processing
        </motion.h1>
        
        <motion.p
          initial={{ y: 50, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ delay: 0.4 }}
          style={{ fontFamily: 'Roboto, sans-serif' }}
        >
          Elevate customer experience using Agentic AI
        </motion.p>
        
        <motion.p
          initial={{ y: 50, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ delay: 0.6 }}
          style={{ 
            fontFamily: 'Roboto, sans-serif', 
            fontSize: '0.9rem', 
            marginBottom: '20px' 
          }}
        >
          Agentic AI allows you to transform claim processing experience for your customer. Experience the future.
        </motion.p>
        
        <motion.button
          className="get-started-btn"
          whileHover={{ scale: 1.05 }}
          whileTap={{ scale: 0.95 }}
          onClick={onGetStarted}
        >
          Get Started
        </motion.button>
      </div>

      {/* Animated Background Elements */}
      <AnimatedBackground />
    </motion.div>
  );
};

export default LandingPage;




.landing-page {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  position: relative;
  background-image: 
    linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.2)), 
    url('./bg-img.jpg');
  background-size: cover;
  background-position: center;
  overflow: hidden;
}

.landing-content {
  position: relative;
  z-index: 10;
  text-align: center;
  max-width: 800px;
  padding: 2rem;
  perspective: 1000px;
}

.landing-content h1 {
  font-size: 4rem;
  font-weight: 900;
  margin-bottom: 1rem;
  background: linear-gradient(
    45deg, 
    #ffffff, 
    #a0a0a0
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 4px 15px rgba(0,0,0,0.3);
  font-family: 'Poppins', sans-serif;
  transform: translateZ(50px);
  letter-spacing: -2px;
  position: relative;
  overflow: hidden;
}

.landing-content h1::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    120deg, 
    transparent, 
    rgba(255,255,255,0.2), 
    transparent
  );
  transform: skewX(-45deg);
  animation: shine 3s infinite linear;
}

@keyframes shine {
  0% {
    left: -100%;
  }
  100% {
    left: 100%;
  }
}

.landing-content p {
  font-size: 1.5rem;
  font-weight: 300;
  margin-bottom: 2.5rem;
  color: rgba(255,255,255,0.7);
  line-height: 1.6;
  font-family: 'Inter', sans-serif;
  transform: translateZ(30px);
  letter-spacing: 0.5px;
  position: relative;
  opacity: 0.9;
}

.get-started-btn {
  position: relative;
  padding: 15px 40px;
  font-size: 1.1rem;
  font-weight: 700;
  color: #ffffff;
  background: linear-gradient(
    135deg, 
    var(--primary-color), 
    var(--secondary-color)
  );
  border: none;
  border-radius: 10px;
  cursor: pointer;
  overflow: hidden;
  transition: all 0.4s ease;
  transform: translateZ(20px);
  box-shadow: 0 15px 30px rgba(0,0,0,0.2);
  text-transform: uppercase;
  letter-spacing: 1px;
}

.get-started-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    120deg, 
    transparent, 
    rgba(255,255,255,0.3), 
    transparent
  );
  transition: all 0.5s ease;
}

.get-started-btn:hover {
  transform: translateZ(20px) scale(1.05) translateY(-5px);
  box-shadow: 0 20px 40px rgba(0,0,0,0.3);
}

.get-started-btn:hover::before {
  left: 100%;
}

/* 3D Depth Effect */
.landing-content {
  transition: transform 0.3s ease;
}

.landing-page:hover .landing-content {
  transform: 
    perspective(1000px) 
    rotateX(5deg) 
    rotateY(-5deg) 
    translateZ(50px);
}
.get-started-btn {
  position: relative;
  padding: 15px 40px;
  font-size: 12px;
  font-weight: 600;
  color: #ffffff;
  background: transparent;
  border: 1.5px solid rgba(255,255,255,0.7);
  border-radius: 50px; /* More rounded */
  cursor: pointer;
  overflow: hidden;
  transition: all 0.4s ease;
  text-transform:capitalize;
  letter-spacing: 1px;
  backdrop-filter: blur(10px); /* Glassmorphism effect */
  box-shadow: 0 8px 15px rgba(0,0,0,0.1);
  text-decoration: none;
  display: inline-block;
  margin-top: 20px; /* Adjust spacing */
}

.get-started-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%)
;
  transition: all 0.5s ease;
  z-index: -1;
}

.get-started-btn:hover {
  background: linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%)
;
  border-color: rgba(255,255,255,1);
  transform: translateY(-5px);
  box-shadow: 0 12px 20px rgba(0,0,0,0.2);
}

.get-started-btn:hover::before {
  left: 100%;
}
