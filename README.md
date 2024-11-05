# AWS Lambda Image Resizer

This AWS Lambda function resizes images uploaded to a source S3 bucket and saves the resized versions in a destination S3 bucket.

## Features

- **Automatic Resizing**: Resizes images uploaded to the source bucket to a specified width.
- **Supported Formats**: Works with `.jpg`, `.jpeg`, and `.png` formats.
- **AWS S3 Storage**: Stores the resized images in a specified destination bucket.

## Requirements

- **AWS Account** with access to S3 and Lambda services.
- **Node.js** installed on your local environment.
- **AWS SDK v3** and **sharp** library for image processing.

## Quick Start

### 1. Set Up Your Environment

#### Clone This Project

Clone this repository or create a new folder for the project:

```bash
git clone <repository-url>
cd <project-folder>
```

#### Install Dependencies

Install the required libraries:

```bash
npm install @aws-sdk/client-s3 sharp
```

#### Configure Environment Variables

Add the following environment variable to your Lambda function:

- `DEST_BUCKET`: The name of the S3 bucket where resized images will be stored.

### 2. Code Overview

The function performs these main steps:

1. **Extract Event Data**: Fetches the bucket name and file key from the S3 event triggered by an upload.
2. **Validate Image Format**: Checks that the uploaded file is in a supported format (`.jpg`, `.jpeg`, or `.png`).
3. **Download and Resize Image**: Retrieves the original image, resizes it using `sharp`, and converts it to a buffer.
4. **Upload Resized Image**: Saves the resized image in the specified destination S3 bucket.

### 3. Configure IAM Role Permissions

The Lambda function needs an IAM role with permissions for:

- `s3:GetObject` and `s3:PutObject` for the source and destination buckets.

### 4. Deploy the Lambda Function

#### Zip the Code

Package your code and dependencies into a zip file:

```bash
zip -r lambda-resizer.zip .
```

#### Upload to AWS Lambda

1. Go to the [AWS Lambda Console](https://console.aws.amazon.com/lambda/).
2. Create a new Lambda function.
3. Set the runtime to **Node.js 18.x** (or compatible version).
4. Upload the `lambda-resizer.zip` file.
5. Set the handler to `index.handler` (if your file is named `index.js`).

### 5. Set Up S3 Event Trigger

1. Open the source S3 bucket in the [AWS S3 Console](https://s3.console.aws.amazon.com/s3/).
2. Go to **Properties** > **Event notifications**.
3. Add an event notification for `PUT` events.
4. Choose your Lambda function as the event destination.

## Usage

Upload an image to the source bucket, and the Lambda function will:

1. Resize the image to a width of 200 pixels.
2. Save the resized image in the destination bucket.

## Troubleshooting

- **Permission Issues**: Ensure the IAM role attached to your Lambda function has `s3:GetObject` and `s3:PutObject` permissions for the source and destination buckets.
- **Unsupported File Type**: The function only supports `.jpg`, `.jpeg`, and `.png` formats.
- **Error Logs**: Check the function's logs in Amazon CloudWatch for detailed error messages.
