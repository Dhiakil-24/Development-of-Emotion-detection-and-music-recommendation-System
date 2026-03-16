# EmoTune 🎵🧠

An AI-powered emotion detection and music recommendation system. EmoTune analyzes your facial expression in real-time and recommends Spotify tracks that match your mood — across multiple languages and genres.

---

## Features

- **Real-time emotion detection** via webcam or image upload using a trained CNN model
- **Spotify music recommendations** tailored to 8 detected emotions
- **Multi-language support** — English, Hindi, Spanish, French, German, Italian, Portuguese
- **User authentication** — register, login, JWT-protected sessions, password reset
- **Liked songs** — save tracks and revisit them anytime
- **User profile** — stats, preferences, profile picture upload
- **Music search** — search Spotify tracks directly
- **Mood analytics** — visualize your emotion history with charts
- **Music Reflex Game** — a fun mini-game built into the app

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, React Router 7, Framer Motion, Recharts, Axios |
| Backend | Flask 3, Flask-JWT-Extended, Flask-CORS |
| ML / AI | TensorFlow 2.17, Keras CNN, OpenCV (Haar Cascade) |
| Music API | Spotify Web API via Spotipy |
| Database | SQLite |
| Auth | JWT + bcrypt |

---

## Project Structure

```
Completed_code/
├── backend/
│   ├── app.py                  # Flask app factory
│   ├── config.py               # Configuration & env vars
│   ├── requirements.txt
│   ├── database/
│   │   ├── schema.sql          # DB schema
│   │   └── emotune.db          # SQLite database
│   ├── model/
│   │   ├── best_model.keras    # Trained emotion CNN
│   │   └── class_indices.json  # Emotion class mapping
│   ├── routes/
│   │   ├── auth.py             # Auth endpoints
│   │   ├── emotion.py          # Emotion detection endpoints
│   │   ├── music.py            # Music recommendation endpoints
│   │   └── user.py             # User profile endpoints
│   └── utils/
│       ├── emotion_detector.py # Model inference logic
│       ├── image_processor.py  # Image preprocessing
│       └── db_helper.py        # Database operations
└── frontend/
    ├── package.json
    └── src/
        ├── App.js
        ├── components/
        │   ├── Auth/           # Login & Register pages
        │   ├── EmotionDetection/ # Main app & webcam capture
        │   ├── Games/          # Music Reflex Game
        │   ├── LandingPage/
        │   └── Profile/
        └── services/
            └── api.js          # Axios API client
```

---

## Getting Started

### Prerequisites

- Python 3.9+
- Node.js 18+
- A [Spotify Developer](https://developer.spotify.com/dashboard) account (for Client ID & Secret)

---

### Backend Setup

```bash
cd Completed_code/backend

# Install dependencies
pip install -r requirements.txt

# Create your .env file
cp .env.example .env
```

Edit `.env` with your values:

```env
SECRET_KEY=your-super-secret-key
FLASK_ENV=development
FLASK_APP=app.py

SPOTIFY_CLIENT_ID=your_spotify_client_id
SPOTIFY_CLIENT_SECRET=your_spotify_client_secret

DATABASE_URL=sqlite:///database/emotune.db

JWT_SECRET_KEY=your-jwt-secret-key
JWT_ACCESS_TOKEN_EXPIRES=3600

PORT=5000
HOST=127.0.0.1
```

```bash
# Run the backend
python app.py
```

Backend runs at `http://127.0.0.1:5000`

---

### Frontend Setup

```bash
cd Completed_code/frontend

# Install dependencies
npm install

# Create .env file
echo "REACT_APP_API_URL=http://127.0.0.1:5000/api" > .env

# Start the app
npm start
```

Frontend runs at `http://localhost:3000`

---

## API Endpoints

### Authentication
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register new user |
| POST | `/api/auth/login` | Login |
| POST | `/api/auth/forgot-password` | Request password reset |
| POST | `/api/auth/verify-reset-code` | Verify reset code |
| POST | `/api/auth/reset-password` | Set new password |
| GET | `/api/auth/validate-token` | Validate JWT token |

### Emotion Detection
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/emotion/detect-upload` | Detect emotion from uploaded image |
| POST | `/api/emotion/detect-live` | Detect emotion from webcam frame |
| GET | `/api/emotion/model-info` | Get model metadata |

### Music
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/music/recommendations/<emotion>` | Get tracks by emotion (`?language=hindi&limit=6`) |
| POST | `/api/music/like` | Like/save a song |
| DELETE | `/api/music/unlike/<song_id>` | Remove liked song |
| GET | `/api/music/liked` | Get user's liked songs |
| GET | `/api/music/search` | Search Spotify tracks |
| GET | `/api/music/track/<track_id>` | Get track details |

### User
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/user/profile` | Get user profile |
| PUT | `/api/user/profile/edit` | Update profile |
| POST | `/api/user/profile/picture` | Upload profile picture |
| GET | `/api/user/statistics` | Get mood & listening stats |
| PUT | `/api/user/preferences` | Update genre preferences |
| DELETE | `/api/user/account` | Delete account |

---

## Emotion → Genre Mapping

| Emotion | Genres |
|---|---|
| Happy | Pop, Dance, Party |
| Sad | Acoustic, Indie, Piano |
| Angry | Rock, Metal, Hard Rock |
| Neutral | Chill, Ambient, Lo-fi, Jazz |
| Surprised | Electronic, EDM, Dance |
| Fearful | Classical, Calm, Meditation |
| Disgusted | Punk, Alternative, Indie |
| Excited | Dance, Electronic, Upbeat |

---

## Database Schema

- `users` — accounts, profile info, preferred genres
- `liked_songs` — saved tracks with emotion context
- `password_reset_codes` — time-limited reset tokens
- `session_tokens` — JWT blacklisting support

---

## How It Works

1. User logs in and grants webcam access (or uploads a photo)
2. The image is sent to the backend and preprocessed (grayscale, resized to 75×75)
3. OpenCV's Haar Cascade detects the face region
4. The cropped face is passed through the trained Keras CNN
5. The predicted emotion is returned with a confidence score
6. The backend queries Spotify for tracks matching the emotion's genre profile
7. Tracks are returned to the frontend and displayed with previews

---

## Environment Variables Reference

| Variable | Description | Default |
|---|---|---|
| `SECRET_KEY` | Flask secret key | — |
| `JWT_SECRET_KEY` | JWT signing key | — |
| `SPOTIFY_CLIENT_ID` | Spotify app client ID | — |
| `SPOTIFY_CLIENT_SECRET` | Spotify app client secret | — |
| `DATABASE_URL` | SQLite DB path | `sqlite:///database/emotune.db` |
| `FLASK_ENV` | `development` or `production` | `development` |
| `PORT` | Backend port | `5000` |
| `REACT_APP_API_URL` | Backend API base URL (frontend) | `http://127.0.0.1:5000/api` |

---

## License

This project is for educational purposes.
