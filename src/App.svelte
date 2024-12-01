<script>
  const boardSize = 9;
  const tilesPerPlayer = 7;

  const createBoard = () => {
    const board = Array.from({ length: boardSize }, () =>
      Array.from({ length: boardSize }, () => null)
    );

    board[boardSize - 1][0] = { type: "artifact", player: 1 }; // Bottom-left for Player 1
    board[0][boardSize - 1] = { type: "artifact", player: 2 }; // Top-right for Player 2
    board[boardSize - 1][2] = { type: "home", player: 1 };
    board[boardSize - 3][0] = { type: "home", player: 1 };
    board[2][boardSize - 1] = { type: "home", player: 2 };
    board[0][boardSize - 3] = { type: "home", player: 2 };

    return board;
  };

  let board1 = createBoard();
  let board2 = createBoard();

  let player1Tiles = Array.from({ length: tilesPerPlayer }, () =>
    String.fromCharCode(65 + Math.floor(Math.random() * 26))
  );
  let player2Tiles = Array.from({ length: tilesPerPlayer }, () =>
    String.fromCharCode(65 + Math.floor(Math.random() * 26))
  );

  let currentPlayer = 1;
  let selectedTile = null;
  let highlightedTileIndex = null;

  const isValidMove = (x, y, board) => {
    if (board[x][y]) return false;

    const directions = [
      [-1, 0],
      [1, 0],
      [0, -1],
      [0, 1]
    ];

    for (const [dx, dy] of directions) {
      const nx = x + dx;
      const ny = y + dy;
      if (nx >= 0 && nx < boardSize && ny >= 0 && ny < boardSize && board[nx][ny]) {
        return true;
      }
    }

    return false;
  };

  const handleDragStart = (tile, index) => {
    selectedTile = tile;
    highlightedTileIndex = index;
  };

  const handleDrop = (event, x, y, board, playerTiles) => {
    event.preventDefault();

    if (selectedTile && currentPlayer === (playerTiles === player1Tiles ? 1 : 2)) {
      if (isValidMove(x, y, board)) {
        board[x][y] = { type: "tile", player: currentPlayer, letter: selectedTile };

        const tileIndex = playerTiles.indexOf(selectedTile);
        if (tileIndex !== -1) {
          playerTiles.splice(tileIndex, 1);
          playerTiles.push(String.fromCharCode(65 + Math.floor(Math.random() * 26)));
          highlightedTileIndex = playerTiles.length - 1;
        }

        selectedTile = null;
        currentPlayer = currentPlayer === 1 ? 2 : 1;

        board2 = board1.map((row) => [...row]);
      }
    }
  };

  const handleDragOver = (event) => {
    event.preventDefault();
  };
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

  .cell.tile {
    background-color: #fff;
  }

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
    cursor: grab;
  }

  .tile.dragging {
    opacity: 0.5;
    cursor: grabbing;
  }

  .tile.highlighted {
    background-color: #ffeb3b;
  }

  .turn-indicator {
    text-align: center;
    margin-top: 20px;
    font-size: 20px;
    font-weight: bold;
  }

  .board-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>

<div class="game-container">
  <div class="board-container">
    <div class="board">
      {#each board1 as row, i}
        {#each row as cell, j}
          <div
            class="cell {cell?.type === 'artifact' && cell?.player === 1 ? 'artifact-player1' : ''} 
                        {cell?.type === 'artifact' && cell?.player === 2 ? 'artifact-player2' : ''}
                        {cell?.type === 'home' && cell?.player === 1 ? 'home-player1' : ''} 
                        {cell?.type === 'home' && cell?.player === 2 ? 'home-player2' : ''}
                        {cell?.type === 'tile' ? 'tile' : ''}"
            on:dragover={handleDragOver}
            on:drop={(event) => handleDrop(event, i, j, board1, player1Tiles)}
          >
            {cell?.type === "tile" ? cell.letter : ""}
          </div>
        {/each}
      {/each}
    </div>
    <div class="tiles">
      {#each player1Tiles as tile, index}
        <div
          class="tile {selectedTile === tile ? 'dragging' : ''} {highlightedTileIndex === index ? 'highlighted' : ''}"
          draggable="true"
          on:dragstart={() => handleDragStart(tile, index)}
        >
          {tile}
        </div>
      {/each}
    </div>
  </div>

  <div class="board-container">
    <div class="board">
      {#each board2.reverse() as row, i}
        {#each row.reverse() as cell, j}
          <div
            class="cell {cell?.type === 'artifact' && cell?.player === 1 ? 'artifact-player1' : ''} 
                        {cell?.type === 'artifact' && cell?.player === 2 ? 'artifact-player2' : ''}
                        {cell?.type === 'home' && cell?.player === 1 ? 'home-player1' : ''} 
                        {cell?.type === 'home' && cell?.player === 2 ? 'home-player2' : ''}
                        {cell?.type === 'tile' ? 'tile' : ''}"
          >
            {cell?.type === "tile" ? cell.letter : ""}
          </div>
        {/each}
      {/each}
    </div>
    <div class="tiles">
      {#each player2Tiles as tile, index}
        <div
          class="tile {highlightedTileIndex === index ? 'highlighted' : ''}"
        >
          {tile}
        </div>
      {/each}
    </div>
  </div>
</div>

<div class="turn-indicator">
  Current Turn: Player {currentPlayer}
</div>
