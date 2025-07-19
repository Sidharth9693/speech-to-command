import pyttsx3
import speech_recognition as sr
import webbrowser
import datetime
import os

engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)
engine.setProperty("rate", 150)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def command():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        try:
            r.adjust_for_ambient_noise(source, duration=1)  
            audio = r.listen(source)
            content = r.recognize_google(audio, language='en-in')
            print("You said: " + content)
            return content.lower()
        except sr.UnknownValueError:
            print("Sorry, I didn't catch that.")
            return ""
        except Exception as e:
            print(f"Error: {e}")
            return ""

def main_process():
    speak("I am sidharthgpt . How can I help you?")
    
    while True:
        request = command()

        if request == "":
            continue  # If no command is detected, keep listening

        if "hello" in request:
            speak("Welcome! How can I help you?")
        
        elif "play music" in request:
            speak("Playing music...")
            webbrowser.open("https://www.youtube.com/watch?v=j7EofK_kajw&list=RDGMEM916WJxafRUGgOvd6dVJkeQVMj7EofK_kajw&start_radio=1")
        
        elif "time" in request:
            current_time = datetime.datetime.now().strftime("%I:%M %p")
            print(f"The current time is {current_time}")
            speak(f"The current time is {current_time}")
        
        elif "google" in request:
            speak("Opening Google for you")
            webbrowser.open("https://www.google.com/")
            webbrowser.open("https://mail.google.com/mail/u/0/#inbox")

        elif "exit" in request or "quit" in request or "stop" in request:
            speak("Goodbye! Have a great day!")
            break

        else:
            search_url = f"https://www.google.com/search?q={request.replace(' ', '+')}"
            speak(f"Searching Google for {request}")
            webbrowser.open(search_url)

main_process()
