.chat-container {
    background-color: rgba(0, 0, 0, 0.5);
    margin: 80px 0 0 10px;
    border-radius: 10px;
    width: 450px;
    display: flex;
    flex-direction: column;
    height: 500px;
    position: relative;
    overflow: hidden;
}
.chat-container h1 {
    text-align: center;
    margin-top: 14px;
    font-size: 1.5rem;
  line-height: 1.2;
  font-weight: 600;
  background: -webkit-gradient(
    linear,
    left top,
    right top,
    color-stop(-19.51%, #7abef7),
    color-stop(36.51%, #4080f5),
    to(#572ac2)
  );
  background: -webkit-linear-gradient(
    left,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  background: -o-linear-gradient(
    left,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  background: linear-gradient(
    90deg,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  text-transform: inherit !important;
}
.hamburger-menu {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 24px;
    color: #fff;
    cursor: pointer;
    z-index: 10;
}

.most-asked-questions {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
    display: flex;
    border-radius: 10px;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 5;
    color: white;
    transform: translateY(-100%);
    transition: transform 0.3s ease-in-out;
}

.most-asked-questions.show {
    transform: translateY(0);
}

.most-asked-questions h4 {
    margin-bottom: 20px;
    opacity: 0;
    animation: fadeIn 0.5s 0.2s forwards;
}

.most-asked-questions ul {
    list-style: none;
    padding: 0;
    opacity: 0;
    animation: fadeIn 0.5s 0.4s forwards;
}

.most-asked-questions li {
    padding: 10px 20px;
    font-size: 14px;
    font-weight: 500;
    margin: 5px 0;
    background-color: #fff;
    color: #000;
    border-radius: 4px;
    cursor: pointer;
    box-shadow: 0 10px 8px rgba(0, 0, 0, 0.2);
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.most-asked-questions li:hover {
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
    transform: scale(1.05);
}

.selected-questions {
    margin-top: 30px;
    overflow-y: auto;
    padding: 10px;
    flex-grow: 1;
}

.selected-questions::-webkit-scrollbar {
    width: 8px;
}

.selected-questions::-webkit-scrollbar-thumb {
    background-color: rgba(111, 54, 205, 0.8);
    border-radius: 10px;
}

.selected-questions::-webkit-scrollbar-thumb:hover {
    background-color: rgba(111, 54, 205, 1);
}

.selected-questions::-webkit-scrollbar-track {
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
}

.selected-question {
    /*background-color: #fff;*/
    padding: 10px;
    border-radius: 4px;
    margin-bottom: 10px;
    box-shadow: 0 10px 8px rgba(0, 0, 0, 0.2);
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.selected-question h4 {
    margin: 0;
}

.selected-question .question-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.selected-question .toggle-icon {
    font-size: 18px;
}

.selected-question p {
    margin: 10px 0 0;
    display: none;
    animation: fadeIn 0.3s ease-in-out forwards;
}

.selected-question.expanded p {
    font-size: 14px;
    display: block;
    transition: 0.1s ease-in;
}
.selected-question.expanded p:hover{
    color: #000;
}
.description {
    /*background-color: #f5f5f5;*/
    display: none;
    padding: 8px;
    border-radius: 4px;
    margin: 8px 0;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.description:hover {
    /*background-color: #ddd;*/
}

.description{
    display: none;
}

.selected-question.expanded .description{
    display: block;
}

.description.expanded ul {
    display: block;
}

.description ul {
    list-style: none;
    padding: 0;
    margin: 10px 0 0;
    display: none;
    animation: fadeIn 0.3s ease-in-out forwards;
}

.description ul li {
    background-color: #e0e0e0;
    text-align: center;
    padding: 5px;
    color: #000;
    border-radius: 4px;
    margin: 5px 0;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: .5s ease;
}

.description ul li:hover{
    background: #e0e0ef;
}

.history {
    margin-top: 30px;
    padding: 20px;
    background: #f7f7f7;
    border: 1px solid #ddd;
    border-radius: 5px;
}

.history h2 {
    margin-bottom: 15px;
    font-size: 24px;
}

.history ul {
    list-style: none;
    padding: 0;
}

.history li {
    margin-bottom: 15px;
}

.history li h4 {
    margin: 0;
    font-size: 20px;
}

.history li ul {
    list-style: disc inside;
    margin-top: 10px;
    padding-left: 20px;
}

.history li ul li {
    margin-bottom: 5px;
}


@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}
