import React from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';

const JobPage = () => {
  return (
    <div>
      <Sidebar/>
      <MainContent/>
    </div>
  );
};

export default JobPage;

import React from 'react';

const Sidebar =()=>{
    
    return <div>
        Sidebar
    </div>
    
}

export default Sidebar;


import React from 'react';

const MainContent =()=>{
    return <div>
        MainContent
    </div>
}

export default MainContent;
