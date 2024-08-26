import React, { useEffect, useRef, useState } from 'react';
import styles from './Footer.module.css';

const Footer = () => {
  const [activeCard, setActiveCard] = useState(0); // State to track the active card
  const blogCardsRef = useRef([]); // Ref to access all blog cards

  const blogs = [
    {
      title: 'Generative AI-powered email EAR (extract, act and respond) on AWS',
      link: 'https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws',
    },
    {
      title: 'LLM cache: Sustainable, fast, cost-effective GenAI app design',
      link: 'https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design',
    },
    {
      title: 'Unlocking the future of recruitment with SmartRecruit',
      link: 'https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit',
    },
  ];

  useEffect(() => {
    const handleScroll = () => {
      const scrollPosition = window.scrollY + window.innerHeight;
      blogCardsRef.current.forEach((card, index) => {
        if (card.offsetTop < scrollPosition - 100) { // Adjust to change when cards show up
          setActiveCard(index + 1);
        }
      });
    };

    window.addEventListener('scroll', handleScroll);
    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  }, []);

  const navigateToBlog = (url) => {
    window.location.href = url;
  };

  return (
    <footer className={styles.footer}>
      <div className={styles.blogSection}>
        <h3 className={styles.blogTitle}>Latest Blogs</h3>
        <div className={styles.blogCards}>
          {blogs.map((blog, index) => (
            <div
              key={index}
              className={`${styles.blogCard} ${index < activeCard ? styles.show : ''}`}
              ref={(el) => (blogCardsRef.current[index] = el)}
              onClick={() => navigateToBlog(blog.link)}
            >
              <p className={styles.blogText}>{blog.title}</p>
              <p className={styles.learnMore}>Learn more →</p>
            </div>
          ))}
        </div>
      </div>
      <div className={styles.footerInfo}>
        <p>Copyright © 2024 HCL Technologies Limited</p>
        <p>Contact: info@yourcompany.com</p>
      </div>
    </footer>
  );
};

export default Footer;


.footer {
  background: linear-gradient(to bottom, #14142b 0%, #05041e 100%) !important;
  padding: 20px;
  text-align: center;
  color: #fcfcfc;
  border-top: 1px solid #ddd;
  margin: 30px -88px;
  margin-bottom: 0px;
  height: 100%;
  position: relative;
  overflow: hidden;
}

.blogSection {
  margin-bottom: 15px;
}

.blogTitle {
  font-size: 24px;
  color: #5931d5;
  margin-bottom: 10px;
  transition: color 0.3s ease;
}

.blogCards {
  position: relative;
  width: 80%;
  margin: 0 auto;
  height: 200px; /* Adjust height if necessary */
}

.blogCard {
  background-color: #1e1e38;
  color: #fcfcfc;
  padding: 20px;
  margin: 10px 0;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.5s ease, opacity 0.5s ease;
  position: absolute;
  width: 100%;
  opacity: 0;
  transform: translateY(50px);
  transition: transform 0.5s ease-out, opacity 0.5s ease-out;
}

.blogCard:nth-child(1) {
  z-index: 3;
}

.blogCard:nth-child(2) {
  z-index: 2;
}

.blogCard:nth-child(3) {
  z-index: 1;
}

.blogCard.show {
  opacity: 1;
  transform: translateY(0);
}

.blogText {
  margin-bottom: 8px;
  font-size: 18px;
}

.learnMore {
  font-size: 14px;
  color: #888;
  transition: color 0.3s ease;
}

.learnMore:hover {
  color: #5931d5;
}

.footerInfo {
  margin-top: 20px;
  font-size: 14px;
  color: #888;
}

.footerInfo p {
  margin: 5px 0;
  transition: color 0.3s ease;
}

.footerInfo p:hover {
  color: #5931d5;
}
