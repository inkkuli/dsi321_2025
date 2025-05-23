FROM python:3.11-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    gnupg \
    ca-certificates \
    curl \
    apt-transport-https \
    procps \
    libxss1 \
    libasound2 \
    libnspr4 \
    libnss3 \
    xdg-utils \
    fonts-liberation \
    libglib2.0-0 \
    lsb-release \
    unzip \
    chromium \
 && rm -rf /var/lib/apt/lists/*

ENV CHROMEDRIVER_VERSION=124.0.6367.207

RUN wget -q "https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/${CHROMEDRIVER_VERSION}/linux64/chromedriver-linux64.zip" -O chromedriver_linux64.zip && \
    unzip chromedriver_linux64.zip -d /tmp/chromedriver_temp && \
    mv /tmp/chromedriver_temp/chromedriver-linux64/chromedriver /usr/local/bin/chromedriver && \
    chmod +x /usr/local/bin/chromedriver && \
    rm chromedriver_linux64.zip && \
    rm -rf /tmp/chromedriver_temp

RUN useradd -m -u 1000 jupyter_user
USER jupyter_user
WORKDIR /home/jupyter_user/work

COPY --chown=jupyter_user:jupyter_user requirements.txt .
RUN pip install --no-cache-dir --user -r requirements.txt

ENV PATH="/home/jupyter_user/.local/bin:${PATH}"

COPY --chown=jupyter_user:jupyter_user . .
