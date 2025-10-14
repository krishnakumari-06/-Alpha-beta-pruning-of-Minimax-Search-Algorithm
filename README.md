<h1>ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game</h1> 
<h3>Name: Persys freeda J    </h3>
<h3>Register Number/Staff Id: 212224060185    </h3>
<H3>Aim:</H3>
<p>
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
</p>
<h1>GOALS of Alpha-Beta Pruning in MiniMax Search Algorithm</h1>

<h3>Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.</h3>
<h3>Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.</h3>
<h1>IMPLEMENTATION</h1>

The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

<h1>The Minimax algorithm</h1>

recursively evaluates all possible moves and their potential outcomes, creating a game tree.

<h1>Alpha-Beta pruning</h1>

Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.

When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance
<hr>
<h2>Sample Input and Output:</h2>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8d5e329a-9aff-41a6-bcf0-46efa10e1b92)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/438b242d-54ba-443e-b040-a936e6ae3b55)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/99a33390-fa11-4ade-a19f-e93bcd7aaec9)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/440797bd-53cb-49c1-b18d-89776864c3e7)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/81575a16-26b2-46f1-a8ac-27c9ed0a0fe5)

# Tic-Tac-Toe using Minimax Algorithm with Alpha-Beta Pruning

import math

# Print the board
def print_board(board):
    print("\n")
    for i in range(3):
        print(" | ".join(board[i]))
        if i < 2:
            print("--+---+--")
    print("\n")

# Check if moves are left
def is_moves_left(board):
    for row in board:
        if "_" in row:
            return True
    return False

# Evaluate the board
def evaluate(board):
    # Check rows for victory
    for row in board:
        if row[0] == row[1] == row[2] and row[0] != "_":
            if row[0] == "O":
                return +10
            elif row[0] == "X":
                return -10

    # Check columns for victory
    for col in range(3):
        if board[0][col] == board[1][col] == board[2][col] and board[0][col] != "_":
            if board[0][col] == "O":
                return +10
            elif board[0][col] == "X":
                return -10

    # Check diagonals for victory
    if board[0][0] == board[1][1] == board[2][2] and board[0][0] != "_":
        if board[0][0] == "O":
            return +10
        elif board[0][0] == "X":
            return -10

    if board[0][2] == board[1][1] == board[2][0] and board[0][2] != "_":
        if board[0][2] == "O":
            return +10
        elif board[0][2] == "X":
            return -10

    # Otherwise no winner
    return 0

# Minimax function with Alpha-Beta pruning
def minimax(board, depth, alpha, beta, isMax):
    score = evaluate(board)

    # Terminal states
    if score == 10:
        return score - depth
    if score == -10:
        return score + depth
    if not is_moves_left(board):
        return 0

    if isMax:  # Maximizer (AI)
        best = -math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == "_":
                    board[i][j] = "O"
                    val = minimax(board, depth + 1, alpha, beta, False)
                    board[i][j] = "_"
                    best = max(best, val)
                    alpha = max(alpha, best)
                    if beta <= alpha:
                        break
        return best

    else:  # Minimizer (Human)
        best = math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == "_":
                    board[i][j] = "X"
                    val = minimax(board, depth + 1, alpha, beta, True)
                    board[i][j] = "_"
                    best = min(best, val)
                    beta = min(beta, best)
                    if beta <= alpha:
                        break
        return best

# Find the best move for AI
def find_best_move(board):
    best_val = -math.inf
    best_move = (-1, -1)

    for i in range(3):
        for j in range(3):
            if board[i][j] == "_":
                board[i][j] = "O"
                move_val = minimax(board, 0, -math.inf, math.inf, False)
                board[i][j] = "_"
                if move_val > best_val:
                    best_move = (i, j)
                    best_val = move_val
    return best_move

# Check for winner
def check_winner(board):
    score = evaluate(board)
    if score == 10:
        return "O"
    elif score == -10:
        return "X"
    elif not is_moves_left(board):
        return "Draw"
    else:
        return None

# Main game loop
def play_game():
    board = [["_", "_", "_"], ["_", "_", "_"], ["_", "_", "_"]]
    print("Welcome to Tic-Tac-Toe with Alpha-Beta Pruning!")
    print_board(board)

    while True:
        # Player move
        print("Your turn (X): ")
        x = int(input("Enter row (0-2): "))
        y = int(input("Enter column (0-2): "))
        if board[x][y] == "_":
            board[x][y] = "X"
        else:
            print("Invalid move, try again.")
            continue

        print_board(board)
        if check_winner(board):
            break

        # AI move
        print("AI is making a move...")
        move = find_best_move(board)
        board[move[0]][move[1]] = "O"
        print_board(board)

        winner = check_winner(board)
        if winner:
            break

    # Display result
    winner = check_winner(board)
    if winner == "X":
        print("Result : You Win!")
    elif winner == "O":
        print("Result : AI Wins!")
    else:
        print("Result : It's a Draw!")

# Run the game
if __name__ == "__main__":
    play_game()




<img width="682" height="897" alt="image" src="https://github.com/user-attachments/assets/595c1037-d47e-440f-8deb-084eb122c885" />

# result 
the output has been executed </h1>
