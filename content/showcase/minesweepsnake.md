+++
title = 'Minesweep-Snake'
date = 2024-10-10T18:41:27+02:00
draft = false
math = true
+++

Visit [GitHub]("https://github.com/DragonStorm97/minesweeper3d") for the code, and more details.

{{< wasm border_width=0 width="100%" src="https://cdn.jsdelivr.net/gh/DragonStorm97/minesweeper3d@release-artifacts-latest/release/raw/minesweeper3d/minesweeper3d.js" >}}

## How To Minesweeper

**To Start**:

- Set the grid size (recommended is 12)
- Set the number of bombs (recommended is 12)
- Optionally, Enable **Snake Mode** and set its speed (recommended is 1)

**Cell Number**:

- Each cell contains the number of adjacent cells that contain a bomb.
- An adjacent cell is any of the 8 cells surrounding a cell.
- There are exactly the number of bombs in those 8 cells as the number represents.

**Steps**:

- Start by uncovering the first cell (will never be a bomb).
- Use the knowledge provided by the cell's number to determine/guess which adjacent
  cell might or might not be a bomb.
- Optionally, flag bombs (or cells you think are bombs that
  you want to determine later).
- Uncover a bomb to lose, or uncover all non-bomb cells to win!
- use w/a/s/d or arrow keys to move the snake (in **Snake Mode**).
