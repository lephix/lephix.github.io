---
title: Note for HuggingFace
---

# Prepare envrionment

## Docker image

As most of the models should using GPU and pytorch, so we could build a Docker Image.

```dockerfile
FROM pytorch/pytorch:2.3.0-cuda12.1-cudnn8-devel

# install some necessary system software
RUN apt update && apt install vim

# install transformers
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple transformers
```

