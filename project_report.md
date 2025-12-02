# Guess Mate Project Report

## Backend Flow Description

The backend of Guess Mate is built using Flask, a lightweight web framework for Python. It leverages asynchronous programming to handle AI interactions with Google's Gemini model without blocking the server. The application manages game state through Flask sessions, ensuring a seamless user experience across multiple requests.

### Detailed Backend Flow

1. **Application Initialization**:
   - Load environment variables (API keys, secret keys).
   - Configure Google Generative AI with the API key.
   - Initialize the Gemini model (gemini-2.0-flash).
   - Set up Flask app with secret key for session management.

2. **Route Handling**:
   - **Index Route (/)**: Renders the home page (index.html) where users start the game.
   - **Play Route (/play)**: Handles the core game logic.
     - Initializes session variables if starting a new game (qnum=0, empty questions and answers lists).
     - On POST requests:
       - If starting (qnum=0 and answer="start"), increment qnum to 1 and generate the first question using AI.
       - If answering a question (qnum > 0), append the answer to session["answers"], increment qnum.
     - Check if game should end (qnum > MAX_QUESTIONS):
       - Generate a guess based on all Q&A pairs.
       - Calculate score (100 - 5 * (qnum-1)).
       - Clear session and render results.
     - If not ended, generate the next question (first one if qnum=1, else based on history).
     - Render play.html with current question, qnum, and MAX_QUESTIONS.

3. **Session Management**:
   - Uses Flask's session to store game state: qnum (current question number), questions (list of questions asked), answers (list of user answers).
   - Session persists across requests, allowing the game to continue seamlessly.

4. **AI Interaction**:
   - Asynchronous calls to Gemini model using asyncio.to_thread to avoid blocking.
   - Prompts are crafted to generate concise yes/no/I don't know questions or final guesses.
   - For questions: "Ask a concise yes/no/I don't know question about an object. Do not include any description or explanation."
   - For follow-ups: "Based on previous questions and answers, ask the next concise yes/no/I don't know question without any description or explanation."
   - For guesses: "Based on these questions and answers, guess the object. Respond with only the object name, no explanation."

5. **Game State Machine**:
   - States: Idle (qnum=0), Playing (0 < qnum <= MAX_QUESTIONS), Ended (qnum > MAX_QUESTIONS).
   - Transitions:
     - Idle -> Playing: User clicks "Start".
     - Playing -> Playing: User answers question, qnum increments.
     - Playing -> Ended: qnum exceeds MAX_QUESTIONS, guess is made, score calculated.

6. **Rendering and Response**:
   - Templates are rendered with context variables (e.g., question, qnum, score).
   - Results page shows the AI's guess and score.

### ASCII Diagram of Backend Flow

```
[User Request] --> [Flask App]
                        |
                        v
                [Route Dispatcher]
                        |
                        +--> [/] Index Route --> Render index.html
                        |
                        +--> [/play] Play Route
                                |
                                +--> Check Session State
                                |       |
                                |       +--> New Game: Init session (qnum=0, questions=[], answers=[])
                                |       |
                                |       +--> POST: Process Answer or Start
                                |               |
                                |               +--> Start: qnum=1, Generate First Question (AI Call)
                                |               |
                                |               +--> Answer: Append answer, qnum++
                                |
                                +--> Check End Condition (qnum > MAX_QUESTIONS)
                                |       |
                                |       +--> Yes: Generate Guess (AI Call), Calculate Score, Clear Session, Render results.html
                                |       |
                                |       +--> No: Generate Next Question (AI Call), Render play.html
```

This diagram illustrates the high-level flow, focusing on decision points and AI interactions.

## Technical Depth in app.py

Refer to app.py for detailed comments added, including sections on Route Handling, Session Management, AI Interaction, and Game State Machine.
