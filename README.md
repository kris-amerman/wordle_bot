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

## Challenges
I have no background in networks, socket semantics, Linux, or systems, so a lot of this is new to me. I also haven't touched Python since before OOD, so I had to pick it up on the job. I spent a lot of time researching TCP, IP, and TLS/SSL. I wasn't sure what the best practice was for socket control flow (i.e. picking between TLS and no TLS). I haven't really worked with context managers before, so I tried to split everything up into methods to avoid a massive conditional tree and any code repetition. I didn't have too many issues with the guessing strategy, aside from fixing a few edge cases.    

## Testing
I didn't write any formal unit tests, although I'd like to if I had the time. Most of my testing was done manually via the terminal. I tested blocks of code as I developed them. Sometimes I would test lines in a separate file to isolate their behavior. This allowed me to test my socket connection, communication, control flow, and word-filtering independently. # wordle_bot
