#!/usr/bin/env python3
"""Hangman: a classic word-guessing game for the terminal.

Run with:  python hangman.py
"""

import random

MAX_MISSES = 6

WORDS = {
    "Animals": ["elephant", "giraffe", "penguin", "kangaroo", "dolphin", "octopus", "cheetah", "crocodile"],
    "Programming": ["python", "variable", "function", "compiler", "algorithm", "recursion", "database", "debugger"],
    "Countries": ["australia", "brazil", "canada", "denmark", "germany", "iceland", "japan", "mexico"],
    "Food": ["spaghetti", "chocolate", "avocado", "pancake", "burrito", "sandwich", "dumpling", "pineapple"],
}

# One picture per number of misses (0 through 6).
# Raw strings keep the backslashes exactly as written.
STAGES = [
    r"""
  +---+
  |   |
      |
      |
      |
      |
=========""",
    r"""
  +---+
  |   |
  O   |
      |
      |
      |
=========""",
    r"""
  +---+
  |   |
  O   |
  |   |
      |
      |
=========""",
    r"""
  +---+
  |   |
  O   |
 /|   |
      |
      |
=========""",
    r"""
  +---+
  |   |
  O   |
 /|\  |
      |
      |
=========""",
    r"""
  +---+
  |   |
  O   |
 /|\  |
 /    |
      |
=========""",
    r"""
  +---+
  |   |
  O   |
 /|\  |
 / \  |
      |
=========""",
]


def word_pool():
    """Endlessly yield (category, word) pairs, using every word once before repeating."""
    pairs = [(cat, word) for cat, words in WORDS.items() for word in words]
    while True:
        random.shuffle(pairs)
        yield from pairs


def masked(word, guessed):
    """Return the word with unguessed letters shown as underscores."""
    return " ".join(ch if ch in guessed else "_" for ch in word)


def ask_for_guess(guessed):
    """Prompt until the player enters something valid; return it in lowercase."""
    while True:
        guess = input("Guess a letter (or the whole word): ").strip().lower()
        if not (guess.isascii() and guess.isalpha()):
            print("  Please enter letters only.\n")
        elif len(guess) == 1 and guess in guessed:
            print(f"  You already guessed '{guess}'. Try another.\n")
        else:
            return guess


def play_round(category, word):
    """Play one game. Return True if the player wins, False otherwise."""
    guessed = set()  # every letter the player has tried
    misses = 0

    print(f"\nCategory: {category}  |  {len(word)} letters")

    while misses < MAX_MISSES and not set(word) <= guessed:
        wrong = sorted(guessed - set(word))
        print(STAGES[misses])
        print(f"\n  Word:   {masked(word, guessed)}")
        print(f"  Missed: {' '.join(wrong) if wrong else '-'}")
        print(f"  Lives:  {MAX_MISSES - misses}\n")

        guess = ask_for_guess(guessed)

        if len(guess) > 1:  # player tried the whole word
            if guess == word:
                guessed.update(word)
            else:
                misses += 1
                print(f"\n  '{guess}' is not the word. You lose a life!")
        elif guess in word:
            guessed.add(guess)
            print(f"\n  Yes! '{guess}' is in the word.")
        else:
            guessed.add(guess)
            misses += 1
            print(f"\n  Sorry, there's no '{guess}'.")

    won = set(word) <= guessed
    print(STAGES[misses])
    print(f"\n  The word was: {word.upper()}")
    print("  You win!" if won else "  You lose!")
    return won


def main():
    print("=" * 36)
    print("            H A N G M A N")
    print("=" * 36)
    print("Guess the secret word one letter at a time.")
    print(f"You can make {MAX_MISSES} mistakes before the hangman is complete.")
    print("Tip: you can also guess the whole word, but a wrong guess costs a life.")

    wins = losses = 0
    for category, word in word_pool():
        if play_round(category, word):
            wins += 1
        else:
            losses += 1
        print(f"\n  Score: {wins} won, {losses} lost")

        if input("\nPlay again? (y/n): ").strip().lower() not in ("y", "yes"):
            break

    print("\nThanks for playing!")


if __name__ == "__main__":
    try:
        main()
    except (KeyboardInterrupt, EOFError):
        print("\n\nGame ended. Goodbye!")
