# Feelify 🎵

An AI-powered emotion detection system that analyzes facial expressions and hand gestures to recognize emotions and recommend songs accordingly.

## Overview

Feelify uses computer vision and deep learning to detect human emotions in real-time and suggest personalized music recommendations based on the detected emotional state. The system combines MediaPipe for pose detection with a trained neural network to classify emotions with high accuracy.

## Features

- **Real-time Emotion Detection**: Analyzes facial landmarks and hand gestures to identify emotions
- **Multiple Emotion Support**: Recognizes various emotional states (happy, sad, angry, neutral, rock, etc.)
- **Web-based Interface**: User-friendly Streamlit application for easy interaction
- **Language Support**: Accepts language preference for localized recommendations
- **Song Recommendations**: Suggests music based on detected emotions
- **Live Video Streaming**: WebRTC-enabled real-time video processing
- **Visual Feedback**: Displays facial and hand landmarks with emotion labels

## Project Structure

```
Feelify/
├── feelify.py                    # Main Streamlit web application
├── style.css                     # Styling for the web interface
├── model.h5                      # Trained Keras neural network model
├── emotion.npy                   # Current detected emotion (state)
├── labels.npy                    # Emotion labels/categories
├── liveEmoji-main/              # Data collection and training utilities
│   ├── data_collection.py        # Script to collect emotion gesture data
│   ├── data_training.py          # Script to train the neural network
│   ├── inference.py              # Standalone inference script
│   ├── *.npy files              # Pre-collected training data for emotions
│   └── README.md
├── README.md                     # This file
└── logo.png                      # Project logo

```

## Technical Stack

- **Computer Vision**: MediaPipe, OpenCV
- **Machine Learning**: TensorFlow/Keras
- **Web Framework**: Streamlit, streamlit-webrtc
- **Data Processing**: NumPy
- **Video Processing**: av (PyAV)

## Model Architecture

The emotion detection model is a neural network with the following architecture:
- **Input Layer**: Normalized facial landmarks and hand gesture coordinates (168 features)
- **Hidden Layer 1**: 512 neurons with ReLU activation
- **Hidden Layer 2**: 256 neurons with ReLU activation
- **Output Layer**: Softmax activation with number of neurons equal to emotion categories

Training details:
- **Optimizer**: RMSprop
- **Loss Function**: Categorical Crossentropy
- **Epochs**: 50
- **Metrics**: Accuracy

## Installation

### Prerequisites

- Python 3.7 or higher
- Webcam/camera device
- pip or conda package manager

### Setup Instructions

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd Feelify
   ```

2. **Create a virtual environment** (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

   Or install packages individually:
   ```bash
   pip install streamlit streamlit-webrtc mediapipe opencv-python tensorflow keras numpy av
   ```

## Usage

### Running the Web Application

```bash
streamlit run feelify.py
```

The application will open in your default browser at `http://localhost:8501`

**Steps to use**:
1. Enter your preferred language in the text input field
2. Click to activate the webcam stream
3. Face the camera and maintain facial expressions/hand gestures
4. The system will detect and display your emotion in real-time
5. Click "Recommend me songs" button to get song recommendations based on detected emotion

### Data Collection and Model Training

If you want to train your own model or add new emotion categories:

1. **Collect Training Data**:
   ```bash
   cd liveEmoji-main
   python data_collection.py
   ```
   - Enter the emotion name when prompted
   - Face the camera and perform various gestures/expressions
   - The script collects 100 samples automatically

2. **Train the Model**:
   ```bash
   python data_training.py
   ```
   - Reads all `.npy` files in the directory
   - Trains a new neural network model
   - Saves the model as `model.h5` and labels as `labels.npy`

3. **Test Inference**:
   ```bash
   python inference.py
   ```
   - Tests the trained model with live webcam feed
   - Press ESC to exit

### Pre-collected Emotion Data

The project includes pre-trained data for the following emotions:
- happy.npy
- sad.npy
- angry.npy
- neutral.npy
- rock.npy

## How It Works

1. **Capture**: WebRTC captures video frames from the user's webcam
2. **Landmark Detection**: MediaPipe detects facial landmarks (468 points) and hand landmarks (21 points per hand)
3. **Feature Extraction**: Coordinates are normalized relative to facial reference points
4. **Prediction**: Neural network predicts emotion category from extracted features
5. **Display**: Emotion label is rendered on the video frame with landmark visualization
6. **Recommendation**: Based on detected emotion, songs are recommended to the user

## File Descriptions

| File | Purpose |
|------|---------|
| `feelify.py` | Main Streamlit application with WebRTC streaming and emotion detection |
| `style.css` | Custom CSS styling for the web interface |
| `model.h5` | Pre-trained Keras model for emotion classification |
| `emotion.npy` | NumPy array storing the current detected emotion state |
| `labels.npy` | NumPy array containing emotion category labels |
| `data_collection.py` | Utility to collect training data from webcam |
| `data_training.py` | Trains neural network on collected data |
| `inference.py` | Standalone script for testing emotion detection |

## Dependencies

- `streamlit`: Web application framework
- `streamlit-webrtc`: WebRTC support for Streamlit
- `mediapipe`: Pose and landmark detection
- `opencv-python` (cv2): Image processing
- `tensorflow/keras`: Deep learning framework
- `numpy`: Numerical computations
- `av`: Audio/video processing

## Performance Considerations

- The model uses relative coordinates to ensure invariance to camera position and scale
- Real-time processing requires a decent CPU; GPU acceleration is recommended for better performance
- Lighting conditions affect facial landmark detection accuracy

## Limitations

- Requires good lighting for accurate facial landmark detection
- Works best with frontal face orientation
- Emotion classification accuracy depends on training data quality and diversity
- May require calibration for different individuals

## Future Enhancements

- Add support for more emotion categories
- Implement emotion intensity/confidence scores
- Integrate with music streaming APIs (Spotify, YouTube Music)
- Add user emotion history and preferences tracking
- Support for offline operation
- Mobile application version
- Multi-user detection and emotion tracking

## Troubleshooting

### Model not loading
- Ensure `model.h5` and `labels.npy` are in the same directory as `feelify.py`
- Verify TensorFlow/Keras version compatibility

### Webcam not detected
- Check camera permissions in system settings
- Ensure no other application is using the camera
- Try restarting the application

### Poor emotion detection
- Ensure adequate lighting
- Face the camera directly
- Make clear emotional expressions or hand gestures
- Consider collecting more training data
