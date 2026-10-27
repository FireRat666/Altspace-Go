# Go

A synced multiplayer upright Go (Weiqi/Baduk) board. Made for **SideQuest / AltspaceVR** worlds, and it also works in **Banter**.

There is no server: players stay in sync through the space's shared state, and the game is plain static files served from <https://go.firer.at>.

To try it, open <https://go.firer.at> in Altspace or Banter.

## Features

- Captures, suicide prevention and the ko rule
- Pass and reset buttons, and a capture scoreboard
- 19×19, 13×13 or 9×9 boards
- Several independent boards in one world

## Add it to an Altspace world (snippet)

In a Creator SDK world, add an empty GameObject at the origin, click **Add Component > BS > Snippet** and type `go`. Or paste this into `Assets/WebRoot/index.html`, between the snippet section comments:

```html
<!-- snippet section -->
<bs-snippet name="go" title="Go"
  description="Synced multiplayer upright Go (Weiqi/Baduk) board with pass, reset and a scoreboard."
  script="https://go.firer.at/snippet.js"
  room=""
  position="0 1.5 2"
  rotation="0 0 0"
  scale="1"
  board-size="19"
  lit="true"
  add-lights="false"
  turn-cooldown="5"
  hide-ui="false"
  hide-board="false"
  use-custom-models="true"
  use-ao-baking="false"
  reset-position="0.3 -1.2 0"
  reset-rotation="0 0 0"
  reset-scale="1"
  pass-position="-0.3 -1.2 0"
  scoreboard-position="0 1.3 0">
  <bs-gizmo type="position" attribute="position"/>
  <bs-gizmo type="plane" attribute="position" size="1.52 1.52"/>
</bs-snippet>
<!-- end snippet section -->
```

- Each placement is its own game. Its sync id is `room`, or the placement's own id when `room` is empty. Give placements the same `room` to share one game.
- `position` and `rotation` are Unity world values, and `scale` can be a single number. The other settings are listed under [Settings](#settings). An empty setting uses the default.
- The Scene-view gizmo shows the board at its default size; it does not follow `scale`.
- Unity reads the snippet section as XML: quote every attribute, close every element, and write `&` as `&amp;`.

## Add it with a script tag

This works in an Altspace world's `Assets/WebRoot/index.html` (outside the snippet section) and in a Banter space's HTML:

```html
<script src="https://go.firer.at/Go.js" position="0 1.5 2" board-size="13"></script>
```

For more than one board in a space, give each one its own `instance`:

```html
<script src="https://go.firer.at/Go.js" instance="board1" position="-2 1.1 -2"></script>
<script src="https://go.firer.at/Go.js" instance="board2" position="2 1.1 -2"></script>
```

Settings can also go in the URL, as in older setups: `https://go.firer.at/Go.js?boardPosition=0+1.1+-2&boardScale=0.8`.

## Settings

| Setting | Also accepted | Default | Description |
| :--- | :--- | :--- | :--- |
| `instance` |  | page URL | Sync id. Boards with the same id share one game. In a snippet, use `room` instead. |
| `boardPosition` | `position`, `board-position` | `0 1.5 0` | Board position (x y z). |
| `boardRotation` | `rotation`, `board-rotation` | `0 0 0` | Board rotation in degrees (x y z). |
| `boardScale` | `scale`, `board-scale` | `1` | Board scale: one number, or x y z. |
| `boardSize` | `board-size` | `19` | Grid size, e.g. 19, 13 or 9. |
| `resetPosition` | `reset-position` | `0.3 -1.2 0` | Reset button position, relative to the board. |
| `resetRotation` | `reset-rotation` | `0 0 0` | Reset button rotation, relative to the board. |
| `resetScale` | `reset-scale` | `1` | Reset button scale. |
| `hideUI` | `hide-ui` | `false` | `true` makes the buttons and scoreboard invisible. The buttons still work and their labels stay visible. |
| `turnCooldown` | `turn-cooldown` | `5` | Seconds after a move before the same person can move for the other side, so a double click or an eager player can't take the opponent's turn. Playing both sides alone still works after the wait. `0` turns it off. |
| `hideBoard` | `hide-board` | `false` | `true` hides the board, e.g. to use your own. |
| `useCustomModels` | `use-custom-models` | `true` | Uses the GLB models in `Models/` for the pieces. `false` uses built-in shapes instead. |
| `lit` |  | `true` | Uses materials that react to scene lights. `false` uses unlit materials that ignore lighting. |
| `lighting` |  | `lit` | Older form of `lit`: `lit` or `unlit`. `lit` wins if both are set. |
| `addLights` | `add-lights` | `false` | With `lit="true"`, `true` adds a directional light, for worlds that have no lights of their own. |
| `useAOBaking` | `use-ao-baking` | `false` | Bakes ambient occlusion onto the board. |
| `passPosition` | `pass-position` | `-0.3 -1.2 0` | Pass button position, relative to the board. |
| `passRotation` | `pass-rotation` | `0 0 0` | Pass button rotation. |
| `passScale` | `pass-scale` | `1` | Pass button scale. |
| `scoreboardPosition` | `scoreboard-position` | `0 1.3 0` | Scoreboard position, relative to the board. |
| `scoreboardRotation` | `scoreboard-rotation` | `0 0 0` | Scoreboard rotation. |
| `scoreboardScale` | `scoreboard-scale` | `1` | Scoreboard scale. |

Settings can be written as `boardPosition` or `board-position`; the snippet uses the hyphenated form.

## How to play

1. Black moves first.
2. Click an intersection to place a stone.
3. Surround stones completely to capture them.
4. Click Pass if you don't want to move. Two passes in a row end the game.
5. Ko: you can't repeat the board position from the previous turn.

## Files

- `Go.js`: the game.
- `Models/`: stone models for `use-custom-models`.
- `snippet.js`, `snippet.html`: the Creator SDK snippet and its definition.
- `index.html`: a test page with a floor and the game.

## License

GPL-3.0. See [LICENSE](LICENSE).
