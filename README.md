# Word-Game
![Screenshot (122)](https://github.com/user-attachments/assets/d62b2579-2c96-4fdc-8ea2-90bf6b22dea5)

#include <iostream>
#include <vector>
#include <string>

#include <ctime>
#include <cstdlib>

// Base Game class
class Game {
public:
    Game(std::string name) : gameName(name), attempts(0) {}
    
    // Virtual function to be overridden by the derived class
    virtual void play() = 0;
    
    // A function to display the game name
    void displayGameName() {
        std::cout << "Welcome to the " << gameName << "!" << std::endl;
    }

    // Function to count number of attempts
    void incrementAttempts() {
        attempts++;
    }

protected:
    std::string gameName;
    int attempts;
};

// Derived LetterGame class
class LetterGame : public Game {
public:
    LetterGame(std::string name, std::vector<std::string> words)
        : Game(name), wordList(words), selectedWord(""), guessedWord(""), maxAttempts(6) {
        srand(time(0)); // Seed for random word selection
    }

    // Function to start the game
    void play() override {
        // Select a random word from the word list
        selectedWord = wordList[rand() % wordList.size()];
        guessedWord = std::string(selectedWord.length(), '_');
        int remainingAttempts = maxAttempts;
        
        std::cout << "Let's play! The word has " << selectedWord.length() << " letters." << std::endl;
        
        // Game loop
        while (remainingAttempts > 0) {
            std::cout << "Current word: " << guessedWord << std::endl;
            std::cout << "You have " << remainingAttempts << " attempts left." << std::endl;
            std::cout << "Enter a letter to guess: ";
            char guess;
            std::cin >> guess;
            std::cin.ignore(); // Clear the input buffer

            // Check if the letter has already been guessed
            if (guessedLetters.find(guess) != std::string::npos) {
                std::cout << "You already guessed this letter. Try again!" << std::endl;
                continue;
            }

            guessedLetters += guess;
            bool found = false;

            // Check if the letter exists in the selected word
            for (int i = 0; i < selectedWord.length(); i++) {
                if (selectedWord[i] == guess) {
                    guessedWord[i] = guess;
                    found = true;
                }
            }

            // If letter not found, decrement attempts
            if (!found) {
                remainingAttempts--;
                std::cout << "Incorrect guess!" << std::endl;
            }

            // Check if the player has guessed the word correctly
            if (guessedWord == selectedWord) {
                std::cout << "Congratulations! You've guessed the word: " << selectedWord << std::endl;
                std::cout << "You won in " << (maxAttempts - remainingAttempts) << " attempts!" << std::endl;
                return;
            }
        }

        std::cout << "Game Over! You've used all attempts. The word was: " << selectedWord << std::endl;
    }

private:
    std::vector<std::string> wordList;
    std::string selectedWord;
    std::string guessedWord;
    std::string guessedLetters; // To keep track of guessed letters
    const int maxAttempts;
};

// Main function
int main() {
    // Define a list of words for the game
    std::vector<std::string> words = {"programming", "inheritance", "polymorphism", "encapsulation", "abstraction"};

    // Create an instance of the LetterGame class
    LetterGame letterGame("Letter Guessing Game", words);

    // Display the game name
    letterGame.displayGameName();

    // Start the game
    letterGame.play();

    return 0;
}

