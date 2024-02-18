# TicTacToe_Python

import tkinter as tk
from tkinter import messagebox

class TicTacToe:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("Tic Tac Toe")

        self.player1 = 'X'
        self.player2 = 'O'
        self.current_player = self.player1

        self.scores = {self.player1: 0, self.player2: 0}

        self.create_widgets()
        self.reset_board()

    def create_widgets(self):
        self.buttons = []
        for i in range(3):
            row = []
            for j in range(3):
                button = tk.Button(self.window, text='', font=('Arial', 30), width=5, height=2,
                                   command=lambda i=i, j=j: self.on_button_click(i, j))
                button.grid(row=i, column=j, padx=5, pady=5)
                row.append(button)
            self.buttons.append(row)

        self.info_label = tk.Label(self.window, text=f"Player {self.current_player}'s turn", font=('Arial', 16))
        self.info_label.grid(row=3, column=0, columnspan=3, pady=10)

        self.score_label = tk.Label(self.window, text=f"Scores: {self.player1} - {self.scores[self.player1]}, {self.player2} - {self.scores[self.player2]}", font=('Arial', 14))
        self.score_label.grid(row=4, column=0, columnspan=3)

        self.reset_button = tk.Button(self.window, text="Reset", font=('Arial', 14), command=self.reset_board_and_scores)
        self.reset_button.grid(row=5, column=0, columnspan=3, pady=10)

    def on_button_click(self, row, col):
        if self.buttons[row][col]['text'] == '':
            self.buttons[row][col]['text'] = self.current_player
            if self.check_winner():
                self.scores[self.current_player] += 1
                self.update_score_label()
                messagebox.showinfo("Winner", f"Player {self.current_player} wins!")
                self.reset_board()
            elif self.check_draw():
                messagebox.showinfo("Draw", "It's a draw!")
                self.reset_board()
            else:
                self.toggle_player()

    def toggle_player(self):
        self.current_player = self.player2 if self.current_player == self.player1 else self.player1
        self.info_label.config(text=f"Player {self.current_player}'s turn")

    def check_winner(self):
        for i in range(3):
            if self.buttons[i][0]['text'] == self.buttons[i][1]['text'] == self.buttons[i][2]['text'] != '':
                self.highlight_winner([self.buttons[i][0], self.buttons[i][1], self.buttons[i][2]])
                return True
            if self.buttons[0][i]['text'] == self.buttons[1][i]['text'] == self.buttons[2][i]['text'] != '':
                self.highlight_winner([self.buttons[0][i], self.buttons[1][i], self.buttons[2][i]])
                return True
        if self.buttons[0][0]['text'] == self.buttons[1][1]['text'] == self.buttons[2][2]['text'] != '':
            self.highlight_winner([self.buttons[0][0], self.buttons[1][1], self.buttons[2][2]])
            return True
        if self.buttons[0][2]['text'] == self.buttons[1][1]['text'] == self.buttons[2][0]['text'] != '':
            self.highlight_winner([self.buttons[0][2], self.buttons[1][1], self.buttons[2][0]])
            return True
        return False

    def highlight_winner(self, cells):
        for cell in cells:
            cell.config(bg='green', fg='white')

    def check_draw(self):
        for row in self.buttons:
            for button in row:
                if button['text'] == '':
                    return False
        return True

    def update_score_label(self):
        self.score_label.config(text=f"Scores: {self.player1} - {self.scores[self.player1]}, {self.player2} - {self.scores[self.player2]}")

    def reset_board_and_scores(self):
        self.scores = {self.player1: 0, self.player2: 0}
        self.update_score_label()
        self.reset_board()

    def reset_board(self):
        for row in self.buttons:
            for button in row:
                button.config(text='', bg='SystemButtonFace', fg='black')
        self.current_player = self.player1
        self.info_label.config(text=f"Player {self.current_player}'s turn")

    def run(self):
        self.window.mainloop()

if __name__ == "__main__":
    game = TicTacToe()
    game.run()
