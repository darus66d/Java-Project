# Java-Project
#Tic Tac Toe Game


import java.util.Random; // Importing the Random class from java.util
import java.util.Scanner; // Importing the Scanner class from java.util

public class TicTacToe3 { // Class declaration
    public static void main(String[] args) { // Main method
        Scanner scanner = new Scanner(System.in); // Creating a Scanner object for input
        boolean exit = false; // Variable to control the main loop
        int player1score = 0, player2score = 0; // Variables to store player scores

        // Main loop for the menu



        
        while (!exit) {
            System.out.println("Tic Tac Toe Menu:"); // Printing menu options
            System.out.println("1. Start Player VS Player Match");
            System.out.println("2. Start Player VS Computer Match");
            System.out.println("3. Exit");
            System.out.print("Enter your choice: ");

            int menuChoice = scanner.nextInt(); // Reading user's choice

            switch (menuChoice) { // Switch statement for menu options
                case 1:
                    System.out.println("How many times you want to play");
                    int value = scanner.nextInt(); // Reading the number of games to play
                    System.out.println("Enter first player name");
                    String player1 = scanner.next(); // Reading the name of the first player
                    System.out.println("Enter second player name");
                    String player2 = scanner.next(); // Reading the name of the second player
                    playGame(scanner, value, player1, player2, player1score, player2score); // Calling the playGame method
                    break;
                case 2:
                    playComputer(scanner); // Calling the playComputer method
                    break;
                case 3:
                    exit = true; // Setting exit to true to end the loop
                    break;
                default:
                    System.out.println("Invalid choice. Please enter 1 or 2."); // Displayed for invalid choices
            }
        }

        System.out.println("Goodbye!"); // Farewell message
        scanner.close(); // Closing the Scanner object
    }

    // Method to play a Player VS Player match
    public static void playGame(Scanner scanner, int value, String player1, String player2, int p1s, int p2s) {
        char[][] board = { // Initializing the game board
                {' ', ' ', ' '},
                {' ', ' ', ' '},
                {' ', ' ', ' '}
        };
        int currentPlayer = 1; // 1 for Player 1, 2 for Player 2
        int totalMoves = 0;

        printBoard(board); // Printing the initial game board

        // Game loop
        while (true) {
            int row, col;

            // Displaying the current player's turn
            if (currentPlayer == 1) {
                System.out.println(player1 + "'s turn (X)");
            } else {
                System.out.println(player2 + "'s turn (O)");
            }

            System.out.print("Enter row (0, 1, 2): ");
            row = scanner.nextInt(); // Reading the row input
            System.out.print("Enter column (0, 1, 2): ");
            col = scanner.nextInt(); // Reading the column input

            // Checking if the move is valid
            if (isValidMove(board, row, col)) {
                char symbol = (currentPlayer == 1) ? 'X' : 'O'; // Determining the player's symbol
                board[row][col] = symbol; // Updating the game board
                totalMoves++; // Incrementing the total number of moves
                printBoard(board); // Printing the updated game board

                // Checking for a win
                if (checkWin(board, symbol)) {
                    if (currentPlayer == 1) {
                        System.out.println("Player " + player1 + " wins!");
                        p1s++;
                    } else {
                        System.out.println(player2 + " wins!");
                        p2s++;
                    }
                    value--;
                    if (value != 0) playGame(scanner, value, player1, player2, p1s, p2s);
                    else {
                        System.out.println(player1 + " has won : " + p1s + " matches");
                        System.out.println(player2 + " has won : " + p2s + " matches");
                        if (p1s == p2s) {
                            System.out.println("Series draw");
                        } else if (p1s > p2s) {
                            System.out.println(player1 + " has won the series");
                        } else {
                            System.out.println(player2 + " has won the series");
                        }
                    }
                    break;
                } else if (totalMoves == 9) {
                    System.out.println("It's a draw!");
                    break;
                }

                currentPlayer = (currentPlayer == 1) ? 2 : 1; // Switching players
            } else {
                System.out.println("Invalid move. Try again.");
            }
        }
    }

    // Method to play a Player VS Computer match
    public static void playComputer(Scanner scanner) {
        System.out.println("Enter player name");
        String player1 = scanner.next();
        char[][] board = {
                {' ', ' ', ' '},
                {' ', ' ', ' '},
                {' ', ' ', ' '}
        };
        int currentPlayer = 1; // 1 for Player 1, 2 for Computer
        int totalMoves = 0;

        printBoard(board);

        // Game loop
        while (true) {
            int row, col;
            if (currentPlayer == 1) {
                System.out.println(player1 + "'s turn (X)");
            } else {
                System.out.println("Computer's turn (O)");
            }

            Random random = new Random();

            // Player's turn
            if (currentPlayer == 1) {
                System.out.print("Enter row (0, 1, 2): ");
                row = scanner.nextInt();
                System.out.print("Enter column (0, 1, 2): ");
                col = scanner.nextInt();
            } else {
                // Computer's turn
                row = random.nextInt(3);
                col = random.nextInt(3);

                // Ensuring the computer makes a valid move
                while ((!isValidMove(board, row, col))) {
                    row = random.nextInt(3);
                    col = random.nextInt(3);
                }
            }

            // Checking if the move is valid
            if (isValidMove(board, row, col)) {
                char symbol = (currentPlayer == 1) ? 'X' : 'O';
                board[row][col] = symbol;
                totalMoves++;
                printBoard(board);

                // Checking for a win
                if (checkWin(board, symbol)) {
                    if (currentPlayer == 1) System.out.println("Player " + player1 + " wins!");
                    else System.out.println("Computer" + " wins!");
                    break;
                } else if (totalMoves == 9) {
                    System.out.println("It's a draw!");
                    break;
                }

                currentPlayer = (currentPlayer == 1) ? 2 : 1; // Switching players
            } else {
                System.out.println("Invalid move. Try again.");
            }
        }
    }

    // Method to check if a move is valid
    public static boolean isValidMove(char[][] board, int row, int col) {
        return row >= 0 && row < 3 && col >= 0 && col < 3 && board[row][col] == ' ';
    }

    
    // Method to check if a player has won
    public static boolean checkWin(char[][] board, char symbol) {
        // Check rows, columns, and diagonals for a win
        for (int i = 0; i < 3; i++) {
            if (board[i][0] == symbol && board[i][1] == symbol && board[i][2] == symbol) {
                return true; // Check rows
            }
            if (board[0][i] == symbol && board[1][i] == symbol && board[2][i] == symbol) {
                return true; // Check columns
            }
        }

        if (board[0][0] == symbol && board[1][1] == symbol && board[2][2] == symbol) {
            return true; // Check diagonal from top-left to bottom-right
        }

        if (board[0][2] == symbol && board[1][1] == symbol && board[2][0] == symbol) {
            return true; // Check diagonal from top-right to bottom-left
        }

        return false;
    }


    // Method to print the game board
    public static void printBoard(char[][] board) {
        System.out.println("  0 1 2");
        for (int i = 0; i < 3; i++) {
            System.out.print(i + " ");
            for (int j = 0; j < 3; j++) {
                System.out.print(board[i][j] + " ");
            }
            System.out.println();
        }
    }
}

