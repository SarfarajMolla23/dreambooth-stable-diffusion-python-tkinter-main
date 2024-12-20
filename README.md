
---

# Dreambooth Stable Diffusion Pipeline

This repository provides a complete setup for training and deploying custom Stable Diffusion models, leveraging AWS services and RunPod for efficient processing. 

---

## Setup Instructions

### 1. AWS Setup
1. **Create an S3 Bucket**  
   - Log in to your AWS account and navigate to the S3 service.  
   - Create a new S3 bucket for storing training data and outputs.  

2. **Create an SQS Queue**  
   - Go to the SQS service and create a FIFO queue.  
   - In the queue settings, enable the **Content-based deduplication** option.  

3. **Create an IAM User**  
   - Create a new IAM user in AWS.  
   - Attach the policy `s3_sqs_access.json` from this repository to the IAM user.  
   - Generate access keys for the user and save them for later use.

---

### 2. RunPod Setup
1. **Launch a Pod**  
   - Go to [RunPod](https://www.runpod.io/).  
   - Under "Secure Cloud," launch an RTX A6000 pod.  
   - Select the **RunPod Stable Diffusion** template and unselect the "Start Jupyter Notebook" option.

2. **Access the Pod via SSH**  
   - SSH into your pod and execute the following commands:  

   ```bash
   git clone https://github.com/JoePenna/Dreambooth-Stable-Diffusion
   wget https://huggingface.co/panopstor/EveryDream/resolve/main/sd_v1-5_vae.ckpt
   apt install zip -y
   mkdir Dreambooth-Stable-Diffusion/training_images
   mv sd_v1-5_vae.ckpt Dreambooth-Stable-Diffusion/model.ckpt
   git clone https://github.com/djbielejeski/Stable-Diffusion-Regularization-Images-person_ddim.git
   mkdir -p Dreambooth-Stable-Diffusion/regularization_images/person_ddim
   mv -v Stable-Diffusion-Regularization-Images-person_ddim/person_ddim/*.* Dreambooth-Stable-Diffusion/regularization_images/person_ddim/
   cd Dreambooth-Stable-Diffusion
   pip install -e .
   pip install boto3
   pip install pytorch-lightning==1.7.6
   pip install torchmetrics==0.11.1
   pip install -e git+https://github.com/CompVis/taming-transformers.git@master#egg=taming-transformers
   pip install captionizer
   ```

---

### 3. Configuration
1. **Download Required Files**  
   - Download the following files from this repository:  
     - `execute_pipeline.py`  
     - `credentials.py`  
     - `variables.py`  
     - `prompts.py`  

2. **Update Credentials**  
   - Open `credentials.py` and add the access keys you created in AWS.  

3. **Update Variables**  
   - Open `variables.py` and update it with the name of your S3 bucket and the URL of your SQS queue.  

---

### 4. Execute the Pipeline
Run the pipeline with the following command:  
```bash
python execute_pipeline.py
```

---

### 5. App Execution
1. **Clone This Repository**  
   ```bash
   git clone https://github.com/your-username/your-repository.git
   ```

2. **Install Requirements**  
   ```bash
   pip install -r requirements.txt
   ```

3. **Update Credentials**  
   - Update `credentials.py` with your AWS access keys.  

4. **Update Variables**  
   - Update `variables.py` with your S3 bucket name and SQS queue URL.  

5. **Run the Application**  
   ```bash
   python main.py
   ```

Enjoy using the app!

---

## Next Steps
- Develop a web-based interface for easier usage.  
- Explore serverless services for model training and inference.  
- Investigate advanced technologies for face and person generation.

---

## License
This project is licensed under the MIT License. See the `LICENSE` file for more details.

---

## Acknowledgements
- **Hugging Face**  
  Models and tools are powered by Hugging Face Transformers.  
- **RunPod**  
  Efficient model training with secure cloud pods.  
- **AWS**  
  Storage and queue services for scalable operations.  

Feel free to contribute by submitting issues or pull requests!
