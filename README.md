.processButton {
    display: inline-flex;
    align-items: center;
    padding: 0.75rem 1.5rem;
    font-size: 1.1rem;
    font-weight: 600;
    color: #fff;
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    border: none;
    border-radius: 50px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    position: relative;
    overflow: hidden;
}

.processButton::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: rgba(255, 255, 255, 0.1);
    transform: rotate(45deg) scale(0);
    transition: transform 0.5s ease;
}

.processButton:hover::before {
    transform: rotate(45deg) scale(1);
}







.processButton {
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    color: #fff;
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.startButton {
    display: inline-flex;
    align-items: center;
    padding: 0.75rem 1.5rem;
    font-size: 1.1rem;
    font-weight: 600;
    color: #fff;
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    border: none;
    border-radius: 50px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    position: relative;
    overflow: hidden;
}

.startButton::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: rgba(255, 255, 255, 0.1);
    transform: rotate(45deg) scale(0);
    transition: transform 0.5s ease;
}

.startButton:hover::before {
    transform: rotate(45deg) scale(1);
}
