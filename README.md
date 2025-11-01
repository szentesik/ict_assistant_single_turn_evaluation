# ICT Assistant Single-Turn Prompt Evaluator

A comprehensive evaluation framework for testing an ICT Assistant AI chatbot specialized in CSTA (Computer Supported Telecommunications Applications) standards. This project uses [promptfoo](https://promptfoo.dev/) to evaluate the chatbot's accuracy, correctness, and consistency across a curated set of CSTA-related questions.

## Summary

This project provides an automated testing suite that evaluates how well an ICT Assistant chatbot responds to questions about CSTA standards. The evaluator tests the chatbot's ability to provide accurate, factually correct answers by comparing responses against ground truth data. It uses LLM-based rubric evaluation to assess response quality and accuracy.

## Features

- **Automated Evaluation**: Runs comprehensive tests against a dataset of CSTA-related questions
- **LLM-Based Rubric**: Uses Google Gemini 2.5 Flash to evaluate response correctness and factual accuracy
- **HTTP API Integration**: Connects to your ICT Assistant chatbot via HTTP API
- **HTML Reports**: Generates detailed HTML evaluation reports with test results
- **CSTA-Specific Test Cases**: Includes 21+ curated test cases covering various CSTA concepts including:
  - CSTA standards and specifications (ECMA-269, ECMA-323, ECMA-348, etc.)
  - CSTA devices, connections, and services
  - Call control events and system statuses
  - XML and ASN.1 representations
  - Implementation details (TCP sockets, SIP sessions)
  - Out-of-scope question handling

## Requirements

- **Node.js** (v20 or higher recommended)
- **npm** or **yarn** package manager
- **promptfoo** CLI tool
- **ICT Assistant API** running on `http://localhost:3000/api/chat`
- **Google Gemini API Key** (for LLM-based evaluation)

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/szentesik/ict_assistant_single_turn_evaluation.git
   cd ict_assistant_single_turn_evaluation
   ```

2. **Install promptfoo globally (if not already installed)**:
   ```bash
   npm install -g promptfoo
   ```
   
   Or using yarn:
   ```bash
   yarn global add promptfoo
   ```

3. **Set up environment variables** (if needed):
   Create a `.env` file in the project root with your API keys:
   ```env
   GOOGLE_API_KEY=your_google_api_key_here
   ```

## Quick Start

1. **Ensure your ICT Assistant chatbot is running**:
   The evaluator expects the chatbot API to be available at `http://localhost:3000/api/chat`. Make sure your chatbot server is running and accessible at this endpoint.

2. **Run the evaluation**:
   ```bash
   promptfoo eval
   ```

3. **View the results**:
   After evaluation completes, open `output/latest_results.html` in your web browser to view detailed test results.

## Usage

### Basic Evaluation

Run all tests defined in `datasets.yaml`:

```bash
promptfoo eval
```

Output file can be customized with *--output* parameter:

```bash
promptfoo eval --output output/results-$(Get-Date -Format "yyyyMMdd_hhmmss").html
```

### View Results

Run `promptfoo view` to use the local web viewer, or
open the HTML report generated at `output/latest_results.html`.

### Configuration

The evaluation is configured via `promptfooconfig.yaml`. Key configuration options:

- **Provider**: HTTP endpoint to your ICT Assistant chatbot
- **Test Dataset**: Questions and ground truth in `datasets.yaml`
- **Evaluation Method**: LLM-rubric based evaluation using Google Gemini
- **Output Path**: Results saved to `output/latest_results.html`

### Customizing Tests

Edit `datasets.yaml` to add, modify, or remove test cases. Each test case includes:
- `question`: The input question to test
- `ground_truth`: The expected correct answer
- Optional custom assertions (see last test case example for out-of-scope questions)

Example test case structure:
```yaml
- vars:
    question: "What is CSTA?"
    ground_truth: |
      CSTA (Computer Supported Telecommunications Applications) is a standard...
```

### Evaluation Criteria

The evaluator uses an LLM-based rubric to assess responses. A response is considered correct if:
1. It contains the key information from the ground truth
2. It doesn't contradict the ground truth
3. It provides accurate information even if phrased differently

## Project Structure

```
ict_assistant_single_turn_evaluation/
├── promptfooconfig.yaml      # Main promptfoo configuration
├── datasets.yaml              # Test cases with questions and ground truth
├── output/                    # Generated evaluation reports
│   └── latest_results.html   # Latest evaluation results
├── .gitignore                # Git ignore rules
└── README.md                 # This file
```

## Test Coverage

The test suite includes questions covering:

- **CSTA Fundamentals**: Definitions, standards, and specifications
- **Core Concepts**: Devices, connections, and identifiers
- **Services**: Answer Call, Clear Connection, Deflect Call, Make Call, etc.
- **Events**: Call control events (Bridged, Call Cleared, Established, etc.)
- **Formats**: XML and ASN.1 representations
- **Implementation**: TCP socket communication, SIP session establishment
- **Edge Cases**: Out-of-scope questions (non-CSTA topics)

## Troubleshooting

### Connection Errors

If you see connection errors, verify:
- The ICT Assistant API is running on `http://localhost:3000/api/chat`
- The API accepts POST requests with JSON payload
- The API response format matches the expected structure (should have an `answer` field in JSON response)

### API Key Issues

If evaluation fails with API key errors:
- Ensure your Google API key is set in environment variables
- Check that the key has access to Gemini 2.5 Flash model
- Verify the API key has sufficient quota

### Test Failures

If tests are failing:
- Review the HTML report to see detailed comparison between responses and ground truth
- Check if the chatbot API response format matches expectations
- Verify the ground truth data in `datasets.yaml` is accurate
- Adjust evaluation criteria in `promptfooconfig.yaml` if needed

## Contributing

To add new test cases:
1. Edit `datasets.yaml`
2. Add a new test case following the existing format
3. Include both the question and accurate ground truth
4. Run `promptfoo eval` to verify the new test case


## References

- [promptfoo Documentation](https://promptfoo.dev/docs)
- [CSTA Standards (ECMA-269)](https://www.ecma-international.org/publications-and-standards/standards/ecma-269/)
- [CSTA-XML (ECMA-323)](https://www.ecma-international.org/publications-and-standards/standards/ecma-323/)
