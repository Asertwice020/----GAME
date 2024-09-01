# app.js code 
"use strict";

function initializeGame() {
  const gameContainer = document.querySelector(".game_container");
  const boxes = [...gameContainer.children];

  let gameOver = false;
  let currentPlayer = "X";
  let isBoardFilled = false;
  let usedPosition = 1;
  let initialPositionBoxToRemove = 1;
  let clickedPositions = [];

  function handleIsBoardFilled() {
    const filledBoxes = boxes.filter((box) => box.innerText !== "");
    if (filledBoxes.length == 9) {
      return (isBoardFilled = true);
    }
  }

  function emptyTheFirstMove() {
    const boxToRemoveData = clickedPositions.find(
      (position) => position.clickedPosition === initialPositionBoxToRemove
    );

    if (boxToRemoveData) {
      const boxToClear = document.getElementById(boxToRemoveData.clicked);

      if (boxToClear) {
        boxToClear.innerText = ""; // Clear the innerText of the box

        // Remove the entry from the clickedPositions array
        clickedPositions = clickedPositions.filter(
          (position) => position.clickedPosition !== initialPositionBoxToRemove
        );

        initialPositionBoxToRemove++; // Update to remove the next position
      }
    }
  }

  function playMove(event) {
    if (gameOver || event.target.innerText !== "") {
      return;
    } else if (event.target.innerText === "") {
      event.target.innerText = currentPlayer;

      const clickedBoxData = {
        clicked: event.target.id,
        clickedPosition: usedPosition,
      };
      clickedPositions.push(clickedBoxData);
      currentPlayer = currentPlayer === "X" ? "O" : "X";
      usedPosition++; // Increment the usedPosition for the next click
      let tempIsBoardFilledFlag = handleIsBoardFilled();

      if (tempIsBoardFilledFlag) {
        emptyTheFirstMove();
      }
      // console.log(isBoardFilled, "the flag");
      console.log(clickedPositions);
    }
  }

  gameContainer.addEventListener("click", playMove);
}

initializeGame();
