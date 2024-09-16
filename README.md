# Minesweeper
import java.util.Random;
import java.util.Scanner;

public class Minesweeper {

    public static void main(String[] args) {
        int rows = 9;
        int cols = 9;       
        int mines = 10;     // mines
        
        // Initializing
        char[][] grid = new char[rows][cols];
        boolean[][] opened = new boolean[rows][cols];
        
        fixMines(grid, mines);
        nearMines(grid);
        display(grid, opened);
        
        ms(grid, opened, rows * cols, mines);
    }

    // Fixing
    static void fixMines(char[][] grid, int mineCount) {
        Random rand = new Random();
        int fixed = 0;
        while (fixed < mineCount) {
            int row = rand.nextInt(grid.length);
            int col = rand.nextInt(grid[0].length);
            if (grid[row][col] != '*') {
                grid[row][col] = '*';   //fixing
                fixed++;
            }
        }
    }

    // adjacent mines
    static void nearMines(char[][] grid) {
        int[] dx = {-1, -1, -1, 0, 0, 1, 1, 1};   // mapping
        int[] dy = {-1, 0, 1, -1, 1, -1, 0, 1};
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                if (grid[row][col] == '*') continue;  
                int mineCount = 0;
                for (int i = 0; i < 8; i++) {
                    int newRow = row + dx[i];
                    int newCol = col + dy[i];
                    if (validation(newRow, newCol, grid) && grid[newRow][newCol] == '*') {
                        mineCount++;
                    }
                }
                if (mineCount > 0) {
                    grid[row][col] = (char) (mineCount + '0'); 
                } else {
                    grid[row][col] = ' '; 
                }
            }
        }
    }

    static boolean validation(int row, int col, char[][] grid) {
        return row >= 0 && row < grid.length && col >= 0 && col < grid[0].length;
   }

    // Displaying
    static void display(char[][] grid, boolean[][] opened) {
        System.out.println("Current Board:");
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                if (opened[row][col]) {
                    System.out.print(grid[row][col] + " ");
                } else {
                    System.out.print("- ");  // Hide unopened cells
                }
            }
            System.out.println();
        }
    }


        // Game 
    static void ms(char[][] grid, boolean[][] opened, int totalCells, int totalMines) {
        Scanner sc = new Scanner(System.in);
        int openCells = 0;
        boolean gameOver = false;
        
        while (!gameOver && openCells < totalCells - totalMines) {
            System.out.println("Enter row and column to reveal (index 0-8):");
            int row = sc.nextInt();
            int col = sc.nextInt();
            
            if (openCell(grid, opened, row, col)) {
                System.out.println("Game Over! You hit a mine.");
                gameOver = true;
            } else {
                openCells = copenCells(opened);
                display(grid, opened);
            }
        }
        
        if (openCells == totalCells - totalMines) {
            System.out.println("Congratulations! You've opened all safe cells!");
        }
    }


    // Game 
    static void ms(char[][] grid, boolean[][] opened, int totalCells, int totalMines) {
        Scanner sc = new Scanner(System.in);
        int openCells = 0;
        boolean gameOver = false;
        
        while (!gameOver && openCells < totalCells - totalMines) {
            System.out.println("Enter row and column to reveal (index 0-8):");
            int row = sc.nextInt();
            int col = sc.nextInt();
            
            if (openCell(grid, opened, row, col)) {
                System.out.println("Game Over! You hit a mine.");
                gameOver = true;
            } else {
                openCells = copenCells(opened);
                display(grid, opened);
            }
        }
        
        if (openCells == totalCells - totalMines) {
            System.out.println("Congratulations! You Won!");
        }
    }    


    // open cell
    static boolean openCell(char[][] grid, boolean[][] opened, int row, int col) {
        if (!validation(row, col, grid) || opened[row][col]) {
            return false;  // Invalid or already opened
        }
        
        opened[row][col] = true;
        if (grid[row][col] == '*') {
            return true;   // Hit a mine
        } else if (grid[row][col] == ' ') {
            // Recursively reveal adjacent cells
            for (int i = -1; i <= 1; i++) {
                for (int j = -1; j <= 1; j++) {
                    openCell(grid, opened, row + i, col + j);
                }
            }
        }
        return false;
    }


    // count open cells
    static int copenCells(boolean[][] opened) {
        int count = 0;
        for (int row = 0; row < opened.length; row++) {
            for (int col = 0; col < opened[0].length; col++) {
                if (opened[row][col]) {
                    count++;
                }
            }
        }
        return count;
    }
}
