# 🚀 Google Colab Run Guide — Banana Bot & OmniStream

Google Colab par run karne ke liye niche diye gaye steps follow karein. Aap **Telegram Bot**, **FastAPI Web UI**, ya **Dono ek sath** run kar sakte hain.

---

## 📋 Prerequisites
Colab par new notebook open karein: [colab.research.google.com](https://colab.research.google.com)

---

## ⚡ Method 1: Sirf Telegram Bot Run Karna

### 🟢 Cell 1: Repo Clone & System Packages (FFmpeg, Bento4, N_m3u8DL-RE)
```python
# 1. Repository clone karein (apna GitHub URL dalein)
!git clone https://github.com/YOUR_USERNAME/Banana_bot--main.git
%cd Banana_bot--main

# 2. System tools install karein
!apt-get update -qq && apt-get install -y ffmpeg aria2 cmake make g++ wget unzip > /dev/null 2>&1

# 3. Bento4 mp4decrypt install karein (DRM decryption ke liye)
!wget -q https://github.com/axiomatic-systems/Bento4/archive/v1.6.0-639.zip
!unzip -q v1.6.0-639.zip && cd Bento4-1.6.0-639 && mkdir -p build && cd build && cmake .. > /dev/null 2>&1 && make -j$(nproc) > /dev/null 2>&1 && cp mp4decrypt /usr/local/bin/ && cd ../.. && rm -rf Bento4-1.6.0-639 v1.6.0-639.zip

# 4. N_m3u8DL-RE downloader install karein
!wget -q https://github.com/nilaoda/N_m3u8DL-RE/releases/download/v0.6.0-beta/N_m3u8DL-RE_v0.6.0-beta_linux-x64_20260629.tar.gz
!tar -xzf N_m3u8DL-RE_v0.6.0-beta_linux-x64_20260629.tar.gz && mv N_m3u8DL-RE /usr/local/bin/ && chmod +x /usr/local/bin/N_m3u8DL-RE && rm -f N_m3u8DL-RE_v0.6.0-beta_linux-x64_20260629.tar.gz

# 5. Python libraries install karein
!pip install --upgrade pip
!pip install -r sainibots.txt
!pip install -U yt-dlp
print("✅ All dependencies installed successfully!")
```

---

### 🟢 Cell 2: Telegram Bot Start Karein
```python
import os

# Agar modules/vars.py me pehle se token daal rakha hai to direct run kar sakte hain,
# ya fir yahan environment variables set karein:
os.environ["API_ID"] = "29719806"
os.environ["API_HASH"] = "c8e87805739aa77bd5bd4076148a9a66"
os.environ["BOT_TOKEN"] = "YOUR_BOT_TOKEN_HERE"   # Apna bot token yahan dalein
os.environ["OWNER"] = "1394117837"

# Bot start karein
!python3 modules/main.py
```

---

## 🌐 Method 2: Web Downloader (FastAPI Glassmorphic UI) Run Karna

Agar aapko Web UI access karna hai public link ke sath:

### 🟢 Cell 1: Setup & Dependencies
```python
!git clone https://github.com/YOUR_USERNAME/Banana_bot--main.git
%cd Banana_bot--main
!apt-get update -qq && apt-get install -y ffmpeg aria2 > /dev/null 2>&1
!pip install -r requirements.txt
!pip install -U yt-dlp
```

### 🟢 Cell 2: Server Start + Cloudflare Tunnel (Free Public URL)
```python
import subprocess, time, urllib.request

# Cloudflared download karein (localtunnel se zyada stable aur fast hai, koi password nahi maangta)
!wget -q -nc https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
!dpkg -i cloudflared-linux-amd64.deb > /dev/null 2>&1

# FastAPI background me start karein
proc = subprocess.Popen(["python3", "-m", "uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"])
time.sleep(3)

# Cloudflare tunnel start karein aur public link print karein
!cloudflared tunnel --url http://127.0.0.1:8000
```
*(Terminal me `https://xxxxxx.trycloudflare.com` link aayega, uspar click karke web UI open karein).*

---

## ⚡ Method 3: Dono Ek Sath (Web App + Telegram Bot)

```python
import subprocess, os, time

# Bot credentials set karein
os.environ["API_ID"] = "29719806"
os.environ["API_HASH"] = "c8e87805739aa77bd5bd4076148a9a66"
os.environ["BOT_TOKEN"] = "YOUR_BOT_TOKEN_HERE"
os.environ["OWNER"] = "1394117837"

# 1. FastAPI background me start karein
web_proc = subprocess.Popen(["python3", "-m", "uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"])
print("🚀 Web App started in background on port 8000")
time.sleep(2)

# 2. Telegram Bot start karein
!python3 modules/main.py
```

---

## 💡 Important Tips for Colab

1. **Colab Keep-Alive (Disconnect hone se bachane ke liye)**:
   Browser me `F12` daba kar **Console** tab me ye paste karke Enter press karein:
   ```javascript
   function ConnectButton(){
       console.log("Connect Clicked"); 
       document.querySelector("#top-toolbar > colab-connect-button").shadowRoot.querySelector("#connect").click() 
   }
   setInterval(ConnectButton, 60000);
   ```
2. **ClassX Feature Usage**:
   - Bot settings me jakar **Set Token** -> **ClassX** par click karke aap apna Edge-Cache token set kar sakte hain (agar direct link me token nahi hai).
   - Agar link me pehle se `edge-cache-token=` hai, to bot aur web app automatically tenant extract karke Google Edge Cache headers ke sath high-speed me stream download kar lenge.
