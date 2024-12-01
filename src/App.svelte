<script>
  const boardSize = 9;
  const tilesPerPlayer = 7;

  const createBoard = () => {
    const board = Array.from({ length: boardSize }, () =>
      Array.from({ length: boardSize }, () => null)
    );

    // Artifact and Home positions
    board[boardSize - 1][0] = { type: "artifact", player: 1 }; // Bottom-left for Player 1
    board[0][boardSize - 1] = { type: "artifact", player: 2 }; // Top-right for Player 2

    board[boardSize - 1][2] = { type: "home", player: 1 }; // Bottom-left (near corner)
    board[boardSize - 3][0] = { type: "home", player: 1 }; // Bottom-left (near edge)
    board[2][boardSize - 1] = { type: "home", player: 2 }; // Top-right (near edge)
    board[0][boardSize - 3] = { type: "home", player: 2 }; // Top-right (near corner)

    return board;
  };

  const board1 = createBoard();
  const board2 = createBoard();

  const player1Tiles = Array.from({ length: tilesPerPlayer }, () =>
    String.fromCharCode(65 + Math.floor(Math.random() * 26))
  );
  const player2Tiles = Array.from({ length: tilesPerPlayer }, () =>
    String.fromCharCode(65 + Math.floor(Math.random() * 26))
  );
</script>

<style>
  .game-container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
    padding: 20px;
  }

  .board {
    display: grid;
    grid-template-columns: repeat(9, 50px);
    grid-template-rows: repeat(9, 50px);
    gap: 2px;
    border: 2px solid #ccc;
    position: relative;
  }

  .cell {
    width: 50px;
    height: 50px;
    background-color: #f0f0f0;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid #ddd;
    position: relative;
    font-size: 14px;
  }

/* Artifact overlays */
.artifact-player1 {
    background-image: linear-gradient(rgba(50, 205, 50, 0.1), rgba(50, 205, 50, 0.4)), url("/images/artifact.jpg");
    background-size: cover;
    background-position: center;
}

.artifact-player2 {
    background-image: linear-gradient(rgba(30, 144, 255, 0.1), rgba(30, 144, 255, 0.4)), url("/images/artifact.jpg");
    background-size: cover;
    background-position: center;
}

/* Home overlays */
.home-player1 {
    background-image: linear-gradient(rgba(50, 205, 50, 0.1), rgba(50, 205, 50, 0.4)), url("/images/home.jpg");
    background-size: cover;
    background-position: center;
}

.home-player2 {
    background-image: linear-gradient(rgba(30, 144, 255, 0.1), rgba(30, 144, 255, 0.4)), url("/images/home.jpg");
    background-size: cover;
    background-position: center;
}



  .tiles {
    display: flex;
    justify-content: center;
    gap: 5px;
    margin-top: 10px;
  }

  .tile {
    width: 40px;
    height: 40px;
    background-color: #d4d4d4;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    border-radius: 5px;
    border: 1px solid #bbb;
    box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
  }

  .board-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>

<div class="game-container">
  <!-- Player 1's Board -->
  <div class="board-container">
    <div class="board">
      {#each board1 as row, i}
        {#each row as cell, j}
          <div
            class="cell {cell?.type === 'artifact' && cell?.player === 1 ? 'artifact-player1' : ''} 
                        {cell?.type === 'artifact' && cell?.player === 2 ? 'artifact-player2' : ''}
                        {cell?.type === 'home' && cell?.player === 1 ? 'home-player1' : ''} 
                        {cell?.type === 'home' && cell?.player === 2 ? 'home-player2' : ''}"
          ></div>
        {/each}
      {/each}
    </div>
    <div class="tiles">
      {#each player1Tiles as tile}
        <div class="tile">{tile}</div>
      {/each}
    </div>
  </div>

  <!-- Player 2's Board -->
  <div class="board-container">
    <div class="board">
      {#each board2.reverse() as row, i}
        {#each row.reverse() as cell, j}
          <div
            class="cell {cell?.type === 'artifact' && cell?.player === 1 ? 'artifact-player1' : ''} 
                        {cell?.type === 'artifact' && cell?.player === 2 ? 'artifact-player2' : ''}
                        {cell?.type === 'home' && cell?.player === 1 ? 'home-player1' : ''} 
                        {cell?.type === 'home' && cell?.player === 2 ? 'home-player2' : ''}"
          ></div>
        {/each}
      {/each}
    </div>
    <div class="tiles">
      {#each player2Tiles as tile}
        <div class="tile">{tile}</div>
      {/each}
    </div>
  </div>
</div>
