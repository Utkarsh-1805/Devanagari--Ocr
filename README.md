"# Devanagari OCR - Handwritten Text Recognition

A deep learning-based Optical Character Recognition (OCR) system for handwritten Devanagari script. This project implements a complete pipeline for recognizing handwritten Hindi/Devanagari text from images.

## 📄 Research Paper

📎 **Google Drive Link:** [Add your research paper link here]

<!-- Example: https://drive.google.com/file/d/YOUR_FILE_ID/view?usp=sharing -->

---

## 🎯 Project Overview

This OCR system processes handwritten Devanagari text images through multiple stages:

1. **Image Preprocessing** - Enhancement and stroke darkening
2. **Sentence Cropping** - Safe margin extraction
3. **Word Segmentation** - Isolating individual words
4. **Shirorekha (Header Line) Removal** - Detecting and removing the top horizontal line
5. **Matra (Vowel Diacritic) Extraction** - Identifying upper matras (े, ै, र्)
6. **Character Segmentation** - Extracting individual characters below the shirorekha
7. **Character & Matra Prediction** - CNN-based classification
8. **Word Reconstruction** - Combining characters with matras using proper Unicode composition

## 🏗️ Architecture

### Models Used
- **Character Recognition CNN** (`characters.keras`) - Classifies 36+ Devanagari consonants and vowels
- **Matra Recognition CNN** (`matras.keras`) - Classifies upper matras (े, ै, र्)

### Supported Characters
- **Consonants:** क, ख, ग, घ, ङ, च, छ, ज, झ, ञ, ट, ठ, ड, ढ, ण, त, थ, द, ध, न, प, फ, ब, भ, म, य, र, ल, व, श, ष, स, ह
- **Conjuncts:** क्ष, त्र, ज्ञ
- **Vowels:** अ, इ, उ, ऊ, ए
- **Matras:** ा (aa), े (e), ै (ai), र् (repha)

## 🛠️ Technologies Used

- **Python 3.x**
- **TensorFlow / Keras** - Deep learning framework
- **OpenCV** - Image processing
- **NumPy** - Numerical computations
- **Matplotlib** - Visualization
- **scikit-learn** - Label encoding
- **joblib** - Model serialization

## 📦 Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/devanagari-ocr.git
   cd devanagari-ocr
   ```

2. **Install dependencies:**
   ```bash
   pip install tensorflow opencv-python numpy matplotlib scikit-learn joblib tqdm pandas
   ```

3. **Download pre-trained models:**
   - `characters.keras` - Character recognition model
   - `matras.keras` - Matra recognition model
   - `character_label_encoder.pkl` - Character label encoder
   - `matra_label_encoder.pkl` - Matra label encoder

## 🚀 Usage

### Running the OCR Pipeline

```python
# 1. Load and preprocess the image
input_path = "path/to/your/image.jpg"
processed_img = enhance_and_darken_strokes_updated(input_path)
cropped_img = crop_sentence_safe_margin(processed_img, padding=10)

# 2. Segment words
word_imgs = segment_words_thicker_strokes(cropped_img, show_steps=True)

# 3. Remove shirorekha and extract components
cleaned_words, shirorekha_positions = remove_shirorekha_from_words(word_imgs)

# 4. Reconstruct the sentence
sentence = reconstruct_sentence(cleaned_words, shirorekha_positions)
print(sentence)
```

### Example Output
```
Input Image: Handwritten "मेरा नाम जोकर"
OCR Output: मेरा नाम जोकर
```

## 📁 Project Structure

```
devanagari-ocr/
├── devnagri_ocr.ipynb      # Main Jupyter notebook with full pipeline
├── Devanagari.pdf          # Research paper/documentation
├── characters.keras        # Trained character CNN model
├── matras.keras           # Trained matra CNN model
├── character_label_encoder.pkl
├── matra_label_encoder.pkl
├── README.md
└── .gitignore
```

## 🔧 Key Functions

| Function | Description |
|----------|-------------|
| `enhance_and_darken_strokes_updated()` | Enhances image contrast and darkens strokes |
| `crop_sentence_safe_margin()` | Crops the text region with safe padding |
| `segment_words_thicker_strokes()` | Segments image into individual words |
| `remove_shirorekha_from_words()` | Detects and removes the header line |
| `extract_matras_after_shirorekha_removal()` | Extracts upper matras above shirorekha |
| `segment_characters_below_shirorekha()` | Segments characters below the header line |
| `predict_single_character()` | Predicts a single character using CNN |
| `predict_single_matra()` | Predicts a single matra using CNN |
| `reconstruct_word()` | Combines characters and matras into words |
| `reconstruct_sentence()` | Combines words into a full sentence |

## 📊 Pipeline Visualization

```
Image Input
    │
    ▼
┌─────────────────────┐
│ Image Enhancement   │
│ & Stroke Darkening  │
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ Sentence Cropping   │
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ Word Segmentation   │
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ Shirorekha Removal  │
└─────────────────────┘
    │
    ├─────────────────────┐
    ▼                     ▼
┌──────────────┐   ┌──────────────┐
│ Matra        │   │ Character    │
│ Extraction   │   │ Segmentation │
└──────────────┘   └──────────────┘
    │                     │
    ▼                     ▼
┌──────────────┐   ┌──────────────┐
│ Matra CNN    │   │ Character    │
│ Prediction   │   │ CNN Prediction│
└──────────────┘   └──────────────┘
    │                     │
    └──────────┬──────────┘
               ▼
    ┌─────────────────────┐
    │ Word Reconstruction │
    │ (Unicode Composition)│
    └─────────────────────┘
               │
               ▼
         OCR Output
```

## 🎓 How It Works

### Shirorekha Detection
The shirorekha (शिरोरेखा) is the horizontal line that connects Devanagari characters at the top. The system uses horizontal projection to detect and remove it, separating matras from base characters.

### Matra Handling
- **Upper Matras** (े, ै): Detected above the shirorekha
- **Repha** (र्): Detected as a special prefix matra
- **AA Matra** (ा): Detected geometrically as a tall, thin vertical stroke

### Unicode Composition
The system correctly handles composite matras:
- े + ा → ो (e + aa = o)
- ै + ा → ौ (ai + aa = au)

## 📈 Future Improvements

- [ ] Support for more matras (ि, ी, ु, ू, etc.)
- [ ] Half-letter (halant) recognition
- [ ] Improved conjunct character support
- [ ] Real-time camera-based OCR
- [ ] Mobile app integration

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👥 Authors

- Your Name - [GitHub Profile](https://github.com/your-username)

## 🙏 Acknowledgments

- Devanagari Handwritten Character Dataset
- TensorFlow/Keras team for the deep learning framework
- OpenCV community for image processing tools

---

⭐ **If you find this project useful, please give it a star!**
" 
