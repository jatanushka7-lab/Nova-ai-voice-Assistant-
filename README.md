# Nova-ai-voice-Assistant-
: A Python-based AI voice assistant with voice commands and useful features.
mport speech_recognition as sr
import pyttsx3
import datetime
import pywhatkit
import wikipedia
import requests
import pyjokes

engine = pyttsx3.init()

def speak(text):
    print("Speaking:", text)

    engine=pyttsx3.init('sapi5')
    engine.setProperty('rate', 170)
    engine.say(text)
    engine.runAndWait()
    engine.stop()
recognizer = sr.Recognizer()
while True:
 with sr.Microphone() as source:
        print("Listening...")
        speak("Heyy Anushka. I am listening.")

        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source, timeout=10, phrase_time_limit=5)

 try:
            command = recognizer.recognize_google(audio)
            if"nova" in command:
                  command=command.replace("nova","").strip()
            print("You said:", command)
            speak("You said" + command)

            if "hello" in command.lower():
                 speak("hello Nush! How can i help you?")

            elif"time" in command.lower():
                 current_time=datetime.datetime.now().strftime("%H:%M:%S")
                 print(current_time)
                 speak("The time is " + current_time)
            elif"date" in command:
                  today = datetime.datetime.now()
                  speak("Todays's date is"+ today.strftime("%B %d, %Y")) 

            elif"google" in command.lower():
                 speak("Opening Google")
                 pywhatkit.search("Google")

            elif"youtube" in command.lower():
                    speak("Opening YouTube")
                    pywhatkit.playonyt("YouTube")

            elif"who is" in command.lower():
                  person = command.lower().replace("who is","").strip()


                  try:
                   info = wikipedia.summary(person, sentences=2)
                   print(info)
                   speak(info)

                  except Exception as e:
                     print(e)
                     speak("Sorry, I could not find any information about " + person)

            elif"weather" in command.lower():
                  city = "Indore"

                  try:
                        url = "https://geocoding-api.open-meteo.com/v1/search"
                        params = {"name": city, "count": 1,"language": "en", "format": "json"}

                        data= requests.get(url, params=params).json()
                        location=data["results"][0]

                        weather_url = "https://api.open-meteo.com/v1/forecast"
                        weather_params = {"latitude": location["latitude"], "longitude": location["longitude"],
                                           "current": "temperature_2m,weather_code"}
                        weather=requests.get(weather_url, params=weather_params).json()
                        temperature=weather["current"]["temperature_2m"]
                        answer="The temperature in indore is"+ str(temperature) + "degrees Celsius."
                        print(answer)
                        speak(answer)
                        print("Temperature in indore:",temperature,"c")
                        speak("The temperature in Indore is " + str(temperature) + " degrees Celsius.")
                  except Exception as e:
                        print(e)
                        speak("Sorry i could not get the whether information.")

            elif"calculate" in command.lower() or "calculator" in command.lower():
                  try:
                        expression= command.lower()
                        expression= expression.replace("calculate","")
                        expression= expression.replace("calculator","")
                        expression= expression.replace("plus","+")
                        expression= expression.replace("minus","-")
                        expression= expression.replace("multiply","*")
                        expression= expression.replace("multiplied by","*")
                        expression= expression.replace("divide","/")
                        expression= expression.replace("divided by","/")

                        result= eval(expression)

                        print("Answer",result)
                        speak("The answer is"+str(result))

                  except Exception as e:
                        print(e)
                        speak("Sorry i could not calculate that.")

            elif "play music" in command.lower() or "play song" in command.lower():
                  speak("Sure, playing music") 
                  pywhatkit.playonyt("popular songs")

            elif "joke" in command.lower():
                              joke= pyjokes.get_joke()
                              print(joke)
                              speak(joke)
            elif "who made you" in command or "who created you" in command:
                  speak("My creater? Anushka My purpose? To be her smartest little assistant.")

            elif "who are you" in command:
                  speak("I am Nova,your personal AI assistant")

            elif "exit" in command.lower() or "quit" in command.lower():
                  speak("Goodbye Nush, have a nice day!")
                  break   
            

            else:
                speak("I heard you say " + command)
                     

 except Exception as e:
        print(e)
        speak("Sorry, I could not understand.")
