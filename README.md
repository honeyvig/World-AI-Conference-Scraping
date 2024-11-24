# World-AI-Conference-Scraping
There are three components of this research about global AI governance and China's world stage, the World Artificial Intelligence Conference.

1) I would like to scrape the attendance records of the WAIC conference.
https://www.worldaic.com.cn/wangjie?year=2023

2) I would like to scrape the agenda of the WAIC
https://www.worldaic.com.cn/wangjie?year=2023

3) I would like to capture the text from videos from the WAIC (most advanced). We can use Whisper AI to convert the speech to text and GPT to classify the speakers.

https://www.bilibili.com/video/BV1iT421k7Bv/?spm_id_from=333.337.search-card.all.click&vd_source=346035d18cf120ed8447fcc6e9d76d2a
===================
To address your research needs on global AI governance and China's role in the World Artificial Intelligence Conference (WAIC), we'll break the task down into the following steps:

    Scraping Attendance Records from the WAIC website: Extract data on participants from the provided URL.
    Scraping the Agenda of the WAIC: Extract details of the agenda and sessions.
    Extracting Text from Videos: Using Whisper AI to transcribe audio from the WAIC videos and GPT to classify the speakers.

Let's go through the implementation for each component in Python.
1. Scraping Attendance Records from WAIC

You can use Python's requests and BeautifulSoup libraries to scrape HTML content from the page and extract the attendance records. However, the URL you provided (https://www.worldaic.com.cn/wangjie?year=2023) does not return any structured attendance data directly on the page. If it requires logging in or interacting with a form, we'll need to consider that.

Below is a simple example for scraping a webpage:

import requests
from bs4 import BeautifulSoup

# Function to scrape the attendance records from WAIC website
def scrape_attendance():
    url = 'https://www.worldaic.com.cn/wangjie?year=2023'
    
    # Send a GET request to the website
    response = requests.get(url)
    
    # If the request was successful (status code 200)
    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        
        # Assuming attendance data might be in tables or lists, inspect the structure
        # This is a placeholder and would require you to identify the HTML structure
        # Example for extracting items (adjust based on actual HTML content)
        attendees = soup.find_all('div', class_='attendee_class_name')  # Adjust to actual class name
        
        # Extract and print attendee names and roles (you need to adjust based on actual HTML)
        for attendee in attendees:
            name = attendee.find('span', class_='name_class').text.strip()
            role = attendee.find('span', class_='role_class').text.strip()
            print(f"Name: {name}, Role: {role}")
    else:
        print(f"Failed to retrieve data. Status code: {response.status_code}")

scrape_attendance()

Note: You'll need to inspect the webpage's structure and adjust the find_all() or find() methods to match the HTML elements where the attendance information resides.
2. Scraping the Agenda of the WAIC

Similarly, to scrape the agenda, we can follow a similar approach as above but focus on extracting information relevant to sessions, times, and speakers.

def scrape_agenda():
    url = 'https://www.worldaic.com.cn/wangjie?year=2023'
    
    # Send GET request
    response = requests.get(url)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        
        # Again, inspect and adapt based on actual HTML structure
        agenda_items = soup.find_all('div', class_='agenda_class_name')  # Adjust class names
        
        for item in agenda_items:
            session_title = item.find('h3', class_='session_title_class').text.strip()
            session_time = item.find('span', class_='session_time_class').text.strip()
            speaker_name = item.find('span', class_='speaker_class').text.strip()
            print(f"Session: {session_title}, Time: {session_time}, Speaker: {speaker_name}")
    else:
        print(f"Failed to retrieve agenda. Status code: {response.status_code}")

scrape_agenda()

Note: Similar to the attendance scraping, inspect the HTML structure to identify correct tags and classes for session details.
3. Capturing Text from Videos using Whisper AI

For this part, we'll use Whisper AI by OpenAI to transcribe the audio content from the video URL you provided. You can download the video using yt-dlp and then use Whisper to transcribe it.

Step 1: Install necessary libraries:

pip install yt-dlp openai-whisper

Step 2: Download the video and extract audio using yt-dlp.

import yt_dlp
import whisper

# Function to download the video and extract audio
def download_video(url):
    ydl_opts = {
        'format': 'bestaudio/best',  # Get the best audio quality
        'outtmpl': 'waic_video.%(ext)s',  # Output template for the filename
        'postprocessors': [{
            'key': 'FFmpegAudioConvertor',
            'preferredcodec': 'mp3',  # Convert audio to mp3
            'preferredquality': '192',
        }],
    }
    
    with yt_dlp.YoutubeDL(ydl_opts) as ydl:
        ydl.download([url])

# Function to transcribe the audio using Whisper
def transcribe_audio(audio_path):
    # Load the Whisper model
    model = whisper.load_model("base")
    
    # Transcribe the audio to text
    result = model.transcribe(audio_path)
    print("Transcription:")
    print(result['text'])

# Download video from the URL and transcribe it
video_url = 'https://www.bilibili.com/video/BV1iT421k7Bv/?spm_id_from=333.337.search-card.all.click&vd_source=346035d18cf120ed8447fcc6e9d76d2a'
download_video(video_url)

# After downloading, transcribe the audio from the video
transcribe_audio('waic_video.mp3')

4. Classifying Speakers using GPT

After extracting text from the video, we can pass the transcription to GPT for classification of speakers (if they are identifiable based on the transcriptions).

import openai

# Function to classify speakers using GPT-3
def classify_speakers(transcript):
    openai.api_key = 'your-openai-api-key'
    
    # Define the prompt for classification
    prompt = f"Identify the speakers in the following transcript:\n\n{transcript}\n\nList the speakers."
    
    # Call GPT-3 to classify the speakers
    response = openai.Completion.create(
        engine="text-davinci-003",  # Or any GPT engine of your choice
        prompt=prompt,
        max_tokens=500
    )
    
    print("Speakers Classification:")
    print(response['choices'][0]['text'])

# Example: Transcribe text and classify speakers
transcription_text = "John Doe: Hello, welcome to the WAIC conference. ... Jane Smith: Thank you, John. It's great to be here."
classify_speakers(transcription_text)

Conclusion:

This Python code addresses the three components:

    Scraping attendance records from the WAIC website.
    Scraping the agenda of the WAIC conference.
    Extracting and transcribing video audio using Whisper AI, followed by GPT-3 classification of speakers.

Each of these tasks can be enhanced further based on the specific requirements and the availability of structured data (e.g., HTML structure for scraping, video quality for transcription
