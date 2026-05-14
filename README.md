# voice-assistant-project---peter

import pyttsx3
import speech_recognition as sr
import webbrowser
import datetime
import pyjokes

# ---------- Text to Speech ----------
def speechtx(text):
    engine = pyttsx3.init()
    voices = engine.getProperty("voices")
    engine.setProperty("voice", voices[0].id)
    engine.setProperty("rate", 150)
    engine.say(text)
    engine.runAndWait()

# ---------- Speech to Text ----------
def sptext():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source, duration=0.5)
        audio = recognizer.listen(source)

    try:
        print("Recognizing...")
        data = recognizer.recognize_google(audio)
        print("User:", data)
        return data.lower()
    except sr.UnknownValueError:
        return None

# ---------- Main Program ----------
if __name__ == "__main__":

    # Welcome message (spoken only once)
    speechtx("Hi Sanjib, how can I help you?")

    while True:
        command = sptext()

        if command is None:
            continue

        # Greeting
        if command in ["hello peter", "hi peter", "hey peter"]:
            speechtx("Yes Sanjib")

        # Name
        elif "your name" in command:
            speechtx("My name is Peter")

        # Age
        elif "how old are you" in command:
            speechtx("I am 22 years old")

        # Time
        elif "time" in command:
            time = datetime.datetime.now().strftime("%I:%M %p")
            speechtx(f"The current time is {time}")

        # YouTube
        elif "youtube" in command:
            speechtx("Opening YouTube")
            webbrowser.open("https://www.youtube.com")

        # Facebook
        elif "facebook" in command:
            speechtx("Opening Facebook")
            webbrowser.open("https://www.facebook.com")

        # Joke
        elif "joke" in command:
            joke = pyjokes.get_joke(language="en", category="neutral")
            speechtx(joke)

        # Exit
        elif command in ["exit", "quit", "stop"]:
            speechtx("Goodbye Sanjib")
            break

        else:
            speechtx("Sorry, I did not understand that command")


