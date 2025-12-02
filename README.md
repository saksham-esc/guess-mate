# Guess Mate

Guess Mate is an interactive web-based guessing game built with Flask and powered by Google's Gemini AI. The game challenges users to think of an object, and the AI attempts to guess it by asking a series of yes/no/"I don't know" questions. The fewer questions needed, the higher the score!

## Features

- **AI-Powered Guessing**: Utilizes Google's Gemini 2.0 Flash model to generate intelligent questions and make accurate guesses.
- **Interactive Web Interface**: Clean, responsive UI with animations and progress tracking.
- **Scoring System**: Earn up to 100 points based on how quickly the AI guesses your object (5 points deducted per question).
- **Session Management**: Maintains game state across questions using Flask sessions.
- **Asynchronous Processing**: Leverages async Flask for smooth AI interactions.

## Requirements

- Python 3.7+
- Google Cloud API Key (for Gemini AI)
- Flask 2.0+ with async support
- python-dotenv
- google-generativeai 0.3.2+

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd guess-mate
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Set up environment variables:
   - Create a `.env` file in the root directory
   - Add your Google API key:
     ```
     GOOGLE_API_KEY=your_google_api_key_here
     FLASK_SECRET_KEY=your_secret_key_here
     ```

## Usage

1. Run the application:
   ```bash
   python app.py
   ```

2. Open your browser and navigate to `http://localhost:5000`

3. Click "Start Game" on the home page

4. Think of an object and answer the AI's questions with Yes, No, or "I don't know"

5. View your final score and the AI's guess on the results page

## Project Structure

```
guess-mate/
├── app.py                 # Main Flask application
├── requirements.txt       # Python dependencies
├── templates/
│   ├── index.html         # Home page
│   ├── play.html          # Game interface
│   └── results.html       # Results page
└── README.md              # This file
```

## How It Works

1. The game starts with the user thinking of an object
2. Gemini AI generates the first question
3. User answers with Yes, No, or "I don't know"
4. AI uses previous Q&A to generate follow-up questions
5. After 20 questions or when ready, AI makes a final guess
6. Score is calculated based on questions asked

## API Key Setup

To use this application, you need a Google Cloud API key with access to the Generative AI API:

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the Generative AI API
4. Create an API key in the Credentials section
5. Add the key to your `.env` file as `GOOGLE_API_KEY`

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the [MIT License](LICENSE).
