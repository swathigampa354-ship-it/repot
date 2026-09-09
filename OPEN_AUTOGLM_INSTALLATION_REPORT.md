# Open-AutoGLM Installation Report

## Environment

- **Python:** 3.13.5
- **Architecture:** aarch64 (ARM64)
- **ADB:** 34.0.5-debian (installed)
- **Android device:** Not connected (requires wireless debugging setup)
- **Open-AutoGLM:** v0.1.0 (installed in editable mode)
- **Model endpoint:** Not configured (requires API key)
- **ADB Keyboard:** APK downloaded to /tmp/ADBKeyboard.apk
- **Final status:** INSTALLED - requires API key and ADB device connection

## Installed Components

1. **System packages:**
   - adb (Android Debug Bridge)
   - python3-pip, python3.13-venv, build-essential
   - libjpeg-dev, zlib1g-dev, libfreetype6-dev

2. **Python packages (in virtual environment):**
   - Pillow 12.3.0
   - openai 3.10.0
   - requests 2.34.2
   - phone-agent 0.1.0 (editable install)

3. **Open-AutoGLM repository:** Cloned to /tmp/Open-AutoGLM

## What Remains To Be Configured

### 1. API Key (Required)

You need an API key from one of these providers:

**Option A: z.ai (Recommended)**
- Documentation: https://docs.z.ai/api-reference/introduction
- Base URL: `https://api.z.ai/api/paas/v4`
- Model: `autoglm-phone-multilingual`

**Option B: Novita AI**
- Documentation: https://novita.ai/models/model-detail/zai-org-autoglm-phone-9b-multilingual
- Base URL: `https://api.novita.ai/openai`
- Model: `zai-org/autoglm-phone-9b-multilingual`

**Option C: Parasail**
- Documentation: https://www.saas.parasail.io/serverless?name=auto-glm-9b-multilingual
- Base URL: `https://api.parasail.io/v1`
- Model: `parasail-auto-glm-9b-multilingual`

### 2. Android Device Connection (Required)

Since we're in a PRoot environment, you need to connect via wireless debugging:

1. **Enable Developer Options on your phone:**
   - Go to Settings > About Phone
   - Tap "Build Number" 7-10 times until Developer Options are enabled

2. **Enable Wireless Debugging:**
   - Go to Settings > Developer Options
   - Enable "Wireless Debugging"
   - Note the IP address and port shown

3. **Connect via ADB:**
   ```bash
   adb connect <phone-ip>:<port>
   ```

4. **Verify connection:**
   ```bash
   adb devices
   ```

### 3. ADB Keyboard (Required for text input)

1. **Install the APK on your phone:**
   ```bash
   adb install /tmp/ADBKeyboard.apk
   ```

2. **Enable ADB Keyboard:**
   - Go to Settings > Input Method
   - Enable "ADB Keyboard"
   - Or run: `adb shell ime enable com.android.adbkeyboard/.AdbIME`

## Working Commands

### Test Setup
```bash
/tmp/Open-AutoGLM/test-setup.sh
```

### Run Open-AutoGLM (after configuring API key and device)
```bash
# Set environment variables
export OPENAI_API_KEY="your-api-key"
export OPENAI_BASE_URL="https://api.z.ai/api/paas/v4"
export OPENAI_MODEL="autoglm-phone-multilingual"

# Run interactive mode
/tmp/Open-AutoGLM/run.sh

# Run specific task
/tmp/Open-AutoGLM/run.sh "Open Settings"
```

### Direct Python Command
```bash
/tmp/Open-AutoGLM/venv/bin/python /tmp/Open-AutoGLM/main.py \
  --base-url https://api.z.ai/api/paas/v4 \
  --model "autoglm-phone-multilingual" \
  --apikey "your-api-key" \
  "Open Settings"
```

### Check Model Deployment
```bash
/tmp/Open-AutoGLM/venv/bin/python /tmp/Open-AutoGLM/scripts/check_deployment_en.py \
  --base-url https://api.z.ai/api/paas/v4 \
  --model "autoglm-phone-multilingual" \
  --apikey "your-api-key"
```

### List Supported Apps (Android)
```bash
/tmp/Open-AutoGLM/venv/bin/python /tmp/Open-AutoGLM/main.py --list-apps
```

## Important Notes

1. **API costs:** Using the model API will incur costs. Check pricing with your chosen provider.

2. **Device authorization:** You must authorize the ADB connection on your phone when prompted.

3. **Wireless debugging:** Ensure your phone and this environment are on the same network.

4. **Model deployment:** If you want to run the model locally, you'll need a GPU with sufficient VRAM (the 9B model requires significant resources). The recommended approach is to use a cloud API.

5. **Safety:** Open-AutoGLM can control your phone. Only use it with tasks you trust.

## Next Steps

1. Get an API key from one of the providers above
2. Enable wireless debugging on your phone
3. Connect your phone via ADB
4. Install ADB Keyboard on your phone
5. Run the test command: `./run.sh "Open Settings"`
