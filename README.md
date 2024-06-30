import "./GraphicalInsight.css"

function GraphicalInsight({graph}){
    return <div className="graphical-container">
       <h3>Graphical Insights</h3>
      <p>{graph}</p> 
    </div>
}

export default GraphicalInsight;
