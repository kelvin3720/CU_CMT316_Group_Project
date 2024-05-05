# CU_CMT316_Group_Project

This is coursework 2 (group project) for CMT316 Applications of Machine Learning: Natural Language Processing/Computer Vision in Cardiff University 2023/2024.

# NLP-6: Sentiment analysis

The goal of this sentiment analysis task is to recognize if a tweet is positive, negative or neutral. We use the Semeval-2017 dataset for  Subtask  A  (Rosenthal et al., 2019). The full dataset can be accessed from [this repository](https://github.com/cardiffnlp/tweeteval).

## How to run the code on Linux

Prerequisite
 - Python 3.11+
 - Linux (Ubuntu 22.04 is preferred) or Windows 10/11
 - Internet access to download pretrained model
 - Jupyter (If Docker is not used)
 - Docker (Recommended)
    - [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) (Only required for Linux)
 - A CUDA supported GPU with Driver (Recommended)
 - Ubuntu 22.04 in WSL2 (Recommended for Windows)

### Enviornment setup with Docker (Preferred)

Note: The image `tensorflow/tensorflow:latest-gpu-jupyter` with manifest digest `sha256:efc25f8ad0ec337e8f4e2de9e7e8e391e6729481c7a7cae4bdea3137da7822c6` does not work on some older GPUs like the `GTX 1070 Max-Q` (Code can still be run but the GPU will not be recognised and used). Please use older images like `tensorflow/tensorflow:2.14.0-gpu-jupyter` in such case.

1. Pull the Docker image (Replace the tag if needed)
```cmd
docker pull tensorflow/tensorflow:latest-gpu-jupyter
```
2. Run the image (Modify the port, path and tag if needed). 

Depending on your config, you may not be able to access the Jupyter Server due to privilege settings, so it is recommended to run this command with sudo.
```cmd
sudo docker run --gpus all -it -p 8888:8888 -v [Path to the project]:/tf  tensorflow/tensorflow:[The tag you used in the previous step]
```
3. A url with token should be prompted, and you may use the url to access the Jupyter Server. Example:
```cmd
http://127.0.0.1:8888/tree?token=[Generated Token]
```

### Enviornment setup without Docker and GPU

1. Please  install Python 3.11+ and install the required packages by
```cmd
pip install -r requirements.txt
```
2. Manually install the following packages with pip if [requirements.txt](requirements.txt) did not work:
    - [emoji](https://pypi.org/project/emoji/)
    - [matplotlib](https://pypi.org/project/matplotlib/)
    - [nltk](https://pypi.org/project/nltk/)
    - [numpy](https://pypi.org/project/numpy/)
    - [sklearn](https://pypi.org/project/scikit-learn/)
    - [tensorflow](https://pypi.org/project/tensorflow/)
    - [tf-keras](https://pypi.org/project/tf-keras/)
    - [transformers](https://pypi.org/project/transformers/)
3. Start and access your Jupyter Server

### Running the code

You should be able to run the `.ipynb` files in Jupyter successfully at this point. Missing packages will be installed in `main.ipynb`, so please make sure there is internet access.

## How to run the code on Windows 10 and 11

To run this code on Windows 10 and 11, using WSL2 (Windows Subsystem for Linux) is recommended to utilise your GPU.

If you are not planning to use GPU to run the code, you may follow the same step as [here](#enviornment-setup-without-docker-and-gpu).

1. Install WSL using PowerShell (Administrator)
```cmd
wsl --install
```
2. Install Ubuntu 22.04
```cmd
wsl --install -d Ubuntu-22.04
```
3. Follow the steps [here](#enviornment-setup-with-docker-preferred) and run the `.ipynb` files.

Note: Before running the Docker image, make sure you are running both Docker and PowerShell as administrator, to access the Jupyter Server.

## Bug acknowledgement and fix

This problem only occurs when running the code with certain GPUs. If you're not experiencing this problem, or running this code with a CPU, ignore the following guidance.

During testing, it has been noted that on some machines (in this case, a machine with NVIDIA GeForce RTX 3050 Laptop GPU), the model will crash despite not utilising the device's full memory. It seems to be a problem with tensorflow's GPU allocator, as it has been noted that the default option does have problems with fragmentation, which can lead to crashes as described above. The fix for this is changing the GPU allocator to `cuda_malloc_async`. 

If you experience such problems:

1. Navigate to "Bug fix - uncomment only if you need it"
2. Uncomment both of the required lines
3. Restart Kernel and run all cells again

If the problem of running out of memory still persists, run the code using a CPU. This will significantly increase the execution time, but there will be no risk of crashing.
To achieve that, simply run the image without the `--gpus all` flag. This will prevent GPUs from being allocated and a CPU will be used instead.
