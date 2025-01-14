# Web-Application-Mimicking-Google-Sheets-
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [grid, setGrid] = useState(
    Array(10)
      .fill(null)
      .map(() => Array(10).fill(""))
  );

  const updateCell = (row, col, value) => {
    const newGrid = [...grid];
    newGrid[row][col] = value;
    setGrid(newGrid);
  };

  return (
    <div className="spreadsheet">
      <div className="toolbar">
        <button>B</button>
        <button>I</button>
        {/* Add other toolbar items */}
      </div>
      <div className="grid">
        {grid.map((row, rowIndex) => (
          <div key={rowIndex} className="row">
            {row.map((cell, colIndex) => (
              <input
                key={${rowIndex}-${colIndex}}
                value={cell}
                onChange={(e) => updateCell(rowIndex, colIndex, e.target.value)}
              />
            ))}
          </div>
        ))}
      </div>
    </div>
  );
};

export default App;
#backend code(node.js)

// Utility functions
exports.sum = (range) => range.reduce((acc, val) => acc + (parseFloat(val) || 0), 0);

exports.average = (range) => (range.length ? exports.sum(range) / range.length : 0);

exports.max = (range) => Math.max(...range.map((val) => parseFloat(val) || -Infinity));

exports.min = (range) => Math.min(...range.map((val) => parseFloat(val) || Infinity));

exports.count = (range) => range.filter((val) => !isNaN(parseFloat(val))).length;

exports.trim = (text) => text.trim();

exports.upper = (text) => text.toUpperCase();

exports.lower = (text) => text.toLowerCase();
#routejs

const express = require("express");
const router = express.Router();
const functions = require("./functions");

router.post("/calculate", (req, res) => {
  const { type, range } = req.body;
  let result;

  switch (type) {
    case "SUM":
      result = functions.sum(range);
      break;
    case "AVERAGE":
      result = functions.average(range);
      break;
    case "MAX":
      result = functions.max(range);
      break;
    case "MIN":
      result = functions.min(range);
      break;
    case "COUNT":
      result = functions.count(range);
      break;
    default:
      result = "Invalid Function";
  }

  res.json({ result });
});

module.exports = router;
