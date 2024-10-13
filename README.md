import random
import enchant

# Initialize English dictionary
dictionary = enchant.Dict("en_US")

# Generate a random valid word
def get_random_word_from_dictionary():
    words = [word.strip() for word in open("/usr/share/dict/words").readlines()]
    valid_words = [word for word in words if dictionary.check(word.lower())]
    return random.choice(valid_words)

# AI chooses a word randomly from the dictionary
ai_word = get_random_word_from_dictionary()

def ask_yes_no_question(question, ai_word):
    question = question.lower()
    if 'is it' in question:
        # Check if the question is directly asking about the AI's word
        guessed_word = question.split('is it')[-1].strip()
        if guessed_word == ai_word:
            return "Yes"
        else:
            return "No"
    return "I don't know. Ask if it's something specific!"

print("Welcome to the Yes or No guessing game!")
print("You have 3 guesses to figure out what word I am thinking of.")
print("You can ask yes or no questions like 'Is it a [word]?'")

# Main game loop
for guess in range(3):
    question = input(f"Guess {guess + 1}: Ask your question: ")
    response = ask_yes_no_question(question, ai_word)
    print("AI says:", response)
    
    # Check if player guessed the word
    if response == "Yes":
        print("Congratulations! You guessed the word!")
        break
else:
    print(f"Sorry, you're out of guesses. The word was: {ai_word}")
