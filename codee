import random
import tkinter as tk
from tkinter import messagebox

def get_digits(num):
    return [int(i) for i in str(num)]

def has_no_duplicates(num):
    num_digits = get_digits(num)
    return len(num_digits) == len(set(num_digits))

def generate_number(digits=4):
    """Generates a number with unique digits of a specified length."""
    while True:
        num = random.randint(10**(digits - 1), 10**digits - 1)
        if has_no_duplicates(num):
            return num

def count_bulls_and_cows(secret, guess):
    bulls = 0
    cows = 0
    secret_digits = get_digits(secret)
    guess_digits = get_digits(guess)

    # Create lists to track unmatched digits
    unmatched_secret = []
    unmatched_guess = []

    # First pass: count bulls and prepare unmatched lists
    for i in range(len(secret_digits)):
        if guess_digits[i] == secret_digits[i]:
            bulls += 1
        else:
            unmatched_secret.append(secret_digits[i])
            unmatched_guess.append(guess_digits[i])

    # Second pass: count cows
    for digit in unmatched_guess:
        if digit in unmatched_secret:
            cows += 1
            unmatched_secret.remove(digit)  # Remove to prevent double counting

    return bulls, cows

class BullsAndCowsGame:
    def __init__(self, master):
        self.master = master
        self.master.title("🎮 Bulls and Cows Game 🎮")
        self.master.config(bg="#f0f0f0")

        self.num_digits = 4
        self.max_tries = 10
        self.secret_number = generate_number(self.num_digits)
        self.tries = self.max_tries
        self.previous_guesses = []

        # Title and instructions
        self.title_label = tk.Label(master, text="🎉 Welcome to Bulls and Cows! 🎉", font=("Arial", 24, "bold"), bg="#f0f0f0", fg="#4a86e8")
        self.title_label.pack(pady=10)

        self.instruction_label = tk.Label(master, text="🕵‍♂ Enter your guess (4 unique digits):", font=("Arial", 16), bg="#f0f0f0")
        self.instruction_label.pack(pady=5)

        self.rules_label = tk.Label(master, text="🎯 Bulls: Correct digits in the correct position.\n"
                                                  "🐄 Cows: Correct digits but in the wrong position.", 
                                     font=("Arial", 14), bg="#f0f0f0")
        self.rules_label.pack(pady=10)

        self.guess_entry = tk.Entry(master, font=("Arial", 16), width=10, justify='center')
        self.guess_entry.pack(pady=5)
        self.guess_entry.bind("<Return>", self.check_guess)  # Bind Enter key

        self.submit_button = tk.Button(master, text="✅ Submit Guess", command=self.check_guess, bg="#4caf50", fg="white", font=("Arial", 16, "bold"))
        self.submit_button.pack(pady=10)

        self.tries_label = tk.Label(master, text=f"🕒 Tries left: {self.tries}", font=("Arial", 16), bg="#f0f0f0")
        self.tries_label.pack(pady=5)

        self.previous_guesses_display = tk.Text(master, height=10, width=30, font=("Arial", 16), bg="#f0f0f0")
        self.previous_guesses_display.pack(pady=5)
        self.previous_guesses_display.config(state=tk.DISABLED)  # Make it read-only

    def check_guess(self, event=None):
        guess_str = self.guess_entry.get()
        
        if not guess_str.isdigit() or len(guess_str) != self.num_digits or not has_no_duplicates(int(guess_str)):
            messagebox.showerror("❌ Invalid Input", f"Please enter a valid {self.num_digits}-digit number with unique digits.")
            return

        guess = int(guess_str)
        bulls, cows = count_bulls_and_cows(self.secret_number, guess)

        self.previous_guesses.append(guess_str)
        self.tries -= 1
        self.tries_label.config(text=f"🕒 Tries left: {self.tries}")

        # Display the results for the current guess
        result_text = f"{guess_str}: {bulls} bulls, {cows} cows!\n"

        # Update the text box with all previous guesses
        self.previous_guesses_display.config(state=tk.NORMAL)  # Enable editing
        self.previous_guesses_display.insert(tk.END, result_text)  # Add new result
        self.previous_guesses_display.config(state=tk.DISABLED)  # Disable editing again

        if bulls == self.num_digits:
            messagebox.showinfo("🎉 Congratulations! 🎉", "You guessed the number! 🎊")
            self.reset_game()
        elif self.tries == 0:
            messagebox.showinfo("😢 Game Over", f"You ran out of tries! The secret number was {self.secret_number}.")
            self.reset_game()

        self.guess_entry.delete(0, tk.END)  # Clear the entry after submission

    def reset_game(self):
        self.secret_number = generate_number(self.num_digits)
        self.tries = self.max_tries
        self.tries_label.config(text=f"🕒 Tries left: {self.tries}")
        self.previous_guesses_display.config(state=tk.NORMAL)  # Enable editing to clear it
        self.previous_guesses_display.delete(1.0, tk.END)  # Clear all previous guesses
        self.previous_guesses_display.config(state=tk.DISABLED)  # Disable editing again
        self.guess_entry.delete(0, tk.END)

if __name__ == "__main__":
    root = tk.Tk()
    game = BullsAndCowsGame(root)
    root.mainloop()
