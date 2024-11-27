# RunPod Initialization Notebook

This Jupyter notebook automates the process of creating, configuring, and connecting to a RunPod instance. It handles everything from pod creation to environment setup and file transfer.

## Prerequisites

- RunPod API key stored in an `.env` file on your Desktop
- Python environment with the following packages:
  - runpod
  - python-dotenv
  - pyperclip (v1.9.0)
- SSH key pair (using ed25519) configured in `~/.ssh/`
- A `runpod_files` directory on your Desktop containing files to be transferred
- A `requirements.txt` file in the `runpod_files` directory

## Features

### 1. Initial Setup
- Loads environment variables from `.env` file
- Configures RunPod API key
- Sets up file paths for data transfer

### 2. Pod Creation
- Creates a pod with the following specifications:
  - PyTorch 2.1.0 image with CUDA 11.8.0
  - NVIDIA RTX A4500 GPU
  - 30GB container disk
  - 130GB volume
  - Exposed ports: 8888 (HTTP) and 22 (TCP)
  - Jupyter notebook enabled
  - Location: France (EU)

### 3. SSH Connection
- Automatically handles SSH connection setup
- Accepts host key for first-time connection
- Configures secure remote access

### 4. File Transfer
- Copies files from local `runpod_files` directory to pod's `/workspace` directory
- Supports copying files back from pod to local machine (commented section available)

### 5. Environment Setup
- Creates a Python virtual environment on the pod
- Installs and configures Jupyter kernel
- Installs dependencies from requirements.txt
- Installs additional packages (e.g., flash-attn)

### 6. Jupyter Access
- Retrieves and formats Jupyter notebook URL
- Automatically copies URL to clipboard for easy access

### 7. Pod Management
- Includes functions to list all pods
- Contains commented code for pod termination

## Usage

1. Ensure all prerequisites are met
2. Run the notebook cells sequentially
3. Wait for the pod to initialize (~90 seconds)
4. Environment setup takes approximately 3 minutes
5. Access Jupyter notebook using the automatically generated URL

## Important Notes

- The pod will continue running (and charging) until explicitly terminated
- Use the termination cell (currently commented out) to stop the pod when finished
- All files are mounted at `/workspace` in the pod
- The notebook uses SSH key-based authentication for security

## Environment Variables

Required in the `.env` file:


## Security Considerations

- Uses SSH key-based authentication
- Supports EU-based deployment
- Community cloud type selected for cost-effectiveness
- Public IP enabled for accessibility

## Troubleshooting

If file transfer fails:
- Check SSH key permissions
- Verify file paths
- Ensure requirements.txt is present
- Check pod status using `runpod.get_pods()`

## Cleanup

To prevent unnecessary charges:
1. Save any important files from the pod
2. Uncomment and run the termination cell
3. Verify pod termination using `runpod.get_pods()`