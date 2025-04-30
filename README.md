# ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
### Name: Shakthi kumar S
### Register Number: 212222110043

## Aim:
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game

## Goals of Alpha-Beta Pruning in MiniMax Search Algorithm

- Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.
- Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.
  
## Implementation::

- The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

### The Minimax algorithm:

- Recursively evaluates all possible moves and their potential outcomes, creating a game tree.

### Alpha-Beta pruning:

- Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.
- When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance

## Program:
```
import time

class Game:
    MIN = -2
    MAX = 2

    def __init__(self):
        self.initialize_game()

    def initialize_game(self):
        self.current_state = [['.', '.', '.'],
                              ['.', '.', '.'],
                              ['.', '.', '.']]
        self.player_turn = 'X'

    def draw_board(self):
        for row in self.current_state:
            print(" | ".join(row))
            print("-" * 9)

    def is_valid(self, px, py):
        if px < 0 or px > 2 or py < 0 or py > 2:
            return False
        elif self.current_state[px][py] != '.':
            return False
        else:
            return True

    def is_end(self):
        # Vertical win
        for i in range(0, 3):
            if self.current_state[0][i] != '.' and \
               self.current_state[0][i] == self.current_state[1][i] and \
               self.current_state[1][i] == self.current_state[2][i]:
                return self.current_state[0][i]

        # Horizontal win
        for i in range(0, 3):
            if self.current_state[i] == ['X', 'X', 'X']:
                return 'X'
            elif self.current_state[i] == ['O', 'O', 'O']:
                return 'O'

        # Main diagonal win
        if self.current_state[0][0] != '.' and \
           self.current_state[0][0] == self.current_state[1][1] and \
           self.current_state[0][0] == self.current_state[2][2]:
            return self.current_state[0][0]

        # Second diagonal win
        if self.current_state[0][2] != '.' and \
           self.current_state[0][2] == self.current_state[1][1] and \
           self.current_state[0][2] == self.current_state[2][0]:
            return self.current_state[0][2]

        # Check if board is full (no empty spaces)
        for row in self.current_state:
            if '.' in row:
                return None  # Game is not over yet

        return '.'  # It's a tie

    def max_alpha_beta(self, alpha, beta):
        maxv = self.MIN
        px = None
        py = None

        result = self.is_end()
        if result == 'X':
            return (-1, 0, 0)
        elif result == 'O':
            return (1, 0, 0)
        elif result == '.':
            return (0, 0, 0)

        for i in range(0, 3):
            for j in range(0, 3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'O'
                    (m, min_i, min_j) = self.min_alpha_beta(alpha, beta)
                    if m > maxv:
                        maxv = m
                        px = i
                        py = j
                    self.current_state[i][j] = '.'

                    # Alpha-Beta pruning
                    if maxv >= beta:
                        return (maxv, px, py)
                    if maxv > alpha:
                        alpha = maxv

        return (maxv, px, py)

    def min_alpha_beta(self, alpha, beta):
        minv = self.MAX
        qx = None
        qy = None

        result = self.is_end()
        if result == 'X':
            return (-1, 0, 0)
        elif result == 'O':
            return (1, 0, 0)
        elif result == '.':
            return (0, 0, 0)

        for i in range(0, 3):
            for j in range(0, 3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'X'
                    (m, max_i, max_j) = self.max_alpha_beta(alpha, beta)
                    if m < minv:
                        minv = m
                        qx = i
                        qy = j
                    self.current_state[i][j] = '.'

                    # Alpha-Beta pruning
                    if minv <= alpha:
                        return (minv, qx, qy)
                    if minv < beta:
                        beta = minv

        return (minv, qx, qy)

    def player_move(self):
        while True:
            try:
                px = int(input('Insert the X coordinate: '))
                py = int(input('Insert the Y coordinate: '))
                if self.is_valid(px, py):
                    return px, py
                else:
                    print('Invalid move, try again.')
            except ValueError:
                print('Invalid input, please enter integers for coordinates.')

    def play_alpha_beta(self):
        while True:
            self.draw_board()
            result = self.is_end()

            if result != None:
                if result == 'X':
                    print('The winner is X!')
                elif result == 'O':
                    print('The winner is O!')
                elif result == '.':
                    print("It's a tie!")

                self.initialize_game()
                return

            if self.player_turn == 'X':
                start = time.time()
                (m, qx, qy) = self.min_alpha_beta(self.MIN, self.MAX)
                end = time.time()
                print('Evaluation time: {}s'.format(round(end - start, 7)))
                print(f'Recommended move for X: X = {qx}, Y = {qy}')

                px, py = self.player_move()

                self.current_state[px][py] = 'X'
                self.player_turn = 'O'

            else:
                (m, px, py) = self.max_alpha_beta(self.MIN, self.MAX)
                self.current_state[px][py] = 'O'
                self.player_turn = 'X'

def main():
    g = Game()
    g.play_alpha_beta()

if __name__ == "__main__":
    main()

```
## Sample Input and Output:
![image](https://github.com/user-attachments/assets/1bbeb793-ec51-4a32-a2e6-bd9055c1b6c3)
![image](https://github.com/user-attachments/assets/b9b80862-bab7-49b1-8ac4-b0d3e221132a)
![image](https://github.com/user-attachments/assets/c831094a-ce8d-4695-b03a-90f4fdf926b1)

## Result:
Hence Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game has been implemented.
