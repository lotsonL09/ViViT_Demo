# ViViT Demo for Action Recognition in Warehouses

This demo was developed as a tool to demonstrate how a computer vision model predicts actions from video data. The model is able to recognize the following actions:

* Lifting an object
* Normal activity
* Placing an object

<div><>

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/165b6f40-9bb9-44a3-a183-a95d47e4a5ce" alt="GIF1" width="200" />
  <img src="https://github.com/user-attachments/assets/72191915-97b4-4a48-abc9-cb327ebf2bfc" alt="GIF2" width="200" />
  <img src="https://github.com/user-attachments/assets/2bfca674-d98c-4a66-8150-eeeb215a0a5a" alt="GIF3" width="200" />
</div>
 
## 1. Training Dataset Structure

The model was trained using a structured dataset built from video recordings obtained from fixed surveillance cameras installed at Automatic Control System Laboratory of University of Piura. The dataset was designed to support supervised learning while remaining simple and interpretable.

### 1.1 Data Organization
Video data was organized at the directory level to reflect the training labels. Each subdirectory corresponds to a specific monitoring category.

```
dataset/
├── train/
│ ├── lifting/
│ │ ├── video_001.mp4
│ │ ├── video_002.mp4
│ │ └── ...
│ ├── placing/
│ │ ├── video_001.mp4
│ │ └── ...
│ └── normal/
│ ├── video_001.mp4
│ └── ...
└── validation/
  ├── lifting/
  ├── placing/
  └── normal/
```

To increase the effective size and diversity of the training data, **data augmentation techniques** were applied during preprocessing. These augmentations introduce controlled variations in the input data, improving the model’s robustness to changes in appearance and motion patterns.

Due to **hardware limitations**, direct training on full-length videos was computationally expensive and resulted in excessively long training times. To address this constraint, **temporal sampling** was performed prior to training, selecting a fixed number of frames per video at uniform intervals. This strategy significantly reduced computational cost while preserving the temporal information required for action recognition.

This structure, combined with augmentation and sampling strategies, enables a clear separation between training and validation data while ensuring an efficient and scalable training process.

---

### 1.2 Data Representation
Each video sample is represented as a sequence of frames extracted at a fixed sampling rate. Frames are resized and normalized before being passed to the model.

- **Input format:** sequence of RGB frames  
- **Frame resolution:** fixed spatial dimensions  
- **Temporal sampling:** uniform frame extraction per video  

---

### 1.3 Labels and Annotations
Labels are assigned at the video level and correspond to the monitoring category associated with each sample. The dataset does not rely on frame-level annotations, which simplifies data preparation and reflects real-world constraints.

---

### 1.4 Scope and Limitations
The dataset was designed for proof-of-concept experiments and does not aim to provide exhaustive coverage of all possible monitoring scenarios. Variations in lighting, occlusions, and camera angles may affect model performance.

---

## 2. Model Training Configuration

### 2.1 Loss Function
A **cross-entropy loss** function was used to optimize the multi-class classification task. This loss function is appropriate for categorical prediction problems, as it penalizes incorrect class probabilities while encouraging confident predictions for the correct action class.

- **Loss function:** Cross Entropy Loss  

---

### 2.2 Optimization Strategy
Model parameters were optimized using the **Adam optimizer**, which combines adaptive learning rates with momentum-based updates. This choice improves convergence speed and training stability compared to standard stochastic gradient descent.

- **Optimizer:** Adam  
- **Initial learning rate:** `0.0001`  

---

### 2.3 Learning Rate Scheduling
To prevent performance stagnation and improve fine-tuning, a **ReduceLROnPlateau** learning rate scheduler was applied. The scheduler reduces the learning rate when the validation loss stops improving, allowing the model to refine its parameters during later training stages.

- **Scheduler:** Reduce on Plateau  
- **Monitored metric:** Validation accuracy  

---

### 2.4 Training Hyperparameters
The following hyperparameters were used during training:

- **Batch size:** 128  
- **Number of epochs:** 200  

---

### 2.5 Hardware and Execution Environment
Training was performed using hardware acceleration to handle video-based data efficiently.

- **Framework:** PyTorch  
- **Hardware:** GPU-enabled system  
- **Device:** CUDA-compatible GPU (if available)  


## 3. Deployment and Demo Execution

The action recognition system was deployed as a web-based demo to allow interactive evaluation of the trained model. The deployment architecture separates backend inference logic from frontend visualization, ensuring modularity and ease of use.

---

### 3.1 Backend Service
The backend was implemented using **FastAPI**, providing a lightweight and efficient API for handling video uploads and model inference. FastAPI enables rapid development, asynchronous request handling, and straightforward integration with deep learning frameworks.

- **Backend framework:** FastAPI  
- **Purpose:** Model inference and request handling  

---

### 3.2 Frontend Interface
The frontend interface was built using **Bootstrap**, allowing a simple and responsive layout for user interaction. The interface enables users to upload video files and visualize predicted actions in an intuitive manner.

- **Frontend framework:** Bootstrap  
- **Purpose:** User interaction and result visualization  

---

### 3.3 Environment Setup
To ensure reproducibility and dependency isolation, the application must be executed within a **Python virtual environment**.

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows
venv\Scripts\activate
# On Linux / macOS
source venv/bin/activate
```

### 3.4 Environment Setup

All required libraries and dependencies are listed in the **requirements.txt** file. Once the virtual environment is activated, dependencies can be installed using:

```bash
pip install -r requirements.txt
```

### 3.5 Running the Demo

After installing the dependencies, the demo can be launched by executing the main application script:

```python
python main.py
```

### 3.6 Deployment Overview
```
User (Web Browser)
        │
        ▼
Bootstrap Frontend
        │
        ▼
FastAPI Backend
        │
        ▼
```
Action Recognition Model
        │
        ▼
Predicted Action
```
