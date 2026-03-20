# Car Number Plate Extraction

A three-step pipeline for car number plate extraction using classical computer vision techniques: Detection, Alignment, and OCR.

## Overview

This project implements a transparent and educational approach to number plate extraction without relying on deep learning. It uses geometric properties and classical OpenCV techniques to detect plates, correct their perspective, and extract text using Tesseract OCR.

**Author:** Gabriel Baziramwabo  
**Organization:** Benax Technologies Ltd · Rwanda Coding Academy

## Features

- **CPU-only design** - No GPU required, runs on ordinary laptops
- **Real-time processing** - Live camera feed processing
- **Geometric detection** - Contour-based plate detection
- **Perspective correction** - Plate alignment and rectification
- **OCR validation** - Pattern matching for plate format validation
- **Temporal confirmation** - Multiple frame confirmation for reliability
- **CSV logging** - Persistent storage of detected plates
- **Duplicate suppression** - Prevents repeated entries

## Project Structure

```
car-number-plate-extraction/
├── data/
│   ├── plates/          # Saved plate images
│   └── logs/            # Log files
├── src/
│   ├── camera.py        # Camera validation
│   ├── detect.py        # Plate detection
│   ├── align.py         # Plate alignment
│   ├── ocr.py           # OCR extraction
│   ├── validate.py      # Pattern validation
│   └── temporal.py      # Full pipeline with CSV logging
├── .gitignore           # Git ignore file
├── README.md            # This file
└── book/                # Project documentation
```

## Prerequisites

### 1. Python Environment
```bash
# Create virtual environment
python -m venv .venv

# Activate (Windows)
.venv\Scripts\activate

# Activate (Linux/Mac)
source .venv/bin/activate

# Install dependencies
pip install opencv-python numpy pytesseract
```

### 2. Tesseract OCR
Install Tesseract OCR engine:

**Windows:**
Download from: https://github.com/UB-Mannheim/tesseract/wiki

**macOS:**
```bash
brew install tesseract
```

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install tesseract-ocr
```

**Verify installation:**
```bash
tesseract --version
```

## Usage

Run the scripts in sequence to test each pipeline stage:

### 1. Camera Validation
```bash
python src/camera.py
```
- Opens webcam and displays live feed
- Press 'q' to quit

### 2. Plate Detection
```bash
python src/detect.py
```
- Detects rectangular plate-like regions
- Draws green boxes around candidates
- Shows "Plate detected" when found

### 3. Plate Alignment
```bash
python src/align.py
```
- Detects plates and corrects perspective
- Shows aligned plate in separate window
- Marks corner points with red circles

### 4. OCR Testing
```bash
python src/ocr.py
```
- Performs OCR on aligned plates
- Shows thresholded image for OCR
- Displays extracted text on screen

### 5. Pattern Validation
```bash
python src/validate.py
```
- Validates OCR results against pattern: `AAA111A`
- Shows "VALID: [plate]" for matches
- Shows "OCR: [text]" for raw results

### 6. Full Pipeline
```bash
python src/temporal.py
```
- Complete end-to-end system
- Temporal confirmation across frames
- CSV logging with timestamps
- Duplicate suppression (10-second cooldown)

## Plate Format

The system expects plates in the format: **3 letters + 3 digits + 1 letter**
- Example: `ABC123D`
- Regex pattern: `[A-Z]{3}[0-9]{3}[A-Z]`

## Output

When running the full pipeline (`temporal.py`):
- **Console:** Shows saved plate numbers
- **CSV file:** `plates_log.csv` with plate numbers and timestamps
- **Windows:** Live camera feed with detection results

## Technical Details

### Detection Algorithm
1. Convert frame to grayscale
2. Apply Gaussian blur
3. Edge detection using Canny
4. Find contours
5. Filter by area and aspect ratio
6. Keep rectangular candidates

### Alignment Process
1. Get four corners of detected rectangle
2. Order corners consistently
3. Compute perspective transform
4. Warp to normalized 450×140 pixel image

### OCR Pipeline
1. Convert aligned plate to grayscale
2. Apply blur and thresholding
3. Use Tesseract with whitelist
4. Validate against expected pattern

### Temporal Confirmation
- Maintains buffer of last 5 valid readings
- Uses majority vote for final confirmation
- 10-second cooldown between saves

## Limitations

- Works best in controlled environments
- Requires plates to appear approximately rectangular
- Performance depends on lighting conditions
- Designed for specific plate format (modify regex for other formats)

## Troubleshooting

### Camera Issues
- Check webcam permissions
- Try different camera indices (0, 1, 2...)
- Ensure camera is not in use by other applications

### OCR Issues
- Verify Tesseract installation and PATH
- Check lighting conditions
- Ensure plates are clearly visible
- Clean camera lens if needed

### Detection Issues
- Adjust `MIN_AREA` parameter for different distances
- Modify aspect ratio range (`AR_MIN`, `AR_MAX`)
- Improve lighting to enhance edge detection

## Contributing

This project is designed for educational purposes. Feel free to:
- Modify detection parameters for your environment
- Add support for different plate formats
- Implement additional validation logic
- Improve error handling

## License

This project is part of educational materials for Rwanda Coding Academy.

## References

- OpenCV Documentation
- Tesseract OCR Documentation
- Computer Vision: Algorithms and Applications - Richard Szeliski
- Digital Image Processing - Gonzalez & Woods