react-dom.development.js:18704 The above error occurred in the <div> component:

    at div
    at MainContainer (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:463:3)
    at div
    at App (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:42:74)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
react-dom.development.js:28478 Uncaught Error: Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: undefined. You likely forgot to export your component from the file it's defined in, or you might have mixed up default and named imports.

Check the render method of `MainContainer`.
    at createFiberFromTypeAndProps (react-dom.development.js:28478:1)
    at createFiberFromElement (react-dom.development.js:28504:1)
    at createChild (react-dom.development.js:13345:1)
    at reconcileChildrenArray (react-dom.development.js:13640:1)
    at reconcileChildFibers (react-dom.development.js:14057:1)
    at reconcileChildren (react-dom.development.js:19186:1)
    at updateHostComponent (react-dom.development.js:19953:1)
    at beginWork (react-dom.development.js:21657:1)
    at beginWork$1 (react-dom.development.js:27465:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
﻿


import React, { useState } from "react";
import { ChatScreen } from "./ChatScreen";
import { Topics } from "./Topics";
import { Insights } from "./Insights";
import Modal from "./Modal";

const getLocalityFromPostcode = async (postcode) => {
  // Mock response - replace this with an actual API call
  const postcodeToLocalityMap = {
    "SW1A 1AA": "Westminster",
    "E1 6AN": "Shoreditch",
    "M1 1AE": "Manchester",
    "E14 0HE": "London Borough of Tower Hamlets",
  };

  return postcodeToLocalityMap[postcode] || "Unknown locality";
};

function MainContainer({ advisorType }) {
      const [selectedSchool, setSelectedSchool] = useState("");

  const [selectedQuestion, setSelectedQuestion] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(true);
  const [postcode, setPostcode] = useState("");
  const [locality, setLocality] = useState("");


const handleSchoolClick = (school) => {
        setSelectedSchool(school);
    };



  const handleQuestionClick = (question) => {
    setSelectedQuestion(question);
  };

  const handleSendQuestionInfo = (questionInfo) => {
    console.log("Question info sent to Chat:", questionInfo);
  };

  const handleCloseModal = () => {
    setIsModalOpen(false);
  };

  const handlePostcodeApply = async (postcode) => {
    const localityName = await getLocalityFromPostcode(postcode);
    setPostcode(postcode);
    setLocality(localityName);
    setIsModalOpen(false);
  };

  return (
    <div>
      <h2>
        Selected Council {locality && `- ${locality}`}
      </h2>
      <ChatScreen />
      <Topics onQuestionClick={handleQuestionClick} onSchoolClick={handleSchoolClick}/>
      <Insights selectedQuestion={selectedQuestion} onSendQuestionInfo={handleSendQuestionInfo} selectedSchool={selectedSchool}/>
      <Modal isOpen={isModalOpen} onClose={handleCloseModal} onPostcodeApply={handlePostcodeApply}>
        {/* You can pass additional children here if needed */}
      </Modal>
    </div>
  );
}

export default MainContainer;
