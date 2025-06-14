# Virtual-Tryon-System
Made by Omkar Gat.

This application provides a virtual dressing room experience where users can try on various accessories through webcam-based augmented reality.

## Features

- Real-time virtual try-on of accessories using webcam
- WebSocket (Socket.IO) based streaming for low-latency experience
- Face detection with facial landmark tracking
- Support for multiple accessories (necklaces, earrings)
- Web-based interface for easy access from any device
- Button-based launcher for virtual try-on in a new tab
- Secure file path handling and input validation
- Rate limiting to prevent abuse
- Comprehensive error handling and logging

## Requirements

- Python 3.7+
- Flask web framework
- Flask-SocketIO for real-time communication
- OpenCV for image processing
- dlib for face detection and landmark tracking
- Additional packages listed in requirements.txt

## Large Files

This repository uses Git Large File Storage (LFS) to handle large files efficiently. The following files are tracked with Git LFS:

- `data/shape_predictor_68_face_landmarks.dat` (~100MB) - Face landmarks model for face detection
- `static/jewelry/Necklace15.png` (~1.2MB) - Large image asset
- Any files with "Yowza" in the name

### Using Git LFS

To clone this repository with all large files:

1. **Install Git LFS** (if you haven't already):
   ```bash
   # Install Git LFS
   git lfs install
   ```

2. **Clone the repository normally**:
   ```bash
   git clone <repository-url>
   cd Virtual-Dressing-Room
   ```

Git LFS will automatically download the large files when you clone or pull.

If you already cloned the repository before installing Git LFS, run:
```bash
git lfs pull
```

For more information about Git LFS, visit [git-lfs.github.com](https://git-lfs.github.com/).

## Installation

For detailed installation instructions, please refer to the [Installation Guide](INSTALLATION_GUIDE.md).

Quick setup:

1. Clone the repository with Git LFS as described above:
```
git clone https://github.com/sun-fibo-intern/Virtual-Tryon-System
cd Virtual-Dressing-Room
```

2. Create a virtual environment (recommended):
```
# Using conda
conda create -n virtual_dressing python=3.8
conda activate virtual_dressing

# Using venv
python -m venv venv
# On Windows
venv\Scripts\activate
# On macOS/Linux
source venv/bin/activate
```

3. Install dependencies:
```
pip install -r requirements.txt
```

## Usage

1. Start the Flask application:
```
python flask_app.py
```

2. Open a web browser and navigate to:
```
http://localhost:5001/
```

3. Allow access to your webcam when prompted
4. Browse different items (necklaces, earrings)
5. Click "Try it Now!" on any item to see it overlaid on your webcam feed in real-time

## Troubleshooting

If you encounter any issues while installing or running the application, please refer to the [Installation Guide](INSTALLATION_GUIDE.md#troubleshooting) for detailed troubleshooting steps.

Common issues include:
- Python environment problems
- Missing dependencies
- Webcam access issues
- Port conflicts
- Face detection problems

## Project Structure

- `flask_app.py` - Main Flask application with routes and Socket.IO implementation
- `virtual_trial.py` - Core functionality for virtual try-on with OpenCV
- `templates/` - HTML templates for the web interface
  - `jewelry_brand.html` - Main jewelry brand page with tabs
  - `index.html` - Original catalog page with item listings for try-on
  - `checkout.html` - Webcam try-on interface with Socket.IO connection
  - `jewelry_checkout.html` - Checkout page for the jewelry store
  - `error.html` - Error page template
  - `api_docs.html` - API documentation page
- `static/` - Static files (images, CSS, JavaScript)
  - `images/` - Images of accessories to try on
  - `css/` - Stylesheets for the web interface
  - `js/` - JavaScript files
- `data/` - Data files (face detection model)
- `uploads/` - Temporary storage for uploaded images

## API Documentation

For detailed API documentation, please refer to [api_docs.md](api_docs.md)

The application provides the following API endpoints:

- `GET /` - Main jewelry brand page
- `GET /jewelry` - Main jewelry brand page (same as root)
- `GET /jewelry_brand` - Main jewelry brand page (alternate route)
- `GET /original_index` - Original index page with catalog of accessories
- `GET /tryon/<path:file_path>` - Try on a specific item in the webcam interface
- `GET /checkout` - Access the checkout page
- `POST /upload_frame` - Process a frame with virtual try-on (REST API)
- `Socket.IO 'stream_frame'` - Process a frame via WebSockets (more efficient)
- `GET /api/docs` - API documentation

For full API documentation, see the `/api/docs` endpoint when the application is running.

## WebSocket API

The application uses Socket.IO for real-time communication:

- Client events:
  - `stream_frame` - Send a webcam frame with file_path to the server
  
- Server events:
  - `processed_frame` - Receive processed frame with item overlay
  - `no_face` - Notification when no face is detected
  - `stream_error` - Error information

## Security Features

- Secure file path handling to prevent path traversal attacks
- Input validation for all parameters
- Rate limiting to prevent abuse
- Proper error handling and logging
- Cross-Origin Resource Sharing (CORS) protection

## Development

### Code Style

This project follows PEP 8 style guidelines. To check your code:

```
flake8 .
```

### Adding New Accessories

To add new accessories:

1. Add image files to `static/images/`
2. The naming convention is important: `Category<number><id>.png` 
   - Categories: Necklace, Earrings
   - Number: Used to group items within a category
   - ID: Unique identifier for the specific item
3. Images must have transparent backgrounds (PNG with alpha channel)
4. Update the index.html file to include the new items in the appropriate tab

## Performance Considerations

- The webcam stream is throttled to 10 FPS to reduce CPU usage
- JPEG compression is used to reduce bandwidth usage in the WebSocket stream
- Face detection is the most CPU-intensive operation

## Browser Compatibility

- Chrome 74+
- Firefox 68+
- Edge 79+
- Safari 13+

Older browsers may not support the required WebRTC and WebSocket features.

## License

This project is licensed under the terms of the license included in the repository.

## Credits

- Face detection using dlib
- Image processing using OpenCV
- Web interface using Flask and Flask-SocketIO
- Bootstrap for responsive design
