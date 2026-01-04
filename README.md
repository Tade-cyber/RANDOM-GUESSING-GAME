# RANDOM-GUESSING-GAME
#It is a random guessing game where we guess a number and between 1-100 and win the tournament of 5 rounds and the code is built using C- program
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <limits.h> // Used for flushing the input buffer

// --- Constants ---
#define MAX_GAMES 5
#define POINTS_HIGH 10
#define POINTS_LOW 5
#define ATTEMPTS_THRESHOLD 5
#define ATTEMPTS_MAX_LOW_POINTS 9

// Function to handle reading input and checking if it's a valid integer
int get_valid_guess(void) {
    int guess;
    int ret;

    // Loop until valid input is received
    while (1) {
        printf("Enter your guess: ");
        ret = scanf("%d", &guess);

        // Check if scanf successfully read an integer (ret should be 1)
        if (ret == 1) {
            // Read and discard any leftover characters until a newline or EOF is found
            int c;
            while ((c = getchar()) != '\n' && c != EOF);
            return guess;
        } else {
            // Clear the buffer if input failed (e.g., user entered text)
            int c;
            while ((c = getchar()) != '\n' && c != EOF); 
            printf("❌ Invalid input. Please enter a whole number.\n");
        }
    }
}

int main() {
    // 1. Setup the Random Number Generator (only once)
    srand(time(NULL)); 

    // --- Game State Variables ---
    int total_score = 0;
    int games_played = 0;

    printf("🎯 Welcome to the Scored Number Guessing Tournament! 🎯\n");
    printf("You will play a maximum of %d rounds.\n", MAX_GAMES);
    printf("Scoring: Guess in <= %d tries: %d points.\n", ATTEMPTS_THRESHOLD, POINTS_HIGH);
    printf("         Guess in 6 to %d tries: %d points.\n", ATTEMPTS_MAX_LOW_POINTS, POINTS_LOW);
    printf("----------------------------------------\n");

    // --- Main Tournament Loop ---
    while (games_played < MAX_GAMES) {
        games_played++;
        
        // 2. Setup for the new round
        int secret_number = (rand() % 100) + 1; // Number between 1 and 100
        int guess = 0;
        int attempts = 0;
        int current_round_score = 0;

        printf("\n--- ROUND %d of %d ---\n", games_played, MAX_GAMES);
        printf("I'm thinking of a new number between 1 and 100.\n");

        // 3. Inner Guessing Loop
        while (guess != secret_number) {
            guess = get_valid_guess(); // Use the robust input function
            attempts++;
            
            // Provide feedback
            if (guess < secret_number) {
                printf("Too low!\n");
            } else if (guess > secret_number) {
                printf("Too high!\n");
            }
        }

        // 4. Calculate and apply score after winning
        if (attempts <= ATTEMPTS_THRESHOLD) {
            current_round_score = POINTS_HIGH;
            printf("⭐ Excellent! You got it in %d tries.\n", attempts);
            printf("You earn the maximum %d points.\n", POINTS_HIGH);
            
        } else if (attempts <= ATTEMPTS_MAX_LOW_POINTS) {
            current_round_score = POINTS_LOW;
            printf("👍 Good job! You got it in %d tries.\n", attempts);
            printf("You earn %d points.\n", POINTS_LOW);
            
        } else {
            printf("😔 You took %d tries. No points awarded for this round (Requires < %d tries).\n", attempts, ATTEMPTS_MAX_LOW_POINTS + 1);
        }

        total_score += current_round_score;
        printf("Current Total Score: %d\n", total_score);
    }

    // --- Game Over Summary ---
    printf("\n========================================\n");
    printf("        🏆 TOURNAMENT OVER 🏆\n");
    printf("You have played all %d games.\n", MAX_GAMES);
    printf("Your FINAL SCORE is: %d points!\n", total_score);
    printf("========================================\n");

    return 0;
}
