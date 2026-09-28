# Structured LLM Output and Function Simulation

A practical exploration of prompt engineering for generating reliable structured outputs using Large Language Models. This project demonstrates how to guide an LLM to produce valid JSON, generate structured travel itineraries, and simulate logical function-based calculations.

The notebook uses a locally running Hugging Face FLAN-T5 model and focuses on improving output consistency through clear prompts, examples, validation, and low-temperature generation.

## Overview

Large Language Models typically generate natural language responses, but many real-world applications require structured and machine-readable outputs such as JSON.

This project explores techniques for controlling LLM outputs and making them easier to process programmatically.

The notebook covers three main tasks:

1. Basic JSON generation
2. Structured travel itinerary generation
3. Function simulation for order total calculations

The project also demonstrates why generated outputs should be parsed and validated before being used by an application.

## Objectives

The main objectives of this project are to:

* Understand structured output generation using LLMs
* Design prompts for predictable JSON responses
* Extract and parse JSON from model-generated text
* Validate generated JSON structures
* Generate structured travel itineraries
* Simulate function-based logical operations
* Compare LLM-generated calculations with Python calculations
* Understand the importance of temperature and prompt design
* Develop better prompt engineering practices

## Technology Stack

| Technology                | Purpose                                |
| ------------------------- | -------------------------------------- |
| Python                    | Programming language                   |
| Hugging Face Transformers | Loading and running the language model |
| FLAN-T5 Small             | Local text-to-text generation          |
| PyTorch                   | Deep learning framework                |
| JSON                      | Structured data representation         |
| Regular Expressions       | Extracting JSON from generated text    |
| Jupyter Notebook          | Development and experimentation        |

## Model

The project uses:

```text
google/flan-t5-small
```

The model is loaded using the Hugging Face Transformers pipeline:

```python
pipeline(
    "text2text-generation",
    model="google/flan-t5-small",
    device=-1
)
```

The model runs locally using the CPU.

## Project Workflow

The overall workflow can be represented as:

```text
User Input
    |
    v
Prompt Engineering
    |
    v
FLAN-T5 Model
    |
    v
Generated Output
    |
    v
Output Extraction
    |
    v
JSON Parsing
    |
    v
Validation
    |
    v
Structured Result
```

For mathematical operations, an additional verification step is included:

```text
Order Data
    |
    +----------------------+
    |                      |
    v                      v
Python Calculation       LLM Calculation
    |                      |
    +----------+-----------+
               |
               v
         Result Comparison
```

# Task 1: Basic JSON Generation

The first task demonstrates how an LLM can be prompted to generate structured JSON.

The example provides input information such as:

```text
name = Aayush
age = 20
city = Kochi
```

The prompt explicitly instructs the model to return only JSON.

Expected structure:

```json
{
  "name": "Aayush",
  "age": 20,
  "city": "Kochi"
}
```

## JSON Extraction

Because LLM output may contain additional text or imperfect formatting, the notebook includes a helper function to extract JSON from the generated response.

The extracted text is then passed to Python's JSON parser.

The output is considered valid only when the required fields are present.

Required fields include:

```text
name
age
city
```

## Validation

The generated response is checked for:

* Valid JSON syntax
* Presence of required fields
* Correct structured output format

This demonstrates an important principle in LLM applications:

**Generated output should be validated before being used by downstream code.**

# Task 2: Travel Itinerary JSON Generation

The second task applies structured output generation to a practical use case.

The model is asked to generate a travel itinerary for different destinations.

The notebook tests multiple destinations:

```text
Paris
Tokyo
Dubai
```

Each itinerary follows a predefined JSON structure.

Example:

```json
{
  "destination": "Paris",
  "duration_days": 3,
  "activities": [
    {
      "day": 1,
      "activity": "Visit Eiffel Tower",
      "location": "Eiffel Tower"
    },
    {
      "day": 2,
      "activity": "Visit Louvre Museum",
      "location": "Louvre Museum"
    },
    {
      "day": 3,
      "activity": "Walk along Champs-Elysees",
      "location": "Champs-Elysees"
    }
  ]
}
```

## Prompt Constraints

The prompt defines explicit rules for the generated output:

* Destination must match the requested destination
* Duration must match the requested number of days
* One activity should be generated for each day
* Day numbering should start from 1
* Each activity must contain the required fields
* Activities should be realistic for the destination
* Output must contain valid JSON
* No additional explanation should be generated

This demonstrates how detailed instructions and examples can improve structured output consistency.

# Task 3: Function Simulation

The third task demonstrates how an LLM can be guided to simulate a logical function.

The example calculates an order total using:

```text
Subtotal
Tax
Shipping
Total
```

The calculation follows:

```text
subtotal = sum(price × quantity)

tax = subtotal × tax_rate

total = subtotal + tax + shipping
```

The notebook contains both:

1. A Python implementation of the calculation
2. An LLM-based implementation

This allows the LLM-generated result to be compared with an actual Python calculation.

## Example Order

The test order contains:

```text
Laptop
Price: 50000
Quantity: 1

Mouse
Price: 1000
Quantity: 2

Keyboard
Price: 2000
Quantity: 1

Tax rate: 18%
Shipping: 500
```

The Python implementation calculates the expected result, which can then be compared with the LLM-generated JSON output.

## Why Verification Matters

LLMs are designed primarily for language generation and should not automatically be treated as reliable mathematical calculators.

For this reason, the notebook emphasizes verifying LLM-generated calculations using deterministic Python code.

A robust application can use an LLM to understand the user's request while allowing traditional program logic to perform the actual calculation.

# Prompt Engineering Techniques

The notebook demonstrates several important prompt engineering techniques.

## Explicit Output Format

The prompt clearly specifies the expected JSON structure.

## Examples

Examples are provided to show the model exactly how the desired output should look.

## Output Restrictions

The model is instructed to:

```text
Return ONLY valid JSON.
Do not write explanations.
Do not use Markdown.
```

These instructions reduce unnecessary text around the structured output.

## Low Temperature

The generation function uses a low temperature:

```python
temperature=0.2
```

Lower temperature is useful for structured tasks because deterministic and consistent outputs are generally preferred over highly creative responses.

## Output Validation

Generated responses are parsed and checked before being accepted.

This is particularly important when integrating LLMs into software applications.

# Key Learnings

This project demonstrates that structured LLM applications require more than simply generating a prompt.

Important practices include:

* Clearly define the expected output format
* Provide examples when appropriate
* Use consistent field names
* Keep temperature low for structured tasks
* Extract generated structured data carefully
* Validate JSON before using it
* Test prompts with multiple inputs
* Verify mathematical operations using deterministic code
* Do not blindly trust generated outputs

# Installation

Install the required dependencies:

```bash
pip install "transformers>=4.41,<5.0" sentencepiece
```

The notebook also uses PyTorch.

If required, install it using:

```bash
pip install torch
```

## Running the Notebook

Clone the repository:

```bash
git clone <your-repository-link>
```

Navigate to the project directory:

```bash
cd <repository-name>
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Open:

```text
structured-output.ipynb
```

Run the cells sequentially.

The first execution may take additional time because the Hugging Face model needs to be downloaded.

# Project Structure

```text
Structured-LLM-Output/
|
├── structured-output.ipynb
└── README.md
```

# Practical Applications

The concepts demonstrated in this project can be applied to many real-world AI applications, including:

* API response generation
* Data extraction
* Automated form processing
* Travel planning systems
* E-commerce workflows
* Database operations
* Information extraction
* AI agents
* Workflow automation
* Structured chatbot responses
* Function calling systems

# Limitations

This project uses a relatively small language model and simple prompts.

The notebook does not implement production-level structured output mechanisms such as:

* JSON schema enforcement
* Pydantic validation
* Native tool calling
* Function calling APIs
* Production API deployment
* Large-scale evaluation
* Error recovery pipelines

These can be explored as extensions of this project.

# Future Improvements

Possible improvements include:

* Add Pydantic-based validation
* Implement JSON schema validation
* Add automatic retry when invalid JSON is generated
* Compare multiple open-source LLMs
* Experiment with different temperature settings
* Add more complex function simulations
* Implement native function calling
* Connect structured outputs to external APIs
* Build an interactive application around the pipeline
* Add automated evaluation of output quality

# Conclusion

This project provides a practical introduction to structured LLM outputs and prompt engineering.

By using Hugging Face FLAN-T5, carefully designed prompts, JSON extraction, validation, and deterministic Python calculations, the notebook demonstrates how language models can be integrated into applications that require predictable and machine-readable outputs.

The key takeaway is that reliable LLM applications combine language-model capabilities with traditional software engineering practices such as validation, structured data handling, and deterministic computation.

# Author

Aayush Kumar
