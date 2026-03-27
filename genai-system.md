# Generative AI Creator for Tasmanian Tourism Marketing

## 1. Project Overview

This project aims to build a region-specific generative AI system for **Tasmanian Tourism Marketing**.  
The platform is designed as a web-based SaaS system that generates **Tasmania-focused promotional images**, **tourism marketing content**, and **personalised travel itineraries** using curated Tasmanian datasets and generative AI models.

The main goal is to support Tasmania’s tourism ecosystem by providing a tool that can create locally relevant and visually authentic digital content.

### Why this project?

Tasmania’s tourism industry has a clear content gap. Many small tourism operators do not have enough resources for professional photography, marketing design, or structured promotional content. At the same time, tourists often struggle to find clear and well-organised local travel information.

This project addresses both problems by providing:
- AI-generated promotional images and posters for tourism operators
- Personalised itinerary and tourism text generation for travellers
- A simple and accessible online platform for content generation and retrieval

### Who is this for?

#### Local Tourism Operators
Small tourism businesses, hotels, local councils, and regional marketers can use the platform to generate professional-quality tourism assets more quickly and at lower cost.

#### Tourists
Travellers can use the platform to generate structured itineraries and tourism-related content tailored to their locations, preferences, and travel dates.

---

## 2. Core Features

- Tasmania-specific AI image generation
- High-resolution output (minimum 1024×1024)
- Image generation target under 30 seconds
- Hybrid prompt builder with guided options and free-text input
- Prompt templates for posters, itineraries, and marketing content
- Real-time generation progress feedback
- User authentication and saved preferences
- Asset library for saving, filtering, sorting, downloading, and managing generated content
- Generation history tracking
- Content moderation and safety filtering
- Text and itinerary generation using RAG and external APIs

---

## 3. Tech Stack

This project combines machine learning, backend services, frontend development, and cloud infrastructure.

### Machine Learning / AI
- **Stable Diffusion XL 1.0**  
  Used as the base image generation model because it is open-source and commercially usable.
- **LoRA (Low-Rank Adaptation)**  
  Used for efficient fine-tuning on Tasmanian datasets with lower GPU memory requirements.
- **PyTorch**  
  Used for training and model development.
- **Hugging Face Diffusers**  
  Used for implementing and managing the image generation pipeline.
- **RAG (Retrieval-Augmented Generation)**  
  Used to improve itinerary and marketing text generation by retrieving tourism-specific context.
- **LLM API (e.g. Claude API / GPT-based API)**  
  Used for itinerary generation, descriptions, and tourism marketing copy.

### Backend
- **REST API**
  Used to expose image and text generation services to the frontend.
- **Celery**
  Used for asynchronous task processing because image generation jobs take time.
- **Redis**
  Used as the task queue / broker for background job management.
- **PostgreSQL**
  Used to store metadata, user accounts, saved assets, and generation history.

### Frontend
- **React / Next.js**
  Used to build the web interface for prompt input, preview, and asset management.
- **Prompt Builder UI**
  Used to help non-technical users create better prompts through dropdowns and templates.

### Cloud / Storage / Delivery
- **AWS S3**
  Used to store generated images and other assets.
- **CloudFront**
  Used to deliver saved assets quickly through CDN caching.
- **Cloud GPU resources**
  Used for model training and potentially inference workloads.

---

## 4. Why these tools were chosen

## Machine Learning / AI Stack

### Stable Diffusion XL 1.0

**Purpose**  
Base image generation model.

**Why it is used**  
Stable Diffusion XL is an open-source diffusion model capable of generating high-quality images. It is commercially usable and widely adopted in generative AI systems, making it suitable for building a scalable tourism content generation platform.

**How it is used in this project**  
In this project, SDXL serves as the foundation model for image generation. The model is fine-tuned using Tasmanian tourism datasets to produce region-specific promotional images such as landscapes, attractions, and travel marketing visuals.


### LoRA (Low-Rank Adaptation)

**Purpose**  
Efficient model fine-tuning.

**Why it is used**  
Training large diffusion models from scratch requires significant GPU resources. LoRA allows efficient fine-tuning by modifying only small adapter layers instead of the entire model, reducing memory and training costs.

**How it is used in this project**  
LoRA is used to adapt the SDXL model to Tasmanian tourism content. The training process uses labelled datasets containing images and captions describing locations, seasons, and tourism themes. This enables the model to generate images that are visually consistent with Tasmanian environments.


### PyTorch

**Purpose**  
Deep learning framework for model training.

**Why it is used**  
PyTorch is widely used for machine learning research and production due to its flexibility and strong ecosystem. It provides efficient GPU acceleration and supports modern generative AI workflows.

**How it is used in this project**  
PyTorch is used to run the LoRA fine-tuning process and manage model training workflows, including dataset loading, training iterations, and model evaluation.


### Hugging Face Diffusers

**Purpose**  
Library for implementing diffusion models.

**Why it is used**  
The Diffusers library provides pre-built pipelines for Stable Diffusion models, making it easier to run training, inference, and model loading without implementing diffusion algorithms from scratch.

**How it is used in this project**  
Diffusers is used to load the SDXL model, apply LoRA weights, and run the image generation inference pipeline when a user submits a prompt through the web interface.


### RAG (Retrieval-Augmented Generation)

**Purpose**  
Enhance text generation with external knowledge.

**Why it is used**  
Standard LLM outputs may lack accurate regional information. RAG improves text generation by retrieving relevant tourism information before generating responses.

**How it is used in this project**  
RAG retrieves tourism descriptions, travel guides, and regional information about Tasmania. This context is provided to the language model when generating travel itineraries or marketing descriptions.


### LLM API (Claude / GPT)

**Purpose**  
Generate tourism text content.

**Why it is used**  
Large language models are effective at generating structured text such as travel itineraries, marketing copy, and destination descriptions.

**How it is used in this project**

The LLM API generates:

- travel itineraries  
- tourism marketing descriptions  
- attraction summaries  

The prompts are enhanced using retrieved context from the RAG system.

---

## Backend Stack

### REST API

**Purpose**  
Communication layer between frontend and backend services.

**Why it is used**  
REST APIs provide a standard way for web applications to interact with backend services and AI models.

**How it is used in this project**

The API handles:

- image generation requests  
- itinerary generation requests  
- user authentication  
- asset retrieval  

**Example flow**


- Frontend → REST API → Queue → Model Worker



### Celery

**Purpose**  
Asynchronous task processing.

**Why it is used**  
Image generation tasks may take several seconds. Running them synchronously would block the server.

**How it is used in this project**

Celery manages background jobs such as:

- image generation tasks  
- model inference jobs  
- heavy processing tasks  

The API submits generation requests to the Celery queue.


### Redis

**Purpose**  
Task queue broker.

**Why it is used**  
Celery requires a message broker to store and distribute background jobs.

**How it is used in this project**

Redis acts as the task queue system that stores image generation requests before they are processed by worker nodes.


### PostgreSQL

**Purpose**  
Primary relational database.

**Why it is used**  
PostgreSQL is reliable and well suited for structured data storage.

**How it is used in this project**

PostgreSQL stores:

- user accounts  
- prompts submitted by users  
- metadata of generated images  
- generation history  
- asset management information  

---

## Frontend Stack

### React / Next.js

**Purpose**  
Web application framework.

**Why it is used**  
React provides a flexible component-based architecture for building interactive user interfaces. Next.js improves performance with server-side rendering and routing.

**How it is used in this project**

The frontend allows users to:

- enter prompts  
- generate images  
- preview generated outputs  
- save or download assets  
- view generation history  


### Prompt Builder UI

**Purpose**  
Simplify prompt creation.

**Why it is used**  
Many users may not be familiar with prompt engineering.

**How it is used in this project**

The prompt builder provides structured input options such as:

- location  
- season  
- subject type  
- visual style  

This helps generate more consistent outputs from the AI model.

---

## Cloud / Storage / Delivery

### AWS S3

**Purpose**  
Cloud object storage.

**Why it is used**  
Generated images must be stored reliably and accessed later by users.

**How it is used in this project**

All generated images and assets are stored in S3 after inference.  
The database stores metadata linking users to their generated images.


### CloudFront (CDN)

**Purpose**  
Content delivery optimisation.

**Why it is used**  
CDNs reduce latency by caching content closer to users.

**How it is used in this project**

Generated images stored in S3 are delivered through CloudFront to improve download speed and scalability.


### Cloud GPU Infrastructure

**Purpose**  
High-performance computing for AI models.

**Why it is used**  
Training and running diffusion models require GPU acceleration.

**How it is used in this project**

GPU instances are used for:

- LoRA model training  
- SDXL inference  
- large-scale image generation  

---

## 5. System Architecture

The system is composed of five major components:

1. **Data Curation Pipeline**  
   Collects, cleans, labels, and prepares Tasmanian image and text datasets.

2. **Model Training Infrastructure**  
   Fine-tunes the base model using LoRA on GPU-enabled infrastructure.

3. **Generation Engine (Backend API)**  
   Accepts prompts, triggers generation jobs, applies the fine-tuned model, and returns outputs.

4. **Web Application (Frontend)**  
   Allows users to create prompts, monitor progress, preview results, and manage generated assets.

5. **Asset Storage & Delivery**  
   Stores generated outputs and delivers them efficiently through cloud storage and CDN.

---

## 6. Workflow / Pipeline

### A. Data Pipeline
1. Collect Tasmanian tourism images and text data
2. Verify licensing and permissions
3. Clean and organise the dataset
4. Label images by location, season, subject type, and mood
5. Build a tourism-focused text corpus for RAG

### B. Model Training Pipeline
1. Load the base model (Stable Diffusion XL)
2. Prepare fine-tuning dataset
3. Train LoRA weights on Tasmanian-specific content
4. Evaluate output quality and regional authenticity
5. Export trained model weights for inference

### C. Inference / Generation Pipeline
1. User enters a prompt through the frontend
2. Frontend sends request to backend API
3. Backend validates prompt and pushes job to Celery queue
4. Worker processes the request using the fine-tuned model
5. Generated image is stored in cloud storage
6. Metadata and history are saved in PostgreSQL
7. Result is returned to the frontend for preview and download

### D. Text / Itinerary Pipeline
1. User provides travel preferences, dates, or prompt inputs
2. Backend retrieves relevant Tasmanian context using RAG
3. LLM generates itinerary or tourism marketing text
4. Output is displayed to the user and optionally saved

---

## 7. Performance Targets

- Image generation time: **under 30 seconds**
- Output resolution: **minimum 1024×1024**
- Concurrent users: **at least 10 simultaneous requests**
- Generated images should be **recognisably Tasmanian**

---

## 8. Project Deliverables

- Fine-tuned LoRA model weights for Tasmanian image generation
- Curated Tasmanian dataset with documentation
- Web application for content generation
- REST API with documentation
- Itinerary generation feature using LLM integration
- Sample gallery of generated tourism content
- Technical documentation for training and deployment
- Evaluation report with quality and feedback results

---

## 9. Current Scope

### Included
- AI-generated promotional images and posters
- Hybrid prompt builder
- Asset library and generation history
- User authentication
- RAG-based itinerary and text generation
- Cloud storage and CDN-based delivery

### Excluded
- GPS and distance-based itinerary optimisation
- 3D scenes, VR outputs, and animation generation in the current phase

---
