import React, { useState } from 'react';
import styles from './MainContent.module.css';

const contentData = {
  1: {
    title: 'Education Resources',
    questions: [
      {
        question: 'What are the available courses?',
        answer: 'You can explore various online and offline courses related to different subjects.',
      },
      {
        question: 'How to enroll in a course?',
        answer: 'You can enroll by visiting the course details and clicking on the enroll button.',
      },
      {
        question: 'What is the duration of the courses?',
        answer: 'Course durations vary. Most of them are between 4-12 weeks.',
      },
      {
        question: 'Are there any certifications?',
        answer: 'Yes, certificates are provided upon successful completion of the courses.',
      },
    ],
  },
  2: {
    title: 'Job Opportunities',
    questions: [
      {
        question: 'How to search for job listings?',
        answer: 'You can use the search bar to filter jobs by title, location, and experience level.',
      },
      {
        question: 'How to apply for a job?',
        answer: 'Visit the job details page and click the "Apply" button to submit your application.',
      },
      {
        question: 'What are the requirements for a job?',
        answer: 'Each job listing will have specific qualifications and requirements mentioned.',
      },
      {
        question: 'Is remote work available?',
        answer: 'Many job listings offer remote work options depending on the employer.',
      },
    ],
  },
  3: {
    title: 'Health Services',
    questions: [
      {
        question: 'How to access health records?',
        answer: 'You can access your health records by visiting the Health Records section.',
      },
      {
        question: 'What health services are available?',
        answer: 'Various services like consultations, check-ups, and telemedicine are available.',
      },
      {
        question: 'How to book an appointment?',
        answer: 'You can book an appointment directly from the health service provider’s page.',
      },
      {
        question: 'Is health insurance required?',
        answer: 'Health insurance is recommended but not mandatory for some services.',
      },
    ],
  },
  4: {
    title: 'Technology Innovations',
    questions: [
      {
        question: 'What are the latest tech trends?',
        answer: 'Latest trends include AI, blockchain, cloud computing, and IoT.',
      },
      {
        question: 'How to stay updated?',
        answer: 'You can subscribe to newsletters and attend webinars to stay updated.',
      },
      {
        question: 'Are there any innovation workshops?',
        answer: 'Yes, workshops are held regularly on different technological advancements.',
      },
      {
        question: 'How to participate in tech communities?',
        answer: 'You can join forums and attend meetups to be part of tech communities.',
      },
    ],
  },
  5: {
    title: 'Learning Platforms',
    questions: [
      {
        question: 'Which platforms are available?',
        answer: 'Platforms like Coursera, Udemy, and edX offer a variety of learning resources.',
      },
      {
        question: 'How to choose the right platform?',
        answer: 'It depends on your learning style and the courses available on each platform.',
      },
      {
        question: 'Are the courses free?',
        answer: 'Some courses are free, while others require payment.',
      },
      {
        question: 'Can I get a refund?',
        answer: 'Refund policies depend on the platform. Check their terms for more information.',
      },
    ],
  },
};

const MainContent = ({ activeTopic }) => {
  const [openQuestion, setOpenQuestion] = useState(null);

  const toggleQuestion = (index) => {
    setOpenQuestion(openQuestion === index ? null : index);
  };

  if (!activeTopic) {
    return <div className={styles.mainContent}>Please select a topic from the sidebar.</div>;
  }

  const content = contentData[activeTopic];

  return (
    <div className={styles.mainContent}>
      <h2>{content.title}</h2>
      {content.questions.map((q, index) => (
        <div key={index} className={styles.question}>
          <div onClick={() => toggleQuestion(index)} className={styles.questionTitle}>
            {q.question}
          </div>
          {openQuestion === index && <div className={styles.answer}>{q.answer}</div>}
        </div>
      ))}
    </div>
  );
};

export default MainContent;



/* MainContent.module.css */
.mainContent {
  flex-grow: 1;
  padding: 30px;
  background-color: #ecf0f1;
}

.mainContent h2 {
  font-size: 2em;
  margin-bottom: 20px;
}

.question {
  margin-bottom: 15px;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.question:hover {
  background-color: #f0f0f0;
}

.questionTitle {
  font-size: 1.2em;
  font-weight: bold;
  color: #34495e;
}

.answer {
  margin-top: 10px;
  font-size: 1em;
  color: #7f8c8d;
}

.answer:before {
  content: 'A: ';
  font-weight: bold;
}
