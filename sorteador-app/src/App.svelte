<script>
  const lettersIndex = ['B', 'I', 'N', 'G', 'O'];
  const letterNumbersRelation = [];
  for (let i = 0; i < 5; i++) {
    const tempRow = [];
    for (let j = i * 15 + 1; j <= (i + 1) * 15; j++) {
      tempRow.push({
        number: j,
        taken: false,
        stringNumber: j < 10 ? `0${j}` : `${j}`
      });
    }
    letterNumbersRelation.push(tempRow);
  }
  let letter = lettersIndex[0],
    firstDigit = 0,
    secondDigit = 0;

  let numbersSorted = 0;
  let disableButton = false;

  let valuesSorted = [];

  function handleRunButtonClick() {
    while (true) {
      let letterIndex = Math.floor(Math.random() * 5);
      if (letterIndex > 4) {
        letterIndex = 4;
      }
      letter = lettersIndex[letterIndex];

      const numbersListToShuffle = letterNumbersRelation[letterIndex];

      const maxNumber = numbersListToShuffle[14].number + 1,
        minNumber = numbersListToShuffle[0].number;

      const randomNumber = Math.floor(Math.random() * (maxNumber - minNumber)) + minNumber;

      const numberIndex = randomNumber % 15;

      const randomDumberData = numbersListToShuffle[numberIndex];

      const { stringNumber, taken } = randomDumberData;
      if (taken) {
        continue;
      }
      firstDigit = stringNumber[0];
      secondDigit = stringNumber[1];

      valuesSorted = [...valuesSorted, `${letter} ${stringNumber}`];

      letterNumbersRelation[letterIndex][numberIndex].taken = true;
      numbersSorted += 1;
      if (numbersSorted === 75) {
        disableButton = true;
        break;
      }
      break;
    }
  }

  function handleResetClick() {
    location.reload();
  }
</script>

<main>
  <h1>Bingo</h1>
  <div class="current-values-container">
    <span>{letter}</span>
    <span>{firstDigit}</span>
    <span>{secondDigit}</span>
  </div>
  <button id="run" on:click={handleRunButtonClick} disabled={disableButton}>
    <p>Sortear numeros / Get a random number</p>
  </button>
  <table>
    {#each letterNumbersRelation as row, index}
      <tr>
        <td>
          <b>{lettersIndex[index]}</b>
        </td>
        {#each row as { number, taken }}
          <td class={taken ? 'taken' : ''}>
            {number}
          </td>
        {/each}
      </tr>
    {/each}
  </table>
  {#if disableButton}
    <button on:click={handleResetClick}> Reset </button>
  {/if}
  <div class="history">
    <br />
    <h2>Ultimos 10 numeros / Last 10 numbers</h2>
    <table>
      {#each valuesSorted.slice(-10).reverse() as val, index}
        <tr>
          <td>
            {val}
          </td>
        </tr>
      {/each}
    </table>
  </div>
</main>

<style>
  main {
    text-align: center;
  }
  div.current-values-container {
    display: flex;
    justify-content: center;
  }
  div.current-values-container > span {
    font-size: 200px;
    border: 1px solid black;
    border-radius: 10px;
    padding: 0px 30px 0px 30px;
    background-color: #59dab5;
  }
  div.current-values-container > span:nth-child(n + 2) {
    margin-left: 50px;
  }
  button {
    width: 100%;
    max-width: 600px;
    margin-top: 50px;
    margin-bottom: 50px;
    font-size: 20px;
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
  table td.taken {
    background-color: #bfda26;
  }

  @media (max-width: 673px) {
    div.current-values-container > span {
      font-size: 150px;
    }
    div.current-values-container > span:nth-child(n + 2) {
      margin-left: 5px;
    }
  }

  @media (max-width: 400px) {
    div.current-values-container > span {
      font-size: 100px;
    }
    div.current-values-container > span:nth-child(n + 2) {
      margin-left: 1px;
    }
  }
</style>
