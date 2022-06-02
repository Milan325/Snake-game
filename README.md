# Snake-game
Basic snake game script written in Python using `turtle` module.

## Rules
Snake is controled by keyboard arrows (up, down, left, right). The point of the game is to get the snake as long as possible before hitting the wall (edge of the window) or snake's tail. Extend snake's tail by eating green turtles.

## Modules, functions and classes
This game uses `turtle` module for creating: window, snake, food and scoreboard. `time.sleep(0.1)` is used to "hold" game loop until screen listens to keyboard and updates, so snake's body is synchronized.
### `snake.py`
```python 
class Snake:

    def __init__(self):
        self.segments = []
        self.create_snake()
        self.head = self.segments[0]
        
   def create_snake(self):
        for position in STARTING_POSITIONS:
            self.add_segment(position)

    def add_segment(self, position):
        new_segment = Turtle("square")
        new_segment.color("red")
        new_segment.penup()
        new_segment.goto(position)
        self.segments.append(new_segment)

    def extend(self):
        self.add_segment(self.segments[-1].position())
```
At the initialization, Snake object is created as a list of 3 `Turtle` objects in the shape of identical rectangles (or squares, as module creators call it), which edges touch to for an continuous tail. For this purpose, `create_snake()` runs `for` loop creating 3 segments using `add_segment(self, position)` function that creates wanted Turtle objects. `extend()` function adds new Turtle object at the end of `self.segments` list.

```python
    def move(self):
        for seg_num in range(len(self.segments) - 1, 0, -1):
            new_x = self.segments[seg_num - 1].xcor()
            new_y = self.segments[seg_num - 1].ycor()
            self.segments[seg_num].goto(new_x, new_y)
        self.head.forward(MOVE_DISTANCE)
```
This function iterates over `self.segments` list, gets coordinates of Turtle object and moves that object to the position of objech ahead, from last to the second turtle object. Fist segment moves for the distance of the Turtle square independently, because, in game engine, distance from head to tail segment lesser then 10px will result in game over. Also, this function runs as a part of game engine.

```python
    def up(self):
        if self.head.heading() != DOWN:
            self.head.setheading(UP)

    def down(self):
        if self.head.heading() != UP:
            self.head.setheading(DOWN)

    def left(self):
        if self.head.heading() != RIGHT:
            self.head.setheading(LEFT)

    def right(self):
        if self.head.heading() != LEFT:
            self.head.setheading(RIGHT)
```
This functions are tied to `sreen.onkey()` function and change diretion of snake's head (tail segments follow, as declared in `move()` function).

### `food.py`
This module creates Food object inheriting from Turtle class. At the initialization, Turtle object is created in the shape of a turtle, streched for 50% and, by randomly picking coordinates using `random.randint()` funtion, is placed on the screen.

### `scoreboard.py`
In this module, Scoreboard subclass is made using Turtle class. It hides Turtl object, puts it at the top of the window, and writes score using `update_scoreboard()`. Score is defined as int variable `self.score` and gets incremented by 1 every time snake's head hitts Food object. `increase_score()`, besides incrementing score variable, deletes previously written score using Turtle's `clear()` module.

## Game engine
Game engine starts by making `turtle.Screen()` object, and setting up game environment: making `Snake()`, `Food()` and `Scoreboard` objects. Screen takes input using `screen.listen()` function. Main part of the engine is `while` loop that checks positions of snake object and: 
- extends snake using `Snake.extend()` module if snake's head position is less then 15px from thefood object's position,
- breaks from the loop if snake's head position is out of window or less then 10px from the segment object's position.
If `running == False`, scoreboard object calls `game_over()` method to write "GAME OVER" at the center of the window.
At the breaking from the loop, `time.sleep(5)` is called, so the user can see "GAME OVER", because, screen object disappeares once the end of the script is reached.

## Running the game
This game is intended to be run by Python IDE or other Python interpeter. 
To install Python 3 see [Tutorials Point page](https://www.tutorialspoint.com/how-to-install-python-in-windows).
