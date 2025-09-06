
# 🎙️ BLESSEE – Your Own Python Voice Assistant

Turn your voice into power using Python 🐍.  
This is **BLESSEE**, a beginner-friendly desktop voice assistant that you can build and customize – Telugu style!  

---

## ✨ Features
- ✅ Listen to your voice  
- ✅ Understand what you said  
- ✅ Open Chrome or VS Code  
- ✅ Play songs on YouTube  
- ✅ Tell jokes  
- ✅ Give you the current time  
- ✅ Tell who someone is using Wikipedia  
- ✅ And most importantly… it talks about you!  

---

## 🛠️ Tools Required
- Python 3.8 or above  
- VS Code (or any editor)  
- A microphone  
- Internet connection (for YouTube/Wikipedia)  

---

## 🚀 Setup Instructions
# Steps
 
 
  Create Project Folder
```bash
mkdir voice-desktop-assistant
cd voice-desktop-assistant
py coding! 🌟

 2️⃣ Create and Activate Virtual Environment
python -m venv venv

 Windows
venv\Scripts\activate

 Mac/Linux
source venv/bin/activate


 3️⃣ Install Dependencies
pip install SpeechRecognition pyttsx3 pywhatkit wikipedia pyjokes


 4️⃣ Create the Assistant Script
echo > assistant.py   # This creates the assistant.py file

# Code

import speech_recognition as sr
import pyttsx3
import pywhatkit
import datetime
import wikipedia
import pyjokes
import os
import sys

# Initialize speech engine
engine = pyttsx3.init()
engine.setProperty('rate', 170)
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)  # Use female voice

def talk(text):
    print("🎙️ GIRI:", text)
    engine.say(text)
    engine.runAndWait() 

def take_command():
    listener = sr.Recognizer()
    with sr.Microphone() as source:
        print("🎧 Listening...")    
        listener.adjust_for_ambient_noise(source)
        voice = listener.listen(source)
    try:
        command = listener.recognize_google(voice)
        command = command.lower()
        print("🗣️ You said:", command)
    except sr.UnknownValueError:
        talk("Sorry bro, I didn’t catch that.")
        return ""
    except sr.RequestError:
        talk("Network issue with Google service.")
        return ""
    return command

def run_blessee():
    command = take_command()

    if "play" in command:
        song = command.replace("play", "")
        talk("Playing on YouTube 🎶")
        pywhatkit.playonyt(song)

    elif "what's the time" in command:
        time = datetime.datetime.now().strftime('%I:%M %p')
        talk(f"It’s {time} ⏰")

    elif "who is uday codes" in command or "who is uday_codes" in command:
        info = (
            "Uday, known as uday_codes on Instagram, is a coding content creator. "
            "He teaches Python projects in Telugu and runs udaycodes.in 💻"
        )
        talk(info)

    elif "who is" in command:
        person = command.replace("who is", "").strip()
        try:
            info = wikipedia.summary(person, sentences=1)
            talk(info)
        except:
            talk("Sorry, I couldn’t find information about that person.")

    elif "joke" in command:
        talk(pyjokes.get_joke())

    elif "open chrome" in command:
        chrome_path = "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe"
        if os.path.exists(chrome_path):
            talk("Opening Chrome 🚀")
            os.startfile(chrome_path)
        else:
            talk("Chrome path not found 😬")

    elif "open code" in command or "open vs code" in command:
        talk("Opening VS Code 💻")
        os.system("code")

    elif "exit" in command or "stop" in command:
        talk("Okay bro, see you later 👋")
        sys.exit()

    elif command != "":
        talk("I heard you, but I don’t understand that yet 😅")

talk("Yo! I'm BLESSEE – your personal voice assistant 💡")
while True:
    run_blessee()

