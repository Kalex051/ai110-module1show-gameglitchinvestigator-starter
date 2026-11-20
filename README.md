# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [ ] Describe the game's purpose.
      This is a guessing game designed to have a player guess a number selected by the computer. The computer picks a secret number in a range set by the difficulty (Easy, Normal, Hard). The player types a guess and the game directs you to go higher or lower. The player wins by guessing the number before running out of attempts, and the score is based on how quickly the correct answer is given. 
- [ ] Detail which bugs you found.
      The new game button does not reset the game.
      Changing the difficulty does not restart the game.
      The game does not adjust the correct guess to the selected range. It always ranges from 1-100.
      The hints "Go Lower!" / "Go Higher!" are backwards. When a higher number is guessed, the response is "Go Higher!"
      The secret turns into text on even attempts.
      The hints come out wrong.
      The attempts are off by 1. The counter starts at 1 instead of 0.
      An invalid guess (not a numerical input) uses an attempt
      The Normal range is (1-100) and the Hard range is (1-50)
      
- [ ] Explain what fixes you applied.
      In 'check_guess', a guess above the secret now returns "Go LOWER!" and a guess below returns "Go HIGHER!". The outcome labels 'Too High'/'Too Low' were already correct.
      I deleted the block in app.py that converted the secret with str(). The secret now stays a number, so guesses are compared as numbers.
      I removed try/except fallback in 'check_guess' that compared text.
      I added a 'start_new_game()' function that picks a secret from the current difficulty's range and resets attempts, score, status, and history. The 'New Game' button calls it.
      The app now remembers which difficulty the current game started on. If you pick a different one, it calls 'start_new_game()' automatically.
      Attempts now start at 0. The 'Attempts Left' box is drawn in a placeholder and refreshed after each guess, so it is never a step behind.
      A win earns '100-10 * attempt_number' points, with a minimum of 10. Every wrong guess now costs 5 points, with no more bonus on even attempts.
      The info box range now uses the real 'low' and 'high' for the chosen difficulty.
      For invalid guesses, the attempt counter only goes up after the input passes 'parse_guess', o typing "abc" no longer costs an attempt.
      In the difficulty settings, 'Hard' is now 1-200, and the attempt limits are Easy: 8, Normal: 6, and Hard: 5.
      'parse_guess' strips spaces from the input and only catches the specific errors a bad number can cause instead of every exception. 

## 📸 Demo Walkthrough

Describe your fixed game in numbered steps so a reader can follow along without watching a video:

1. Select a difficulty. Easy(1-20) 8 attempts, Normal(1-100) 6 attempts, and Hard (1-200) 5 attempts. The box under "Make a Guess" will show the range for the difficulty selected and how many attempts the player has left.
2. Type a whole number into the text box and click "Submit Guess". If the player inputs a guess that is not a number (like "abc"), the game will show an error and will not use any attempts.
3. Read the Hint. If your guess is too high, it says "Go LOWER!". If your guess is too low, it says "Go HIGHER!". Each wrong guess is 5 points, and the attempts left counts down by 1.
4. Keep guessing until you find the secret number. When you do, balloons appear and you see your final score. Winning sooner earns more points, with a minimum of 10.
5. If you run out of attempts first, the game with return "Out of attempts!" along with the secret number.
6. Click "New Game" to play again. The game will pick a new secret number for the current difficulty that the player has set and resets your score, attempts, and history. Changing the difficulty in the sidebar also starts a fresh game.
7. Optional: open "Developer Debug Info" to see the secret number, your attempts, score, and guess history, which is helpful for checking that the game behaves correctly.


**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
# Paste your pytest output here, e.g.:
# pytest tests/
# ========================= X passed in 0.XXs =========================
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
