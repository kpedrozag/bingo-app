<script>
  const lettersIndex = ['B', 'I', 'N', 'G', 'O'];
  let boardGenerated = false;
  let boardElements = [];

  function calculateRandomBoard() {
    let board = {};
    for (let i = 0; i < lettersIndex.length; i++) {
      const temporalColumn = [];
      const columnLength = 5;
      while (true) {
        if (temporalColumn.length === columnLength) {
          break;
        }
        let randomValue = Math.floor(Math.random() * 16);
        if (randomValue <= 0) {
          randomValue = 1;
        } else if (randomValue >= 16) {
          randomValue = 15;
        }
        const finalValue = randomValue + i * 15;
        if (temporalColumn.includes(finalValue)) {
          continue;
        } else {
          temporalColumn.push(finalValue);
        }
      }
      board[lettersIndex[i]] = temporalColumn;
    }
    return board;
  }

  function buildBoardToPrint(board) {
    let boardObject = [];
    for (let j = 0; j < 6; j++) {
      const row = [];
      for (let k = 0; k < lettersIndex.length; k++) {
        if (j === 0) {
          row.push({
            content: lettersIndex[k],
            className: 'header'
          });
        } else {
          if (lettersIndex[k] === 'N' && j === 3) {
            row.push({
              content: 'X',
              className: 'empty'
            });
          } else {
            const number = board[lettersIndex[k]][j - 1];
            row.push({
              content: number,
              className: 'number'
            });
          }
        }
      }
      boardObject.push(row);
    }
    return boardObject;
  }

  function handleClickGenerateBoard() {
    const randomBoard = calculateRandomBoard();
    boardElements = buildBoardToPrint(randomBoard);
    boardGenerated = true;
  }

  function handleResetClick() {
    location.reload();
  }

  let lastPosClicked = [];

  function handleClickNumber(posI, posJ) {
    lastPosClicked = [posI, posJ];
    boardElements[posI][posJ].className = 'taken';
  }

  function handleClickUndoLastNumber() {
    const [lastPosI, lastPosJ] = lastPosClicked;
    boardElements[lastPosI][lastPosJ].className = 'number';
    lastPosClicked = [];
  }

  function handleCleanClick() {
    for (let i = 1; i < boardElements.length; i++) {
      for (let j = 0; j < boardElements[i].length; j++) {
        if (boardElements[i][j].className === 'taken') {
          boardElements[i][j].className = 'number';
        }
      }
    }
    lastPosClicked = [];
  }
</script>

<main>
  <h1>Carton de bingo /<br/>Bingo Cards</h1>
  <button on:click={handleClickGenerateBoard} disabled={boardGenerated}>Generar carton/Generate card</button>
  {#if boardGenerated}
    <div class="board-container">
      <table>
        {#each boardElements as row, i}
          <tr>
            {#each row as { content, className }, j}
              <td class={className}>
                {#if className === 'number'}
                  <button
                    on:click={() => {
                      handleClickNumber(i, j);
                    }}>{content}</button
                  >
                {:else}
                  {content}
                {/if}
              </td>
            {/each}
          </tr>
        {/each}
      </table>
    </div>
    <button class="reset-button" on:click={handleResetClick}>Nuevo carton/New board</button>
    <button class="clean-button" on:click={handleCleanClick}>Limpiar carton/Clean board</button>
  {/if}

  {#if lastPosClicked.length > 0}
    <br />
    <button on:click={handleClickUndoLastNumber}>Deshacer ultimo numero/Undo last clicked number</button>
  {/if}
</main>

<style>
  main {
    text-align: center;
    margin: 0 auto;
  }
  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  table {
    border: 1px solid black;
    margin-left: auto;
    margin-right: auto;
  }
  table td {
    padding: 10px;
    font-size: 30px;
  }
  table td.header {
    background-color: cyan;
  }
  table td.empty {
    background-color: red;
  }
  table td.taken {
    background-color: #bfda26;
  }

  .reset-button {
    margin-top: 25px;
  }
  .clean-button {
    margin-left: 15px;
  }
</style>
