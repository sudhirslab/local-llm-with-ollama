<p align="center">
  <img src="https://github.com/user-attachments/assets/2b726410-cac5-477e-aa40-427c2b7a76cf" />
</p>

## Running Open-Source LLMs Locally with Ollama: A Comprehensive Guide

Large language models (LLMs) have become ubiquitous in today’s tech landscape, and even if you’ve been somewhat disconnected from the conversation, you’ve likely heard of ChatGPT. While there are several commercial models available—such as OpenAI’s GPT, Anthropic’s Claude, and Google’s Gemini—numerous open-source models can be run on your local machine.

In this article, I’ll focus on my preferred method for running open-source LLMs locally: using Ollama. With Ollama, you can utilize curated models or import custom ones.

### Benefits of Running Open-Source Models Locally

Running open-source models has several advantages:

- **Access to Unique Models**: You can use models not available as a service elsewhere.
- **Enhanced Privacy**: For sensitive data, running models locally ensures your information remains confidential.
- **Cost Savings**: Using a smaller model that meets your needs can reduce expenses compared to cloud-based options.

### Open-Source LLMs: Does Size Matter?

The fast-evolving AI landscape is filled with both large (like Llama2 70B) and smaller models, each becoming more efficient over time. Smaller models often require less hardware, consume less energy, and can still perform exceptionally well. 

Choosing a smaller model can lead to benefits such as:

- Reduced compute resource requirements.
- Lower energy consumption.
- Greater control over scaling, avoiding reliance on provider limits.

Some notable open-source models include:

- **Llama 2 70B**: A general-purpose model with 70 billion parameters from Meta.
- **Phi-2**: A 2.7 billion parameter model from Microsoft, known for its efficiency on lower-end hardware.
- **Mistral 7B**: A compact 7 billion parameter model.
- **Mixtral 8x7B**: A powerful “mixture-of-experts” model with a total of 46.7 billion parameters, yet only utilizes 12.9 billion parameters at a time.

### Advantages of Using Ollama

Ollama is a popular open-source tool for running large language models locally. Here are a few reasons to consider it:

- **Active Maintenance**: Regular updates and improvements.
- **Community Support**: An engaged community is available via Discord.
- **Docker Compatibility**: Easily run Ollama in a Docker container without local installation.
- **User-Friendly**: Simple to set up and operate.
- **Model Variety**: Supports numerous models, with new ones added regularly, and allows for custom models.
- **Open-Source Frontend**: The Ollama Web-UI enhances user experience.
- **Multi-Modal Support**: Handle models with various input types.

You can find the Ollama project on GitHub.

### Setting Up Ollama Locally in Four Steps

#### Step 1: Install Ollama

Ollama is easy to install on macOS and Linux, with Windows support available in preview via WSL2 (Windows Subsystem for Linux).

For a smoother experience, I recommend using Docker, which keeps your installation independent of your main operating system. Both Ollama and the Web-UI offer Docker images for easy deployment.

**Prerequisites**:
- Install the Docker engine.
- Decide if you want to run Ollama with or without a GPU.

**To install Ollama without a GPU**:
Run the following command:
```bash
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```
This command sets up a Docker container for Ollama and maps the necessary ports.

**To install Ollama with a GPU**:
1. Install the **NVIDIA Container Toolkit**.
2. Configure the runtime:
   ```bash
   sudo nvidia-ctk runtime configure --runtime=docker
   sudo systemctl restart docker
   ```
3. Start the container:
   ```bash
   docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
   ```

You can verify the GPU is working by checking its usage during model operation.

**Check if Ollama is running**:
```bash
docker exec -it ollama ollama help
```

#### Step 2: Running the Ollama Web-UI

Next, you’ll run the Ollama Web-UI in a Docker container:
```bash
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v ollama-webui:/app/backend/data --name ollama-webui --restart always ghcr.io/ollama-webui/ollama-webui:main
```
Access the Web-UI at `http://127.0.0.1:3000/` and create an account.

#### Step 3: Choosing a Model

Explore Ollama’s model library to see available options. For this example, we’ll run the Mixtral 8x7B model:
```bash
docker exec -it ollama ollama run mixtral
```
The initial run will take time as the model downloads.

You can also manage models through the Web-UI.

#### Step 4: Using the Model

After selecting the Mixtral model, you can prompt it to generate code. If you encounter issues, you can ask it to debug the code, showcasing its interactive capabilities.

### Storage Considerations

Keep in mind that these models can consume significant storage. Docker volumes are persistent, so remember to manage your storage effectively. Use these commands to view storage information:
```bash
docker system df
docker system df -v
```

### Multi-Modal Models

Ollama also supports multi-modal models, including those with vision capabilities. For example, you can run the Llava 34B model:
```bash
docker exec -it ollama ollama run llava:34b
```
The Web-UI allows you to input images and interact with the model.

### Loading Additional Models

You can import models that aren’t part of the Ollama library by using the GGUF format. Many GGUF models can be found on Hugging Face, curated by TheBloke, which can simplify your search.

### Conclusion

With a growing array of open-source models available, Ollama and Ollama Web-UI empower you to run these models locally on your hardware, providing complete control and privacy. Whether for experimentation or application development, you can leverage these tools to explore the full potential of LLMs in your projects.
