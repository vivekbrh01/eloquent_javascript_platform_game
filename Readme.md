# Platform Game

## How to play?

- Player's task is to collect the yellow boxes i.e. Coins while avoiding red structures i.e. Lava.
- A level is completed when all the coins have been collected.
- Player can move around with left, right and up arrow keys.
- The player can change direction in mid-air and can jump several times its own height.

## Structure of the game

- The background is static and laid out like a grid. The moving elements are overlaid on that background.
- Each field on the grid is either empty, solid or lava.
- Browser DOM display is used to display the game, while reading user input by handling key events.
- The background is represented as a table, since it is unchanging grid of squares.
- Free-moving elements are overlaid using absolutely positioned elements.

## Levels

- We have used strings to represent levels. It is done so that we get a human-readable and human-editable way to specify the levels.
- Big strings are being used in which each character represents an element.
- Periods '.' - Empty spaces.
- '#' - Walls
- '+' - Static Lava
- '@' - Starting position of the player
- 'O' - Coins
- '=' - Block of lava that moves back and forth horizontally
- '|' - Vertically moving blobs of lava
- 'V' - Vertically moving lava that dosen't bounce back and forth.

## Reading a level

The Level class stores a level object. The argument is the string that defines the level.

```js
var Level = class Level {
	constructor(plan) {
		let rows = plan
			.trim()
			.split("\n")
			.map((l) => [...l]);
		this.height = rows.length;
		this.width = rows[0].length;
		this.startActors = [];

		this.rows = rows.map((row, y) => {
			return row.map((ch, x) => {
				let type = levelChars[ch];
				if (typeof type == "string") return type;
				this.startActors.push(type.create(new Vec(x, y), ch));
				return "empty";
			});
		});
	}
};
```

- Trim method -> removes white spaces at the start and end of the plan. This results in our example plan to start with a new line.
- It is done so that all the lines are directly below each other.
- The remaining string is split on new line characters. Each line is spread into an array, producing arrays of characters.
- Therefore, 'rows' variable holds an array of characters i.e. the rows of the plan. The width and height of the level is derived from 'rows' varible.
- Moving elements, mentioned as Actors are stored in an array of objects.
- Background grid is stored as an array of arrays of strings. It holds feild types such as 'empty', 'wall' or 'lava'.
- To create these 'array of objects' and 'array of array of strings' we map over the rows and then map over their content.
- Map passes the array index as a second argument to the maping function. This second argument tells us the x and y coordinates of a given character.
- POsitions are stored as pairs of coordinates. They are calculated from the top-left corner. The height and width of each background square is 1 unit.
- Level constructor uses 'levelChars' object to interpret the characters in the plan.
- The 'levelChars' object maps background elements to strings and actor characters to classes.
- When 'type' variable is actor class its static create method is used to create an object.
- This created object is added to start actors and the mapping function return 'empty' for this background square.
- The position of the actor is stored as a Vec object. This is a 2D vector. An object with x and y properties.
- As the game runs, actors will end up in different places (player and lava) or even disappear entirely (coins)
