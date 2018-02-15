# 4InARow
//Basic 4 in a row java game
package com.company;

import java.util.Random;
import java.util.Scanner;
public class ConnectFour{

    public static void main(String[] args) {
        char[][] connectFourBoard = new char[6][7];
        Scanner keyboard = new Scanner(System.in);
        String player;
        System.out.println("\n**FOUR IN A ROW GAME** \n\nChoose the number of players! " +
                "\n\n1 player(Enter 1) \n2 players(Enter 2)");
        board(connectFourBoard);
        player = keyboard.nextLine();
        switch (player) {
            case "1":
                System.out.println("JK, there's no 1 player option yet. " +
                        "It's more fun playing with a friend anyway! :)");
                board(connectFourBoard);
                playGame(connectFourBoard);
                break;

            case "2":
                board(connectFourBoard);
                playGame(connectFourBoard);
                break;

            default:
                    System.out.println("That wasn't an option, so let's just pretend you have a friend :)");
                board(connectFourBoard);
                playGame(connectFourBoard);
                    break;

        }
    }

    public static void board(char[][] connectFourBoard){
        System.out.println("---------------");
        for(int i = 0; i < connectFourBoard.length; i++){
            for(int j = 0; j < connectFourBoard[i].length; j++){
                System.out.print("|" + (connectFourBoard[i][j] != '\u0000'? connectFourBoard[i][j] : " "));
            }
            System.out.println("|");
        }
        for(int i = 0; i < connectFourBoard.length - 1; i++){
            System.out.print("---");
        }
        System.out.println("");
    }

    public static void playGame(char[][] connectFourBoard){
        Scanner keyboard = new Scanner(System.in);
        boolean gameOver = false;
        boolean playersTurn = true;
        char gamePieceColor;
        int column = 0;
        String answer;
        while(!gameOver){
            if(playersTurn){
                System.out.print("Drop a red game piece at column 0-6");
                gamePieceColor = 'R';
            }else{
                System.out.print("Drop a yellow game piece at column 0-6");
                gamePieceColor = 'Y';

            }
            column = keyboard.nextInt();
            while(column < 0 || column > 6){
                if(gamePieceColor == 'Y'){
                    System.out.println("Invalid column, Yellow. Pick a column between 0 and 6");
                }else{
                    System.out.println("Invalid column, Red. Pick a column between 0 and 6");
                }
                column = keyboard.nextInt();
            }
            playersTurn = !playersTurn;
            if(dropPiece(connectFourBoard, column, gamePieceColor)){
                playersTurn = !playersTurn;
            }else{
                board(connectFourBoard);
                if(gameStatus(connectFourBoard, column, gamePieceColor)) {
                    gameOver = true;
                    if (gamePieceColor == 'Y') {
                        System.out.println("YELLOW won! Game over!");
                        System.out.println("Play again? Type yes to play again, or type anything else to exit");
                        answer = keyboard.next();
                        System.out.println("You typed: " + answer);
                        if("yes".equals(answer)){
                            board(connectFourBoard);
                            playGame(connectFourBoard);
                        }
                    } else {
                        System.out.println("Red won! Game over!");
                        System.out.println("Play again? Type yes to play again, or type anything else to exit");
                        answer = keyboard.next();
                        System.out.println("You typed: " + answer);
                        if("yes".equals(answer)){
                            board(connectFourBoard);
                            playGame(connectFourBoard);
                        }
                    }
                }
                if(checkTie(connectFourBoard)){
                    gameOver = true;
                    System.out.print("Tie!");
                    System.out.println("Play again? Type yes to play again, or type anything else to exit");
                    answer = keyboard.next();
                    System.out.println("You typed: " + answer);
                    if("yes".equals(answer)){
                        board(connectFourBoard);
                        playGame(connectFourBoard);
                    }
                }
            }
        }
        System.exit(0);
    }

    public static boolean gameStatus(char[][] connectFourBoard, int column, char gamePieceColor) {
        int rowPosition = getHighestPieceInColumn(connectFourBoard, column);
        if(checkColumn(connectFourBoard, column, gamePieceColor, rowPosition)){
            return true;
        }
        if(checkRow(connectFourBoard, column, gamePieceColor, rowPosition)){
            return true;
        }
        if(checkDiagonalRight(connectFourBoard, column, gamePieceColor, rowPosition)){
            return true;
        }
        if(checkDiagonalLeft(connectFourBoard, column, gamePieceColor, rowPosition)){
            return true;
        }
        return false;
    }

    public static int getHighestPieceInColumn(char[][] board, int column) {
        for (int i = 0; i < board.length; i++) {
            if (board[i][column] != 0) {
                return i;
            }
        }
        return -1;
    }

    /*public static void clearBoard(char[][] board){
        char[][] connectFourBoard = new char[6][7];

        return;
    }*/

    /* public static void clearPieces(){

    }
     */

    public static boolean checkDiagonalLeft(char[][] connectFourBoard, int column, char gamePieceColor, int rowPosition){
        int gamePieceCounter = 1;
        for(int i = rowPosition + 1, j = column - 1; i < connectFourBoard.length && j >= 0; i++, j--){
            if(gamePieceColor == connectFourBoard[i][j]){
                gamePieceCounter++;
            }else{
                break;
            }
        }
        if(gamePieceCounter >= 4){
            return true;
        }
        for(int i = rowPosition - 1, j = column + 1; i >= 0 && j < connectFourBoard[0].length; i--, j++){
            if(gamePieceColor == connectFourBoard[i][j]){
                gamePieceCounter++;
            }else{
                break;
            }
            if(gamePieceColor >= 4){
                return true;
            }
        }
        return false;
    }

    public static boolean checkDiagonalRight(char[][] connectFourBoard, int column, char gamePieceColor, int rowPosition){
        int gamePieceCounter = 1;
        for(int i = rowPosition - 1, j = column - 1; i >= 0 && j >= 0; i--, j--){
            if(gamePieceColor == connectFourBoard[i][j]){
                gamePieceCounter++;
            }else{
                break;
            }
        }
        if(gamePieceCounter >= 4){
            return true;
        }
        for(int i = rowPosition + 1, j = column + 1; i < connectFourBoard.length && j < connectFourBoard[0].length; i++, j++){
            if(gamePieceColor == connectFourBoard[i][j]){
                gamePieceCounter++;
            }else{
                break;
            }
        }
        if(gamePieceCounter >= 4){
            return true;
        }
        return false;
    }

    public static boolean checkRow(char[][] connectFourBoard, int columnPosition, char pieceColor, int rowPosition){
        int pieceCounter = 1;
        for(int i = columnPosition - 1; i >= 0; i--){
            if(pieceColor == connectFourBoard[rowPosition][i]){
                pieceCounter++;
            }else{
                break;
            }
        }
        if(pieceCounter >= 4){
            return true;
        }
        for(int i = columnPosition + 1; i < connectFourBoard[0].length; i++){
            if(pieceColor == connectFourBoard[rowPosition][i]){
                pieceCounter++;
            }else{
                break;
            }
        }
        if(pieceCounter >= 4){
            return true;
        }
        return false;
    }

    public static boolean checkColumn(char[][] connectFourBoard, int column, char gamePieceColor, int rowPosition){
        int gamePieceCounter = 0;
        while (rowPosition < connectFourBoard.length) {
            if(gamePieceColor == connectFourBoard[rowPosition][column]){
                gamePieceCounter++;
                rowPosition++;
            }else{
                break;
            }
        }
        return gamePieceCounter >= 4;
    }

    public static boolean checkTie(char[][] connectFourBoard){
        for(int i = 0; i < connectFourBoard[0].length; i++){
            if(connectFourBoard[0][i] == 0){
                return false;
            }
        }
        return true;
    }

    public static boolean dropPiece(char[][] connectFourBoard, int column, char gamePieceColor){

        for(int i = connectFourBoard.length - 1; i >= 0; i--){
            if(connectFourBoard[i][column] == 0){
                connectFourBoard[i][column] = gamePieceColor;
                return false;
            }
        }
        if(gamePieceColor == 'Y'){
            System.out.println("Column full, YELLOW. Try again");
        }else{
            System.out.println("Column full, RED. Try again");
        }
        return true;
    }
}
