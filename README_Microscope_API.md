# Microscope Image Processing API

A Flask-based API for processing microscope images with OpenCV, enabling image stitching, ROI selection, zoom functionality, and auto-focus simulation.

## Features

1. **Image Stitching** - Combines multiple microscope images into a single high-resolution image.
2. **ROI Selection** - Extracts a specific Region of Interest (ROI) from an image for detailed analysis.
3. **Zoom Functionality** - Applies digital zoom (10X, 20X) to magnify extracted ROIs.
4. **Auto-Focus Simulation** - Enhances image clarity using contrast-based sharpening techniques.

## Prerequisites

- Python 3.7 or higher
- pip (Python package manager)

## Installation

1. Clone this repository or download the source code.

2. Navigate to the project directory:
   ```
   cd api
   ```

3. Install the required dependencies:
   ```
   pip install -r requirements.txt
   ```

4. Run the Flask application:
   ```
   python app.py
   ```

5. Open your browser and navigate to:
   ```
   http://localhost:5000
   ```

## Project Structure

- `api/app.py` - Main Flask application with API endpoints
- `api/image_processing.py` - OpenCV-based image processing functions
- `api/templates/index.html` - Web interface for the API
- `api/uploads/` - Directory for storing uploaded and processed images
- `api/static/` - Directory for static assets
- `api/samples/` - Sample images and Jupyter notebook with API usage examples
- `api/Documentation.md` - Detailed documentation of the API
- `api/README.md` - API overview and setup instructions

## API Endpoints

### 1. Upload Images
- **Endpoint:** `/upload_images`
- **Method:** `POST`
- **Description:** Upload multiple microscope images for processing.

### 2. Stitch Images
- **Endpoint:** `/stitch_images`
- **Method:** `POST`
- **Description:** Stitch overlapping images into one seamless high-resolution image.

### 3. ROI Selection
- **Endpoint:** `/roi_selection`
- **Method:** `POST`
- **Description:** Extract a Region of Interest (ROI) based on user input.

### 4. Zoom
- **Endpoint:** `/zoom`
- **Method:** `POST`
- **Description:** Magnify the selected ROI (10X or 20X).

### 5. Auto-Focus
- **Endpoint:** `/auto_focus`
- **Method:** `POST`
- **Description:** Enhance image sharpness using contrast-based sharpening.

### 6. Get Image
- **Endpoint:** `/get_image/<filename>`
- **Method:** `GET`
- **Description:** Retrieve an uploaded or processed image.

## Implementation Details

### Image Stitching
The application uses OpenCV's stitching module with ORB feature detection for identifying key points and matching them across images. Homography transformation aligns images correctly, and blending techniques create seamless transitions.

### ROI Selection
The API allows precise extraction of regions of interest using (x, y, width, height) coordinates, validating input to ensure coordinates fall within image boundaries.

### Digital Zoom
Digital zoom functionality is implemented using bicubic interpolation to ensure high-quality results when magnifying ROIs at 10X or 20X.

### Auto-Focus Simulation
Auto-focus is simulated by:
1. Assessing image sharpness using Laplacian variance
2. Applying unsharp masking for edge enhancement
3. Using LAB color space transformations for better perceptual quality
4. Adjusting contrast to improve visibility

## Using the API

### Web Interface
The easiest way to interact with the API is through the provided web interface. Simply open a browser and navigate to `http://localhost:5000` after starting the Flask application.

### Programmatic Access
You can also interact with the API programmatically. See `api/samples/api_examples.ipynb` for a Jupyter notebook with examples of how to use the API with Python.

### cURL Examples

**1. Upload Images**
```bash
curl -X POST -F "files=@sample1.jpg" -F "files=@sample2.jpg" http://localhost:5000/upload_images
```

**2. Stitch Images**
```bash
curl -X POST -H "Content-Type: application/json" -d '{"filenames": ["unique_id_sample1.jpg", "unique_id_sample2.jpg"]}' http://localhost:5000/stitch_images
```

**3. ROI Selection**
```bash
curl -X POST -H "Content-Type: application/json" -d '{"filename": "unique_id_sample1.jpg", "roi": {"x": 100, "y": 100, "width": 200, "height": 200}}' http://localhost:5000/roi_selection
```

**4. Zoom**
```bash
curl -X POST -H "Content-Type: application/json" -d '{"filename": "unique_id_sample1.jpg", "zoom_factor": 10}' http://localhost:5000/zoom
```

**5. Auto-Focus**
```bash
curl -X POST -H "Content-Type: application/json" -d '{"filename": "unique_id_sample1.jpg"}' http://localhost:5000/auto_focus
```

## Documentation

For detailed documentation of the API, implementation details, and troubleshooting information, see `api/Documentation.md`.

## Challenges and Solutions

1. **Challenge**: Stitching images with limited overlapping features
   **Solution**: Implemented custom stitching with ORB feature detection and tunable parameters

2. **Challenge**: Maintaining image quality during digital zoom
   **Solution**: Used bicubic interpolation for smoother results

3. **Challenge**: Determining appropriate auto-focus parameters
   **Solution**: Implemented adaptive sharpening based on blur detection 