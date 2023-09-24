<script>
  import { onMount } from "svelte";

  let gameBoard = Array(100);
  let possiblePlace = Array()
  let currentValue = 0;
  let gameIndex = 0;

  onMount(async () => {
    initializeGame();
    
    console.log(gameBoard)
  });

  function initializeGame() {
    currentValue += 1;
  }

  function newGame() {
    gameBoard = Array(100);
    possiblePlace.length = 0
    currentValue = 1;
  }

  /**
   * @param {number} index
   * @param {string} value
   */
  function updateNumber(index, value) {
    gameIndex = index;
    currentValue += 1;
    gameBoard[index] = parseInt(value);
  }

  /**
   * @param {number} index
   * @param {string} value
   */
  function updateGame(index, value){
    // Check if cell holds a value
    // disable button if not colored
    updateNumber(index, value)
    gameRules()
  }

  function getFirstDigit(number) {
    let numStr = number.toString();
    if(numStr.length === 1){
      numStr = "0" + numStr;
    }
    // Check if the string is not empty
    if (numStr.length > 0) {
      return parseInt(numStr[0]);
    } else {
      return null;
    }
  }
  function gameRules() {
    console.log('Spelregels')
    possiblePlace.length = 0;
    
    let top = gameIndex - 30
    let left = gameIndex - 3
    let right = gameIndex + 3
    let bottom = gameIndex + 30

    let topLeft = gameIndex - 18
    let topRight = gameIndex - 22
    let bottomLeft = gameIndex + 18
    let bottomRight = gameIndex + 22

    // Diagonal
    if (topLeft >= 0 && getFirstDigit(top + 10) === getFirstDigit(topLeft))
      possiblePlace.push(topLeft);

    if (topRight >= 0 && getFirstDigit(top + 10) === getFirstDigit(topRight))
      possiblePlace.push(topRight);

    if (bottomLeft <= 99 && getFirstDigit(bottom - 10) === getFirstDigit(bottomLeft))
      possiblePlace.push(bottomLeft);

    if (bottomRight <= 99 && getFirstDigit(bottom - 10) === getFirstDigit(bottomRight))
      possiblePlace.push(bottomRight);

    // Horizontal & vertical
    if (top >= 0)
      possiblePlace.push(top)

    if (left >= 0)
      if(getFirstDigit(left) === getFirstDigit(gameIndex))
      possiblePlace.push(left)

    if (right >= 0)
      if(getFirstDigit(right) === getFirstDigit(gameIndex))
      possiblePlace.push(right)

    if (bottom >= 0)
      possiblePlace.push(bottom)
  }

  function isNumber(value) {
    return typeof value === "number";
  }
</script>

<!-- Disable cell if it holds a value gameBoard[index]-->
<!-- Disable cell if not in possiblePlace -->

<!-- niet 22 3 30 possiblePlace -->
<div class="grid">
  {#each gameBoard as number, index}
    <div class="cell">
      <button
        disabled={gameBoard[index] || (!possiblePlace.includes(index) && possiblePlace.length !== 0)}
        class="gridButton {possiblePlace.includes(index) ? 'buttonPossibility' : ''}"
        contenteditable="true"
        bind:textContent={gameBoard[index]}
        on:click={(event) => updateGame(index, currentValue)}>
        {#if isNumber(gameBoard[index])}{gameBoard[index]}{/if}
      </button>
    </div>
  {/each}
  <button on:click={newGame}>New game</button>
  <h1>{currentValue - 1}</h1>
</div>

<style>
  @import "../style.css";
</style>
