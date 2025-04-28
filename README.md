# MTurk Image Annotation Interface

This repository contains customizable HTML templates for creating image annotation interfaces specifically designed for Amazon Mechanical Turk (MTurk).

## Overview

These annotation templates provide a user-friendly interface for image selection and classification tasks on MTurk. Workers can:

- View images with accompanying instructions
- Select one or multiple images based on specific criteria
- Navigate between multiple annotation tasks
- Submit their selections with automatic data collection

## Templates

The repository includes two main template variants:

### 1. Single-Selection Template (`annotation-single.html`)
- Designed for verification/classification tasks
- Allows workers to select a single option from a set of choices
- Typically used for "yes/no/not sure" type tasks or label verification

### 2. Multi-Selection Template (`annotation-multi.html`)
- Designed for collection/curation tasks
- Allows workers to select multiple images (configurable minimum number)
- Ideal for gathering sets of images that match specific criteria

## MTurk Setup

### Input Format

MTurk requires a specific input format for these templates:

1. Create a CSV file with a **single column** titled `input_dict`
2. Each row in this column should contain a complete JSON string representing a HIT (task)
3. The JSON string should be properly escaped (use a JSON validator to check)

Example CSV structure:
```
input_dict
"{\"0\":{\"instruction_1\":\"Do you believe label of this image is <b>Dress</b>?\",\"example\":\"https://example.com/image.jpg\",\"options\":{\"Yes\":\"https://example.com/image.jpg\",\"Not_Sure\":\"https://example.com/image.jpg\",\"No\":\"https://example.com/image.jpg\"},\"class\":\"Dress\",\"color\":\"Blue\",\"material\":\"Silk\",\"pattern\":\"Houndstooth\"}}"
"{\"0\":{\"instruction_1\":\"Is this a <b>Red</b> shirt?\",\"example\":\"https://example.com/image2.jpg\",\"options\":{\"Yes\":\"https://example.com/image2.jpg\",\"Not_Sure\":\"https://example.com/image2.jpg\",\"No\":\"https://example.com/image2.jpg\"},\"class\":\"Shirt\",\"color\":\"Red\",\"material\":\"Cotton\",\"pattern\":\"Solid\"}}"
```

### Template Configuration

Each template is configured via a JavaScript object that defines:

1. Tasks and instructions
2. Image options
3. Selection requirements

Example configuration:

```javascript
input_dict = {
    '0': {
        'instruction_1': 'Do you believe label of this image is <b>Dress</b>?', 
        'example': 'https://example.com/image.jpg',
        'options': {
            'Yes': 'https://example.com/image.jpg', 
            'Not_Sure': 'https://example.com/image.jpg',
            'No': 'https://example.com/image.jpg'
        }, 
        'class': 'Dress', 
        'color': 'Blue', 
        'material': 'Silk', 
        'pattern': 'Houndstooth'
    },
    // Multiple tasks can be included in a single HIT
}
```

### Customization Options

- `minimum_selection`: Number of required selections (multi-selection template)
- `enable_option_capture`: Show/hide option labels beneath images
- `rows_count`: Number of images per row

### Testing Before Deployment

**IMPORTANT:** Always test your HITs in the MTurk Sandbox environment before spending real money!

1. **Local Testing Mode:**
   ```javascript
   // Local debug mode - Edit this for local testing
   input_dict = { // Your test configuration here }
   ```

2. **Sandbox Testing:**
   - Upload your HTML template to the MTurk Requester Sandbox
   - Create a small batch with your CSV input file
   - Preview the HIT to ensure proper rendering
   - Test the full workflow as a Worker would experience it

3. **For MTurk Production Deployment:**
   Uncomment and use these lines:
   ```javascript
   // Online mode
   input_dict = "${input_dict}"
   input_dict = JSON.parse(input_dict.replace(/'/g, '"'));
   ```

## Features

- Mobile-responsive design
- Configurable selection requirements
- Task navigation with progress tracking
- Time tracking for each task
- Automatic data formatting for submission
- Support for various image classification scenarios

## Output Format

Results are returned in a structured JSON format that includes:
- Selected options for each task
- Time spent on each task (in milliseconds)

## License

MIT License