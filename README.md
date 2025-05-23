# Ukrainian Text Pseudonymizer

A Python library for pseudonymizing Ukrainian text using the Presidio analyzer framework with a Ukrainian NER model. This tool can identify and anonymize various types of entities in Ukrainian text, including:

- Person names
- Job titles
- Locations
- Organizations
- Date/Time expressions
- Email addresses
- Credit card numbers
- URLs
- Phone numbers

## Requirements

- Python 3.8
- Git LFS (for model download)

## Installation

1. Install the package using `uv`:
```bash
uv pip install pseudonymizer-uk
```

2. Install Git LFS (required for model download):
```bash
git lfs install
```

3. Download the Ukrainian NER model:
```bash
git clone https://huggingface.co/dchaplinsky/uk_ner_web_trf_13class
```

## Usage

```python
from pseudonymizer_uk import UkPseudonymizer

# Initialize the pseudonymizer with the path to the downloaded model
pseudonymizer = UkPseudonymizer(path_to_model="./uk_ner_web_trf_13class")

# Pseudonymize text
text = "Іван Франко народився в селі Нагуєвичі"
anonymized_text = pseudonymizer.pseudonymize(text)
```

## Supported Entity Types

By default, the pseudonymizer recognizes the following entity types:
- PERSON
- JOB
- LOCATION
- ORGANIZATION
- DATE_TIME

You can customize which entities to recognize by passing the `entities` parameter:

```python
pseudonymizer = UkPseudonymizer(
    path_to_model="uk_ner_web_trf_13class",
    entities=['PERSON', 'LOCATION']  # Only recognize persons and locations
)
```

## Custom Recognizers and Operators

You can extend the functionality by adding custom recognizers and operators:

```python
from presidio_analyzer import EntityRecognizer
from presidio_anonymizer import OperatorConfig

# Add custom recognizer
pseudonymizer.add_custom_recognizer(your_custom_recognizer)

# Add custom operator
pseudonymizer.add_custom_operator(
    "CUSTOM_ENTITY",
    OperatorConfig("custom", {"param": "value"})
)
```

## Development

To set up the development environment:

1. Clone the repository:
```bash
git clone https://github.com/fox-rudie/pseudonymizer-uk.git
cd pseudonymizer-uk
```

2. Create a virtual environment and install dependencies using `uv`:
```bash
uv venv
source .venv/bin/activate  # On Unix/macOS
# or
.venv\Scripts\activate  # On Windows

uv pip install -e ".[dev]"
```

3. Install pre-commit hooks (optional):
```bash
uv pip install pre-commit
pre-commit install
```

## Publishing

To publish a new version to PyPI:

1. Update version in `pyproject.toml` and `__init__.py`

2. Build the package:
```bash
uv pip install build
python -m build
```

3. Upload to PyPI:
```bash
uv pip install twine
python -m twine upload dist/*
```

## License

MIT License