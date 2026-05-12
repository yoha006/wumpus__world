<h1>ExpNo 9: Solve Wumpus World Problem using Python demonstrating Inferences from Propositional Logic</h1> 
<h3>Name:P.YOHALAKSHMI </h3>
<h3>Register Number:212224060314</h3>
<H3>Aim:</H3>
<p>
    To solve  Wumpus World Problem using Python demonstrating Inferences from Propositional Logic
</p>
<h1>Problem Description</h1>
<hr>
<h2>Wumpus World</h2>
<hr>
The Wumpus world is a simple world example to illustrate the worth of a knowledge-based agent and to represent knowledge representation.

The figure below shows a Wumpus world containing one pit and one Wumpus. There is an agent in room [1,1]. The goal of the agent is to exit the Wumpus world alive. The agent can exit the Wumpus world by reaching room [4,4]. The wumpus world contains exactly one Wumpus and one pit. There will be a breeze in the rooms adjacent to the pit, and there will be a stench in the rooms adjacent to Wumpus.

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/cd6b68dc-c79f-4dcb-8126-04da90d65912)

<center>Wumpus World Representation</center>
<p>
This is a python program that uses propositional logic sentences to check which rooms are safe. 

It is assumed that there will always be a safe path that the agent can take to exit the Wumpus world. The logical agent can take four actions: Up, Down, Left and Right. These actions help the agent move from one room to an adjacent room. The agent can perceive two things: Breeze and Stench.
</p>

<hr>
<h1>Sample Input and Output:</h1>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8696111a-a4a7-47cb-ba4b-43a4ef88573f)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/4be5bf06-79fa-4fa0-9334-38a33f06060b)

<hr>
<h3>Program:</h3>

```
import random

# ----------------- WORLD SETUP -----------------
SIZE = 4
world = [['' for _ in range(SIZE)] for _ in range(SIZE)]

# Place Gold
gold_pos = (random.randint(0, SIZE-1), random.randint(0, SIZE-1))
world[gold_pos[0]][gold_pos[1]] = 'G'

# Place Pits
num_pits = 3
pits = []

while len(pits) < num_pits:
    pit = (random.randint(0, SIZE-1), random.randint(0, SIZE-1))
    if pit not in pits and pit != gold_pos:
        pits.append(pit)
        world[pit[0]][pit[1]] = 'P'

# Place Wumpus
while True:
    wumpus = (random.randint(0, SIZE-1), random.randint(0, SIZE-1))
    if wumpus != gold_pos and wumpus not in pits:
        world[wumpus[0]][wumpus[1]] = 'W'
        break

# ----------------- GENERATE PERCEPTS -----------------
percepts = [['' for _ in range(SIZE)] for _ in range(SIZE)]

for i in range(SIZE):
    for j in range(SIZE):
        if world[i][j] == '':
            for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
                ni, nj = i+dx, j+dy
                if 0 <= ni < SIZE and 0 <= nj < SIZE:
                    if world[ni][nj] == 'P':
                        percepts[i][j] += 'B'   # Breeze
                    if world[ni][nj] == 'W':
                        percepts[i][j] += 'S'   # Stench

# ----------------- AGENT SETUP -----------------
agent_pos = (0, 0)
visited = set()
score = 0
safe_cells = set()
frontier = [(0, 0)]  # cells to explore


def print_world():
    print("WORLD:")
    for row in world:
        print(row)
    print()


def print_percepts():
    print("PERCEPTS:")
    for row in percepts:
        print(row)
    print()


def check_current_location():
    global score
    x, y = agent_pos

    if world[x][y] == 'P':
        print(f"Agent at {(x,y)}: FELL IN PIT! Game Over.")
        return True
    elif world[x][y] == 'W':
        print(f"Agent at {(x,y)}: EATEN BY WUMPUS! Game Over.")
        return True
    elif world[x][y] == 'G':
        print(f"Agent at {(x,y)}: GOLD FOUND! You won!")
        score += 1000
        print(f"Score: {score}")
        return True
    return False


def get_neighbors(pos):
    x, y = pos
    neighbors = []
    for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
        nx, ny = x+dx, y+dy
        if 0 <= nx < SIZE and 0 <= ny < SIZE:
            neighbors.append((nx, ny))
    return neighbors


# Simple inference: mark safe cells based on percepts
def infer_safe():
    global safe_cells
    for x, y in visited:
        if percepts[x][y] == '':
            for nx, ny in get_neighbors((x, y)):
                if (nx, ny) not in visited:
                    safe_cells.add((nx, ny))


# ----------------- AUTOMATIC AGENT LOOP -----------------
print_world()
print_percepts()

while frontier:
    agent_pos = frontier.pop(0)

    if agent_pos in visited:
        continue

    visited.add(agent_pos)
    score -= 10  # cost of moving

    print(f"Agent moving to {agent_pos}, percepts: {percepts[agent_pos[0]][agent_pos[1]]}")

    if check_current_location():
        break

    infer_safe()

    # Add new safe cells to frontier
    for cell in safe_cells:
        if cell not in visited and cell not in frontier:
            frontier.append(cell)

print("Final Score:", score)
```
<h3>Output:</h3>

<img width="444" height="405" alt="image" src="https://github.com/user-attachments/assets/e70efe37-f052-41f9-9eba-2aa7f5018f93" />
<hr>
<h3>Result:</h3>

Wumpus World Problem using Python demonstrating Inferences from Propositional Logic was solved.
