Voice Commands for Gaming
Introduction:
In recent years, voice recognition technology has significantly evolved, opening new
possibilities for interactive and hands-free applications. This project focuses on
leveraging speech recognition to create an interactive voice-controlled gaming system.
By processing audio commands such as “shoot,” “jump,” “reload,” and classic game
inputs like “stone,” “paper,” and “scissors,” the system allows players to engage with
games through natural voice inputs rather than traditional controllers or keyboards.

The main objective is to develop a Python-based application that can handle audio files
containing spoken commands, recognize and validate these commands accurately, and
respond with appropriate game actions. This approach not only enhances the gaming
experience but also improves accessibility for users who may find conventional controls
challenging. The project demonstrates the integration of speech recognition with game
logic, enabling real-time, voice-driven gameplay.

Problem Statement:
Traditional gaming interfaces rely heavily on manual inputs through keyboards, mice, or
controllers, which can limit accessibility and reduce the natural interaction experience for
many users. There is a growing need for more intuitive, hands-free control mechanisms
that allow players to interact with games using voice commands. However, accurately
recognizing spoken commands from varied audio inputs and integrating them effectively
into game logic poses technical challenges, especially when handling diverse audio
formats and background noise.

This project aims to address these challenges by developing a robust voice-command
recognition system that can process audio files containing game commands, filter valid
instructions, and execute corresponding gameplay actions such as shooting, jumping, or
playing stone-paper-scissors. The goal is to improve user engagement and accessibility
by enabling seamless voice-driven gaming experiences.

Objectives:
● To develop a Python-based system capable of processing audio files with spoken
game commands.
● To support recognition of multiple commands like "shoot," "jump," "reload," and
play a simple stone-paper-scissors game.
● To handle audio files inside zipped folders with nested directory structures.
● To provide a clean output documenting the recognized commands and game
results.
● To demonstrate basic speech recognition integrated with game logic.
Methodology:
The project follows a systematic approach to develop a voice-command-based gaming
system. The key steps involved are:

Data Input and Extraction:
The system accepts a ZIP file containing nested folders with audio files in
formats such as WAV or MP3. The ZIP file is programmatically extracted, and
audio files are located recursively through all subdirectories.
Audio Format Handling:
Since the speech recognition library works best with WAV format, any MP
audio files encountered are converted to WAV using the pydub library, ensuring
compatibility and consistency.
Speech Recognition:
The extracted audio files are processed using the speech_recognition Python
library, which interfaces with Google's Speech Recognition API to transcribe the
spoken commands into text.
Command Filtering and Validation:
The transcribed text is cleaned and compared against a predefined list of valid
game commands: "shoot," "jump," "reload," "stone," "paper," and "scissors."
Commands outside this list are ignored to prevent invalid inputs.
Game Logic Execution:
○ For commands related to the shooting game ("shoot," "jump," "reload"),
the system acknowledges the command with a suitable response.
○ For stone-paper-scissors commands, the system randomly selects a
computer move and determines the winner based on the classic game
rules.
Output Generation:
Results from the command recognition and gameplay are logged into an output
text file (output.txt) including the audio file name, recognized command, game
moves, and outcomes.
Testing and Validation:
Multiple audio files are randomly selected for testing to ensure accuracy of
speech recognition and correctness of the game logic. The system is also tested
for robustness against unrecognized or irrelevant audio inputs.
Program Flow:
Step 1: Install necessary libraries
!pip install SpeechRecognition pydub

!apt-get install ffmpeg - y

import zipfile

import os

import random

import speech_recognition as sr

from pydub import AudioSegment

from google.colab import files

Step 2: Upload ZIP file
print("Upload your input.zip containing folders and audio files:")

uploaded = files.upload()

zip_path = list(uploaded.keys())[ 0 ]

extract_folder = "extracted_audio"

Step 3: Extract ZIP file
with zipfile.ZipFile(zip_path, 'r') as zip_ref:

zip_ref.extractall(extract_folder)
print(f"Extracted files to '{extract_folder}'")

Step 4: Function to get all audio files recursively
def get_all_audio_files(base_folder, extensions=('.wav', '.mp3')):

audio_files = []
for root, dirs, files in os.walk(base_folder):
for file in files:
if file.lower().endswith(extensions):
audio_files.append(os.path.join(root, file))
return audio_files
Step 5: Convert any mp3 files to wav for compatibility
audio_files = get_all_audio_files(extract_folder)

for file_path in audio_files:

if file_path.lower().endswith(".mp3"):
wav_path = file_path.rsplit('.', 1 )[ 0 ] + ".wav"
print(f"Converting {file_path} to {wav_path}")
sound = AudioSegment.from_mp3(file_path)
sound.export(wav_path, format="wav")
os.remove(file_path) # Remove original mp
Step 6: Refresh audio files list to only wav files now
audio_files = get_all_audio_files(extract_folder, extensions=('.wav',))

print(f"Found {len(audio_files)} WAV audio files after conversion.")

if len(audio_files) < 5 :

print("Warning: Less than 5 audio files found, will process all available.")
Step 7: Select 5 random audio files (or all if less than 5)
num_files = min( 5 , len(audio_files))

selected_files = random.sample(audio_files, num_files)

print(f"Selected {num_files} audio files:")

for f in selected_files:

print(f)
Step 8: Recognizer setup
recognizer = sr.Recognizer()

valid_commands = {

"shoot", "jump", "reload", "stone", "rock", "paper", "scissors", "scissor"
}

output_lines = []

Step 9: Process each selected audio file
for idx, audio_path in enumerate(selected_files, 1 ):

with sr.AudioFile(audio_path) as source:
audio = recognizer.record(source)
try:
spoken_text = recognizer.recognize_google(audio).lower()
print(f"[{idx}] Recognized text: {spoken_text}")
except Exception as e:
print(f"[{idx}] Could not recognize audio: {str(e)}")
spoken_text = None
output_lines.append(f"Audio file [{idx}]: {os.path.basename(audio_path)}")

if spoken_text in valid_commands:

# Handle stone-paper-scissors game commands
if spoken_text in {"stone", "rock", "paper", "scissors", "scissor"}:
if spoken_text == "stone":
player_move = "rock"
elif spoken_text == "scissor":
player_move = "scissors"
else:
player_move = spoken_text
moves = ["rock", "paper", "scissors"]
computer_move = random.choice(moves)
def decide_winner(player, comp):
if player == comp:
return "draw"
elif (player == "rock" and comp == "scissors") or \
(player == "scissors" and comp == "paper") or \
(player == "paper" and comp == "rock"):
return "player"
else:
return "computer"
winner = decide_winner(player_move, computer_move)
output_lines.append(f"Your move: {player_move}")
output_lines.append(f"Computer move: {computer_move}")
output_lines.append(f"Winner: {winner}")
Input Format:
● A ZIP file uploaded by the user.
● The ZIP file contains folders and subfolders with audio files inside.
● Audio files are in `.wav` or `.mp3` format (MP3 files are converted
automatically).
Output Format:
● A plain text file (`output.txt`) saved to disk.
● The file contains:
○ Audio file name processed.
○ Recognized command or error message.
○ Game moves and winner for stone-paper-scissors commands.
○ Messages for invalid or ignored commands.
else:
# Other valid commands (shoot, jump, reload)
output_lines.append(f"Command recognized: {spoken_text}")
else:
output_lines.append("Command not recognized or not a valid game command.
Ignored.")

output_lines.append("") # blank line for readability
Step 10: Write output to output.txt
with open("output.txt", "w") as f:

for line in output_lines:
f.write(line + "\n")
print("Output saved to output.txt")

Step 11 (optional): Download the output file
files.download("output.txt")

Output:
Testing and Validation:
● Tested with a variety of audio files in nested folder structures.
● Verified MP3 to WAV conversion works correctly.
● Confirmed speech recognition accuracy with clear audio samples.
● Validated game logic by simulating multiple random command inputs.
● Handled unrecognized or invalid commands gracefully by ignoring them.
Conclusion:
This project successfully demonstrates the integration of speech recognition technology
with simple gaming mechanics to create a voice-controlled interactive gaming system. By
processing audio files containing spoken commands, the system effectively recognizes
valid game inputs and executes corresponding actions such as shooting, jumping,
reloading, and playing stone-paper-scissors. Handling various audio formats within
nested folder structures and filtering irrelevant commands enhances its robustness and
usability.

The voice-command gaming approach not only enriches user experience by providing
hands-free interaction but also opens up opportunities for greater accessibility in gaming.
This foundation can be extended to more complex games and advanced voice control
features, paving the way for innovative, natural user interfaces in the gaming industry.

