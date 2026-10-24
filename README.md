# Banter Go (Weiqi/Baduk)

A fully synced, multiplayer Go game designed for [Banter](https://bantervr.com).

## Features

* **Multiplayer Sync:** Game state is synchronized across all users in the space using Banter's public space properties.
* **Full Ruleset:** Implements standard Go rules including:
* **Liberties & Capturing:** Stones are removed when they have no liberties left.
* **Suicide Prevention:** Moves that result in self-capture are forbidden (unless they capture opponent stones).
* **Ko Rule:** Prevents immediate repetition of the board state.
* **UI Elements:**
* **Scoreboard:** Tracks captured stones for Black and White.
* **Pass Button:** Allows players to pass their turn. The game logic detects when the game should end (though manual agreement is often used).
* **Reset Button:** Clears the board to start a new game.

## Usage

To add the Go board to your Banter space, include the `Go.js` script in your `index.html`.

### Basic Embed

```html
<script src="https://banter-go.firer.at/Go.js"></script>
```

### Configuration

You can configure the board's appearance and behavior using **HTML script attributes** (recommended) or legacy **URL query parameters** (for backwards compatibility).

| Parameter / Attribute | Description | Default | Example (Attribute) |
| :--- | :--- | :--- | :--- |
| `instance` | Unique ID for this game instance. Use this to have multiple separate games in one space. | `URL path` | `instance="table1"` |
| `boardSize` / `board-size` | Grid size (e.g., 19, 13, 9). | `19` | `boardSize="9"` |
| `hideUI` / `hide-ui` | Makes the UI (buttons, scoreboard) invisible but still functional. | `false` | `hideUI="true"` |
| `hideBoard` / `hide-board` | Hides the board geometry. | `false` | `hideBoard="true"` |
| `useCustomModels` / `use-custom-models` | Use GLB models for pieces. | `false` | `useCustomModels="true"` |
| `lighting` | Shader type: 'lit' or 'unlit'. | `unlit` | `lighting="lit"` |
| `addLights` / `add-lights` | Add directional light for lit mode. | `true` | `addLights="false"` |
| `useAOBaking` / `use-ao-baking` | Bake Ambient Occlusion on board geometry. | `false` | `useAOBaking="true"` |
| `boardPosition` / `position` / `board-position` | Position of the game board (x y z). | `0 1.5 0` | `boardPosition="0 1 -2"` |
| `boardRotation` / `rotation` / `board-rotation` | Rotation of the game board (x y z). | `0 0 0` | `boardRotation="0 45 0"` |
| `boardScale` / `scale` / `board-scale` | Scale of the game board (x y z). | `1 1 1` | `boardScale="0.5 0.5 0.5"` |
| `scoreboardPosition` / `scoreboard-position` | Position of the scoreboard, relative to world origin. | `0 1.3 0` | `scoreboardPosition="0 2.5 -2"` |
| `scoreboardRotation` / `scoreboard-rotation` | Rotation of the scoreboard. | `0 0 0` | `scoreboardRotation="0 180 0"` |
| `scoreboardScale` / `scoreboard-scale` | Scale of the scoreboard. | `1 1 1` | `scoreboardScale="1.2 1.2 1.2"` |
| `resetPosition` / `reset-position` | Position of the Reset button, relative to world origin. | `0.3 -1.2 0` | `resetPosition="1 1 -2"` |
| `resetRotation` / `reset-rotation` | Rotation of the Reset button. | `0 0 0` | `resetRotation="0 0 30"` |
| `resetScale` / `reset-scale` | Scale of the Reset button. | `1 1 1` | `resetScale="2 2 2"` |
| `passPosition` / `pass-position` | Position of the Pass button, relative to world origin. | `-0.3 -1.2 0` | `passPosition="-1 1 -2"` |
| `passRotation` / `pass-rotation` | Rotation of the Pass button. | `0 0 0` | `passRotation="0 0 -30"` |
| `passScale` / `pass-scale` | Scale of the Pass button. | `1 1 1` | `passScale="2 2 2"` |

**Examples:**

**HTML Script Attributes (Recommended):**
```html
<script src="https://banter-go.firer.at/Go.js" boardPosition="0 1 2" boardSize="13" instance="game1"></script>
```

**Legacy URL Query Parameters:**
```html
<script src="https://banter-go.firer.at/Go.js?boardPosition=0+1+2&boardSize=13&instance=game1"></script>
```

## SideQuest / Altspace Creator SDK (Snippet)

In a SideQuest/Altspace Creator SDK world, add an empty GameObject at the origin, click **Add Component > BS > Snippet** and type `go`. Or paste it into `Assets/WebRoot/index.html` between the snippet section comments:

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
  lighting="unlit"
  add-lights="true"
  hide-ui="false"
  hide-board="false"
  use-custom-models="false"
  use-ao-baking="false"
  reset-position="0.3 -1.2 0"
  pass-position="-0.3 -1.2 0"
  scoreboard-position="0 1.3 0">
  <bs-gizmo type="position" attribute="position"/>
  <bs-gizmo type="plane" attribute="position" size="1.52 1.52"/>
</bs-snippet>
<!-- end snippet section -->
```

- Settings are the same as the script attributes above. `position` and `rotation` are Unity world values, and `scale` can be a single number.
- Each placement is its own game. Its sync id is `room`, or the placement's own id when `room` is empty. Give placements the same `room` to share one game.
- An empty setting uses the game's default.
- The Scene-view gizmo shows the board at its default size; it does not follow `scale`.
- Unity reads the snippet section as XML: quote every attribute, close every element, and write `&` as `&amp;`.

## How to Play

1. **Black** moves first.
2. Click an intersection on the grid to place a stone.
3. **Capturing:** Surround an opponent's stone or group of stones completely to capture them.
4. **Passing:** If you cannot make a beneficial move, click the **Pass** button.
5. **Ko:** You cannot make a move that repeats the exact board position of the previous turn.
