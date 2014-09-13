

package Hangman;

/**
 *
 * @author kimkoss
 * Name: Kim Koss
 * Class: CSIS2450 - 003
 * Assignment: A01 - Hangman
 * Date: Sept 13, 2014
 */
import java.util.*;

/**
 * Game Hangman
 * @author kimkoss
 */
public class Hangman
{
    private final List<String> list = new ArrayList<>();
    private final Scanner scan = new Scanner(System.in);
    private String loadWord;
    private String guessLetter;
    private char[] charWordArray;
    private int lives;
    private int wordSize;
    private int end;
    private boolean inWord;
    
    //Hardcoded words

    /**
     * Constructor containing hard coded state names.
     * 
     */
        public Hangman()
    {
        list.add("Mississippi");
        list.add("Hawaii");
        list.add("Florida");
        list.add("Colorado");
        list.add("Oregon");
        list.add("Maine");
        list.add("Texas");
        list.add("Alaska");
        list.add("New York");
        list.add("California");
        
        this.loadWord = null;
        this.guessLetter = null;
        this.lives = 6;
        this.wordSize = 0;
        this.end = 1;
        this.charWordArray = null;
        this.inWord = false;
    }
    
    //Welcome

    /**
     * Welcome statement for beginning of game
     */
        public void welcome()
    {
        System.out.println();
        System.out.println("Welcome to Hangman");
        System.out.println();
    }
    
    //Game loop

    /**
     * Performs one iteration of game, then request to repeat
     */
        public void gameLoop()
    {
        int enterAnswer;
        
        do
        {
            lives = 6;
            loadWord();
            guessLoop();
            System.out.println();
            System.out.print("Do you want to continue?(1 = Yes/ 0 = No): ");
            if(scan.hasNextInt())
            {
                scan.nextInt();
                System.out.println();
                System.out.println();
            }
            else
                {
                    System.out.println();
                    System.out.println();
                    System.out.println("Goodbye");
                    end = 0;
                }
         
        }while(end == 1);
    }
    
    //Guess loop

    /**
     * Performs an iteration for one guess
     */
        public void guessLoop()
    {
        do
        {
            currentLives();
            graphic();
            guessLetter();
            isLetterInWord();
            displayLetterInWord();
            winLoose();
            
        }while(lives > 0 && !wordRevealed());
    }
     
    //Random word chosen from hard coded

    /**
     * Randomly chooses a word from hard coded list.
     * Size of word calculated.
     * String word converted to character array.
     * Character array loaded with characters underscore and 
     * white space if needed.
     */
        public void loadWord()
    {
        String tempHold = loadWord;
        int random = (int)(Math.random() * list.size());
        
        do
        {
            loadWord = list.get(random);
        }while(tempHold == loadWord);
       
        wordSize = loadWord.length();
        charWordArray = loadWord.toCharArray(); 
        
        for(int i = 0; i < wordSize; i++)
        { 
            if(charWordArray[i] == ' ')
            {
                charWordArray[i] = ' ';
            }
            else
                 charWordArray[i] = '_';
        }
    }
    
    //Current lives

    /**
     * Displays number of lives
     */
        public void currentLives()
    {
        System.out.println("The current number of lives is: " + lives);
        System.out.println();
    }
    
    //Graphics

    /**
     * Graphic of Hangman
     */
        public void graphic()
    {
        System.out.println("---|");
        switch(lives)
        {
            case 0:
                System.out.println(" 0 |");
                System.out.println("/|\\|");
                System.out.println("/ \\| ");
                System.out.println("   |");
                System.out.println("___|");
                break;
            case 1:
                System.out.println(" 0 |");
                System.out.println("/|\\|");
                System.out.println("/  |");
                System.out.println("   |");
                System.out.println("___|");
                break;
            case 2:
                System.out.println(" 0 |");
                System.out.println("/|\\|");
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("___|");
                break;
            case 3:
                System.out.println(" 0 |");
                System.out.println("/| |");
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("___|");
                break;
            case 4:
                System.out.println(" 0 |");
                System.out.println("/  |");
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("___|");
                break;
            case 5:
                System.out.println(" 0 |");
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("___|");
                break;
            case 6:
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("   |");
                System.out.println("___|");
                break;
        }
        System.out.println();
    }
            
    //Display word

    /**
     *  Display letters guessed correctly in word.
     */
        public void displayWord()
    {       
        if(!loadWord.isEmpty())
        {
           for(int i = 0; i < loadWord.length(); i++)
           {
               System.out.print(loadWord.charAt(i) + " ");
           }
        }
        System.out.println();
        System.out.println();
    }
    
    //Guess letter

    /**
     * Scan for a letter to be guessed
     */
        public void guessLetter()
    {    
        System.out.print("Guess a letter: ");
        guessLetter = scan.next();    
      
        System.out.println();
    }
   
    //Is chosen letter in word
    //If so character is stored in array

    /**
     * Performs comparison of loaded word with guessed letter.
     * If letter is contained in loaded word then continue, 
     * if not, then one life is lost. 
     * Lives decrement by one.
     */
        public void isLetterInWord()
    {
        inWord = false; 
        
        for(int i = 0; i < loadWord.length(); i++)
        {
            if(loadWord.charAt(i) == guessLetter.charAt(0))          
            {
                charWordArray[i] = guessLetter.charAt(0);  
                inWord = true;
            }
            else if(loadWord.charAt(i) == guessLetter.toUpperCase().charAt(0))
            {
                charWordArray[i] = guessLetter.toUpperCase().charAt(0);  
                inWord = true;
            }
        }
        
        if(!inWord)
        {
            lives--;
        }
      
        System.out.println(); 
    }
    
    //Display letter in word

    /**
     * Displays the letter(s) in loaded word
     * 
     */
        public void displayLetterInWord()
    {
        
        //System.out.print("Word:  ");
        for(int i = 0; i < charWordArray.length; i++)
        {
            System.out.print(charWordArray[i]);
            System.out.print(" ");
        }
        System.out.println();
        System.out.println();
    }
    
    //Is the word revealed

    /**
     * Compares loaded word with a String representation of 
     * the character array containing the guessed letters.
     * @return
     */
        public boolean wordRevealed()
    {
        String charWord = new String(charWordArray);

        if(loadWord.compareTo(charWord) == 0)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
    
    //display results

    /**
     * Display whether there is a win or loose.
     */
        public void winLoose()
    {   
        if(lives > 0 && wordRevealed() )
        {
            System.out.println("You win");
        }
        else if( lives == 0 )
        {
            System.out.println("You loose");
            System.out.println();
            graphic();
            displayWord();
        }
        System.out.println();
        System.out.println();
    }
                
    /**
     * Main function.
     * @param args
     */
    public static void main(String[] args)
    {
       Hangman hangman = new Hangman();
       hangman.welcome();
       hangman.gameLoop();
    }    
}
