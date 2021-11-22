# Pacman Graph Computations

Based on the PacMan Assignment designed by Jim de St. Germain, University of Utah

## Overview

We have been asked to create a tool to help Pacman solve mazes. More generally, we must create a tool that can find the shortest path from one location to another in any enclosed field of obstacles. We are given the maze as input, represented as a simple text file. The start-point and end point are indicated in this text file, as well as the layout of the maze (the location of walls). We must produce a similar text file, but with the shortest path from the start-point to the end-point added to the maze. If there is no possible path, the output file will mirror the input file.

To solve this problem, you must take a text file that describes a maze, transform this into a graph representation (using a dependency graph implementation), and perform a breadth-first search (using a Queue) from Pacman's starting location to the goal location, finding the shortest path. You will then output a new text file showing the maze and path.

## Key Ideas to be Learned, Reviewed, & Relearned:

1. Implementing Graphs
2. Breadth First Search Algorithm to find the shortest path in a Graph
3. Ability to write code to a limited set of instructions and constraints

## Project Setup
This will be a project where little starter code is provided. You will follow the few requirements, but then beyond that, it is up to you to figure out the project and how to solve the problem.

Your Pathfinder.java must contain the following method:

`public static void solveMaze(String inputFileName, String outputFileName) { }`
              
This method will read a maze from a file with the given input name, and output the solved maze to a file with the given output name. This method must use a graph and graph pathfinding to solve the problem.

It is suggested that you create a class called: Graph.java
This is the data structure you will use to perform the breadth-first search algorithm in order to solve the shortest path problem.

Place your tests in the PathfinderTest.java file. You should at least have a test for each of the provided mazes.

## Writing the Program
### Input File
The input files contain text similar to the following example:

```
10 19
XXXXXXXXXXXXXXXXXXX
X       S         X
X                 X
X                 X
XX X       X      X
X X XXX      X    X
X G    XX         X
X                 X
X                 X
XXXXXXXXXXXXXXXXXXX
```
  
The first line contains two numbers, separated by a space. The first number is the number of rows (height of the maze), and the second is the number of columns (width of the maze). The rest of the lines contain the layout of the field. The characters have the following meaning:

- X - A single wall segment. Pacman can not travel on to or through walls.

- S - The starting point of the path that we are trying to find (where Pacman starts). This is an open space (no wall).

- G - The ending point of the path we are trying to find (where Pacman wants to be). This is an open space (no wall).

- (space) - An open space on the field. Pacman is free to travel in any open space, assuming he can get there.

- (new line) - all lines in the file are terminated with a newline character.

You can assume that all input mazes will be rectangular. All of the border positions around the perimeter of the field will be walls (the field is fully enclosed). If a file violates this condition, you should throw a generic run time exception with the message ("Illegal File Format");

### Output File
The output of your program should be a file containing an image of the maze exactly like the original input file, but with the following changes: replace the open spaces (space characters) with dot characters ('.') representing the path pacman should follow from the Start to the Goal. The spaces that you replace with dots are the spaces on the shortest path from the start point to the goal point. For example, given the above input file, the following output would be produced:

```
10 19
XXXXXXXXXXXXXXXXXXX
X       S.        X
X        .        X
X        .        X
XX X     . X      X
X X XXX  .   X    X
X G....XX.        X
X     ....        X
X                 X
XXXXXXXXXXXXXXXXXXX
```
    
### Multiple Paths
If there are multiple shortest path solutions (as there are in the above example), any connected path from 'S' to 'G' with the same length is a correct solution. For example, another acceptable solution to the above maze is:

```
10 19
XXXXXXXXXXXXXXXXXXX
X       S.        X
X        .        X
X        .        X
XX X     . X      X
X X XXX  .   X    X
X G.   XX.        X
X  .......        X
X                 X
XXXXXXXXXXXXXXXXXXX
```
    
If there is no path from 'S' to 'G', simply do not place any dots in the output file (i.e., return the original maze).

### Sample Mazes
See the Class Files Assignments section for a set of test mazes with solutions.

### Maze Traversal Rules
Pacman is free to move from his current location (at the start this would be 'S') to any adjacent open space. This includes the space directly above, below, left and right of where he is. It does not include diagonally adjacent spaces. If any of the adjacent spaces are a wall, Pacman can not travel in that direction. The path that your maze solver finds must be a connected path (it can't skip spaces and have Pacman "jumping" over walls or empty spaces).

More specifically:

- The path cannot go through or on top of walls.
- The path must be connected (no skips or jumps).
- Diagonally-adjacent spaces are not connected.
  - Only up, left, down, right
- If no path exists, the output file will be the same as the input file.
- If multiple shortest paths exist, any one is valid return result.
- The output of your program must match the exact format specified. This allows us to use automatic grading scripts.
- Your program must run in 10 seconds or less on all mazes we test. This includes file input, graph construction, pathfinding, and file output. The biggest test maze is 100x100 (see the randomMaze file). There will be other test mazes of similar size.

### Sample code for Reading/Writing from/to Files
An example of reading numbers (height and width) form a file, assuming you're using a BufferedReader named `input` to read the file:

```
String[] dimensions = input.readLine().split(" ");
height = Integer.parseInt(dimensions[0]);
width = Integer.parseInt(dimensions[1]);
```
          
An example of creating a file and writing to it in Java is below (where outputFile is a String). It will create the specified file if it does not exist. Use the exact String outputFile passed in to your solveMaze method (do not modify the path of the file).

```
try(PrintWriter output = new PrintWriter(new FileWriter(outputFile)))
{
	output.println(height + " " + width);
	// write more data here
}
```
          
### Program guidelines
Other than the required PathFinder class with method solveMaze (see above), the design of your program is completely up to you. While you can create a main java file with a main method, you are allowed (and encouraged) to solve each of the puzzles within your JUnit tests.


```
@Test
void testTinyMaze() {
	Pathfinder.solveMaze("src/main/resources/mazes/tinyMaze.txt", "src/main/resources/solvedMazes/tinyMazeOutput.txt");
	// do some assertions to test the accuracy of your txt files
}
```
          
REMEMBER: You have to refresh your project to see the output in your solvedMazes folder.

### Graph Hint
I suggest that to generate the Graph, you first read the file in and create a 2Darray of integers (default values of -1). As your code parses the text file line by line, any time you come to an empty space (or start/end goal) you should create a new node, give this new node a unique (incremented) id, and place this node in the graph. I would then suggest re-parsing the 2D array, and for each non-negative number, check for edges to the N,S,E, and W. If an edge exists, add this edge to the dependency graph representation of the graph.

### Queue Hint
You will need to use a Queue data structure for your Breadth First Search algorithm to find the shortest path. DO NOT use Java's PriorityQueue. This Queue does not behave exactly as you need it to. You will need to use an acutal queue. Hint: Java's LinkedList implements the Queue interface.

## README Info and Code Pledge
In addition to the information provided for the assignment requirements, sometimes when you create your program, you will want to inform the user (in this case the Teacher) something about how to run your program, or what cool features you have added that are not obvious. Please add TO THE END of this README (in the "Any Additional Information" section) any information that should be noted to the teacher.

Please fill out the following:

Your Name: 

Your ID: 

The Date: 

The Class Number: 

The Assignment Number: 

Your partner name (when appropriate): 

I pledge that the work done here was my own and that I have learned how to write this program, such that I could throw it out and restart and finish it in a timely manner. I am not turning in any work that I cannot understand, describe, or recreate. I further acknowledge that I contributed substantially to all code handed in and vouch for it's authenticity. (YOUR-NAME)

TODO: Type your name at the end of this pledge to acknowledge it.

### Any Additional Information

