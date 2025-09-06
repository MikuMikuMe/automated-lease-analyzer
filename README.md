# automated-lease-analyzer

Creating an automated lease analyzer is a complex task as it involves multiple technologies such as OCR (Optical Character Recognition), NLP (Natural Language Processing), and data visualization. Below is a simplified version of a Python program to provide a basic structure for such an application. This is only a starting point and will require further enhancement for a production-level system.

```python
import pytesseract
from PIL import Image
import spacy
import matplotlib.pyplot as plt
from wordcloud import WordCloud
import os

# Ensure the tesseract executable is located at the path specified below
# pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe' # Windows example

def perform_ocr(image_path):
    """
    Perform OCR on a given image to extract text.
    
    :param image_path: Path to the image file
    :return: Extracted text
    """
    try:
        img = Image.open(image_path)
        text = pytesseract.image_to_string(img)
        return text
    except Exception as e:
        print(f"Error performing OCR on image: {e}")
        return None

def analyze_text(text):
    """
    Perform NLP analysis on text to extract key information.
    
    :param text: Extracted text from OCR
    :return: Named entities recognized in the text
    """
    try:
        nlp = spacy.load("en_core_web_sm")
        doc = nlp(text)
        return [(ent.text, ent.label_) for ent in doc.ents]
    except Exception as e:
        print(f"Error during NLP analysis: {e}")
        return []

def visualize_results(entities):
    """
    Create a simple visualization of the NLP results using a word cloud.
    
    :param entities: Named entities extracted from text
    """
    try:
        entity_text = " ".join([ent[0] for ent in entities])
        wordcloud = WordCloud(width=800, height=400).generate(entity_text)

        plt.figure(figsize=(10, 5))
        plt.imshow(wordcloud, interpolation='bilinear')
        plt.axis('off')
        plt.show()
    except Exception as e:
        print(f"Error creating visualization: {e}")

def analyze_lease(image_path):
    """
    High-level function to perform the entire lease analysis process.
    
    :param image_path: Path to the lease image
    """
    print("Starting lease analysis...")

    text = perform_ocr(image_path)
    if text:
        print("OCR successful. Text extracted from image.")
        entities = analyze_text(text)
        print("NLP analysis complete. Named entities extracted:")
        for ent in entities:
            print(f" - {ent[0]} ({ent[1]})")

        visualize_results(entities)

if __name__ == '__main__':
    # Example usage
    image_path = 'path/to/lease_image.jpg'  # Replace with your lease image path

    if not os.path.exists(image_path):
        print(f"Image path {image_path} does not exist.")
    else:
        analyze_lease(image_path)
```

### Key Components and Considerations:

1. **OCR with Tesseract**: The `perform_ocr` function uses `pytesseract` to perform OCR on images. Ensure Tesseract OCR is properly installed and its path is correctly set.
   
2. **NLP with SpaCy**: The `analyze_text` function uses SpaCy to identify named entities. It requires the English model (`en_core_web_sm`) to be downloaded (`python -m spacy download en_core_web_sm`).

3. **Data Visualization**: The `visualize_results` function uses `wordcloud` to visually represent the frequency of extracted entities.

4. **Error Handling**: Basic error handling is included to capture and display errors gracefully.

### Additional Considerations:

- To extend this script for practical use, refine the OCR and NLP parts to extract specific lease terms.
- The visualization part can be expanded to include more useful comparative metrics.
- Consider using more advanced NLP techniques to further parse and analyze lease agreements.
- Security and privacy considerations should be addressed to ensure sensitive data in leases is protected.