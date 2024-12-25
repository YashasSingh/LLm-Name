# LLm-Name
# Markov Chain-Based Name Generator

This project implements a name generator using a Markov chain trained on historical name data. The program processes data files, builds a Markov model for generating names, and supports advanced features like biasing name patterns and applying constraints on name length, gender, and prefixes.

## Features

- **Data Preprocessing**: Combines multiple datasets, removes duplicates, and aggregates counts for each unique name.
- **Markov Chain Model**: Generates names based on probabilistic transitions between characters.
- **Gender-Specific Names**: Generates names for males and females based on the input data.
- **Custom Constraints**:
  - Specify minimum and maximum name lengths.
  - Add prefixes to generated names.
  - Bias results toward rare or popular patterns.
- **Multithreaded Generation**: Efficiently generate multiple names concurrently.
- **Error Handling and Retry Mechanisms**: Ensures constraints are met or relaxes them gradually to generate valid names.

## Prerequisites

- Python 3.8+
- Required Libraries:
  - `concurrent.futures`
  - `collections`
  - `random`
  - `os`

## Setup

1. Place your data files (e.g., `yob1880.txt`, `yob1881.txt`, ...) in the working directory. Each file should contain names in the following format:

   ```
   Name,Gender,Count
   Example,M,123
   Example2,F,456
   ```

2. Run the Python script to preprocess the data and generate the Markov model.

## Usage

### Preprocessing Data

The program combines and processes name files:

```python
combine_and_filter_files("data_directory", "combined_filtered_aggregated_names.txt")
```

### Building the Markov Model

Build a Markov chain model based on the processed data:

```python
markov_model = build_markov_model("combined_filtered_aggregated_names.txt")
```

### Generating Names

Generate names with customizable constraints:

```python
names = generate_names(
    count=5,          # Number of names to generate
    gender="M",      # Gender: 'M' or 'F'
    min_length=4,     # Minimum name length
    max_length=8,     # Maximum name length
    prefix="Jo",     # Optional prefix
    bias_factor=0.8,  # Favor rare patterns (low value) or popular ones (high value)
    threads=4         # Number of threads for parallel generation
)
print(names)
```

### Example Output

```plaintext
Generating names:
Male names with prefix 'Jo' (bias rare patterns, length 4-8):
['Jory', 'Jonah', 'Jovan', 'Jordi', 'Jordan']

Female names (favor popular patterns, length 5-10):
['Sophia', 'Emma', 'Olivia', 'Ava', 'Mia']
```

## Error Handling

- If valid names cannot be generated within 100 attempts, the program gradually relaxes constraints (e.g., adjusts `min_length` and `max_length`).
- Errors are logged with detailed information about the failed attempts.

## Advanced Features

1. **Bias Adjustment**:
   - Control the weighting of transitions in the Markov model using the `bias_factor` parameter.
   - `bias_factor > 1.0`: Favor popular patterns.
   - `bias_factor < 1.0`: Favor rare patterns.

2. **Prefix Support**:
   - Specify a prefix to generate names that start with the given sequence.

3. **Dynamic Relaxation**:
   - Automatically adjusts constraints to ensure successful generation.

## Limitations

- Requires a sufficiently large and diverse dataset to generate meaningful results.
- Constraints that are too restrictive may cause retries or failure.

## Future Improvements

- Add support for multilingual name generation.
- Enhance the Markov model with higher-order transitions.
- Provide a user-friendly interface for customization and visualization.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

