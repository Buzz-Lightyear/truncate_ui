<script>
  const BOARD_SIZE = 9;

  // Predefined positions for artifacts and homes
  const artifacts = [
    { x: 0, y: 0 },
    { x: 8, y: 8 },
  ];
  const homes = [
    { x: 0, y: 2 },
    { x: 2, y: 0 },
    { x: 6, y: 8 },
    { x: 8, y: 6 },
  ];

  // Generate a board with artifacts and homes
  const generateBoard = () => {
    const board = Array.from({ length: BOARD_SIZE }, () =>
      Array.from({ length: BOARD_SIZE }, () => ({ type: "empty" }))
    );

    artifacts.forEach(({ x, y }) => {
      board[x][y].type = "artifact";
    });

    homes.forEach(({ x, y }) => {
      board[x][y].type = "home";
    });

    return board;
  };

  const board = generateBoard();

  // Player tiles
  const player1Tiles = ["A", "B", "C", "D", "E", "F", "G"];
  const player2Tiles = ["H", "I", "J", "K", "L", "M", "N"];
</script>

<style>
  .board-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 20px;
  }

  .board {
    display: grid;
    grid-template-columns: repeat(9, 50px);
    grid-template-rows: repeat(9, 50px);
    gap: 2px;
    margin: 20px;
  }

  .cell {
    width: 50px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    background-color: #f0f0f0;
    border: 1px solid #ccc;
  }

  .cell.artifact {
    background-color: #d4af37;
    color: white;
  }

  .cell.home {
    background-color: #6495ed;
    color: white;
  }

  .player-tiles {
    display: flex;
    gap: 10px;
    margin: 20px;
  }

  .tile {
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    background-color: #ffe4c4;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
</style>

<div class="board-container">
  <!-- Player 1 Tiles -->
  <div class="player-tiles">
    {#each player1Tiles as tile}
      <div class="tile">{tile}</div>
    {/each}
  </div>

  <!-- Main Board -->
  <div class="board">
    {#each board as row, x}
      {#each row as cell, y}
        <div class="cell {cell.type}">
          {cell.type === "artifact" ? "A" : cell.type === "home" ? "H" : ""}
        </div>
      {/each}
    {/each}
  </div>

  <!-- Rotated Board for Player 2 -->
  <div class="board" style="transform: rotate(180deg);">
    {#each board as row, x}
      {#each row as cell, y}
        <div class="cell {cell.type}">
          {cell.type === "artifact" ? "A" : cell.type === "home" ? "H" : ""}
        </div>
      {/each}
    {/each}
  </div>

  <!-- Player 2 Tiles -->
  <div class="player-tiles">
    {#each player2Tiles as tile}
      <div class="tile">{tile}</div>
    {/each}
  </div>
</div>
