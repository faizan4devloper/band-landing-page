import React, { createContext, useState } from "react";

export const DataContext = createContext();

export const DataProvider = ({ children }) => {
  const [rows, setRows] = useState([]);
  const [data, setData] = useState(null); // Shared extracted data

  return (
    <DataContext.Provider value={{ rows, setRows, data, setData }}>
      {children}
    </DataContext.Provider>
  );
};
