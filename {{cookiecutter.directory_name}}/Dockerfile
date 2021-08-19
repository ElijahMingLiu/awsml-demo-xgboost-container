FROM ubuntu:18.04

RUN apt-get update && \
    apt-get -y install --no-install-recommends \
    wget \
    curl \
    python3-dev \
    nginx \
    ca-certificates \
    libgomp1 \
    gcc \
    && rm -rf /var/lib/apt/lists/* \
    && curl -O https://bootstrap.pypa.io/get-pip.py \
    && python3 get-pip.py
RUN pip install boto3

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN update-alternatives --install /usr/local/bin/pip pip /usr/local/bin/pip3 1

RUN pip3 install -i https://pypi.doubanio.com/simple boto3 numpy scipy scikit-learn pandas flask gevent gunicorn xgboost

# Set some environment variables. PYTHONUNBUFFERED keeps Python from buffering our standard
# output stream, which means that logs can be delivered to the user quickly. PYTHONDONTWRITEBYTECODE
# keeps Python from writing the .pyc files which are unnecessary in this case. We also update
# PATH so that the train and serve programs are found when the container is invoked.

ENV PYTHONUNBUFFERED=TRUE
ENV PYTHONDONTWRITEBYTECODE=TRUE
ENV PATH="/opt/ml/code:${PATH}"

WORKDIR /opt/ml/code

COPY code /opt/ml/code
RUN chmod +x train

