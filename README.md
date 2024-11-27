MainContent.js:44 TypeError: Converting circular structure to JSON
    --> starting at object with constructor 'HTMLButtonElement'
    |     property '__reactFiber$5ag6t2lldr5' -> object with constructor 'FiberNode'
    --- property 'stateNode' closes the circle
    at stringify (<anonymous>)
    at async handleVerify (MainContent.js:31:1)
