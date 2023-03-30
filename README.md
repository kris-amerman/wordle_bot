## High-level Approach
I started out by defining the structure of the program. The problem involves several key components: the server, the client, a guessing strategy, and a socket connection. To facilitate the protocol, I defined a "Controller" class that would handle communication between the server and the client. It runs through a series of startup procedures to establish a connection via socket. If the user opts for TLS, main() will call the controller's TLS protocol. It then executes a Wordle guessing lifecycle, which is broken up into 4 tasks: setup, start, retry, and bye. The setup involves sending a "hello" message to the server and storing the game ID. The start procedure sends an initial guess to activate the guessing loop. The guessing loop runs as long as the server responds with "retry". Within this loop, the retry procedure makes a call to the Wordle guessing strategy. Once the secret word has been identified, the bye procedure will print the resulting flag and exit. 

## Guessing Strategy and High-level Approach cont. 
The Wordle guessing strategy is defined in a class called "WordleFilter". This class maintains a set of possible words to guess. It filters out words based on the data it receives about the current guess. In our case, this data comes from the server, which provides information about the "marks" of a guess. The guess and its associated marks are processed by the "update_words" method. This method proceeds as follows:
    1. Match the letters of the given word to its associated marks (0, 1, or 2) to generate 3 lists of letters.
    2. At the same time, build a list of regex characters to match the exact marks pattern. 
    3. Remove the given word from the list of possible guesses to avoid guessing it again.
    4. Filter out words that don't match the exact regex pattern.
    5. Eliminate the intersection of missed marks with partial and exact marks to generate a list of letters that don't appear in the secret word.
    6. Filter out words that contain letters that don't appear in the secret word.

The next guess is defined as the first word in the updated wordlist. This could be optimized, but it was sufficient enough for this purpose.  
