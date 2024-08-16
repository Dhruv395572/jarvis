import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os
import smtplib
import cv2
from requests import get 
import pywhatkit
import time  # Import the time module

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def wishMe():
    hour = datetime.datetime.now().hour
    if 0 <= hour < 12:
        speak("Good morning!")
    elif 12 <= hour < 15:
        speak("Good afternoon!")
    elif 15 <= hour < 18:
        speak("Good evening!")

    speak("I am your personal voice command assistant, Jack. How can I assist you today?")

def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.adjust_for_ambient_noise(source, duration=1)  # Adjust for ambient noise
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')  # Changed language to English (India)
        print(f"User said: {query}\n")
    except Exception as e:
        print("Say that again please...")
        return "None"

    return query.lower()

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.starttls()
    server.login('youremail@gmail.com', 'your-password')
    server.sendmail('youremail@gmail.com', to, content)
    server.close()

if __name__ == "__main__":
    wishMe()
    
    while True:
        query = takeCommand()

        if 'hello' in query:
            speak("Hello! How can I help you?")
        elif 'what\'s your name' in query:
            speak('My name is Jack! I am your personal assistant.')
        elif 'wikipedia' in query:
            speak("Searching Wikipedia...")
            query = query.replace('wikipedia', '')
            results = wikipedia.summary(query, sentences=5)
            speak("According to Wikipedia...")
            print(results)
            speak(results)
        elif 'open youtube' in query:
            speak("Opening YouTube...")
            webbrowser.open("https://www.youtube.com")
        elif 'open whatsapp' in query:
            speak("Opening WhatsApp...")
            webbrowser.open("https://web.whatsapp.com")
        elif 'open instagram' in query:
            speak("Opening Instagram...")
            webbrowser.open("https://www.instagram.com")
        elif 'open chat gpt' in query:
            speak("Opening ChatGPT...")
            webbrowser.open("https://web.chatgpt.com")
        elif 'what\'s the time' in query or 'whats the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            print(strTime)
            speak("Sir! The time is")
            speak(strTime)
        elif 'open vs code' in query:
            app = "C:\\Users\\victus\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
            os.startfile(app)
        elif 'open chrome' in query:
            app1 = "C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Google Chrome.lnk"
            os.startfile(app1)
        elif 'open notepad' in query:
            speak("Opening Notepad...")
            app2 = "C:\\Program Files\\Microsoft Office\\root\\Office16\\ONENOTE.EXE" 
            os.startfile(app2)
        elif 'open powerpoint' in query:
            speak("Opening PowerPoint...")
            app3 = "C:\\Program Files\\Microsoft Office\\root\\Office16\\POWERPNT.EXE" 
            os.startfile(app3)
        elif 'open word' in query:
            speak("Opening Word...")
            app4 = "C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Word.lnk"
            os.startfile(app4)
        elif 'open excel' in query:
            speak("Opening Excel...")
            app5 = "C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Excel.lnk"
            os.startfile(app5)
        elif 'open cmd' in query:
            app6 = "C:\\Users\\victus\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\System Tools\\Command Prompt.lnk"
            os.startfile(app6)
        elif 'open camera' in query:
            speak('Opening the camera...')
            cap = cv2.VideoCapture(0)  # Open default camera (index 0)
            
            while True:
                ret, img = cap.read()
                cv2.imshow('webcam', img)
                k = cv2.waitKey(1)
                if k == 27:  # Press ESC key to exit
                    break

            cap.release()  # Release the camera
            cv2.destroyAllWindows()
        elif 'ip address' in query:
            ip = get('https://api.ipify.org').text
            speak(f"Your IP address is {ip}")
        elif 'send message' in query:
            speak("Whom do you want to send a message to?")
            recipient_name = takeCommand()

            if 'sorry' in recipient_name or 'repeat' in recipient_name or 'none' in recipient_name:
                speak("Sorry, I didn't catch that. Please try again.")
                continue

            message = f"Hello {recipient_name}, this is a test message sent using Python."

            # Calculate scheduled time (10 seconds from current time)
            scheduled_time = time.time() + 10  # 10 seconds from current time
            
            speak(f"Sending message to {recipient_name} on WhatsApp after 10 seconds...")
            time.sleep(10)  # Wait for 10 seconds
            pywhatkit.sendwhatmsg("{recipient_name}", message, scheduled_time.hour, scheduled_time.minute)  # Replace with recipient's phone number
        elif 'what\'s your name' in query or'whats your name' in query:
            speak("My name is Jack")
        elif 'tell me about you' in query:
            speak("I am your personal voice assistant, Jack. My founder is Dhruv Kaushik, and he was born on October 28, 2006.")
        elif 'about your founder' in query:
            speak("My founder is Dhruv Kaushik. He was born on October 28, 2006.")
        elif 'detail of founder' in query:
            speak("If you need personal details about Dhruv, you need to enter the 4-digit PIN.")
            pin = 2810
            user_input = int(input("Enter the 4-digit PIN:"))
            if pin == user_input:
                speak("""You are authorized. Now, I will tell you about Dhruv Kaushik. He uses an HP Victus laptop. 
                        His contact details are 8630548315 and 7017365398. His father's name is Manoj Kaushik, 
                        and his mother's name is Preeti Kaushik. His brother's name is Raghav. He lives in Mathura, 
                        and his address is C17, Indupuram. His laptop password is 2810. If you need more details, 
                        please ask.""")
            else:
                speak("You are not authorized.")
        elif 'goodbye' in query or 'bye-bye' in query or 'go to sleep' in query:
            speak("Goodbye!")
            break
        else:
            speak("Sorry, I didn't catch that. Can you please repeat?")

