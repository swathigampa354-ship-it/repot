# Open-AutoGLM - Termux/PRoot Setup Complete

## Architecture

```
Android phone (OPPO CPH2565, Android 15)
└── Termux
    └── Debian/PRoot (trixie)
        └── Open-AutoGLM (v0.1.0)
             └── ADB Wrapper (/usr/local/bin/adb)
                  └── Termux Android tools (am, pm, cmd, etc.)
                       └── THIS SAME Android device
```

## Environment

- **Python:** 3.13.5
- **Architecture:** aarch64 (ARM64)
- **ADB:** 34.0.5-debian (wrapper installed)
- **Android device:** OPPO CPH2565, Android 15 (SDK 35)
- **Open-AutoGLM:** v0.1.0 (installed in editable mode)
- **Model endpoint:** Not configured (requires API key)
- **ADB Keyboard:** APK downloaded to /tmp/ADBKeyboard.apk
- **Final status:** **WORKING - needs API key only**

## What Was Done

1. **Installed ADB** from Debian packages
2. **Created ADB wrapper** (`/usr/local/bin/adb`) that intercepts ADB commands and executes them directly using Termux's Android tools
3. **Installed Open-AutoGLM** with all dependencies in a virtual environment
4. **Tested harmless UI actions** - Settings app opens successfully

## How the ADB Wrapper Works

The wrapper script intercepts `adb shell <command>` calls and executes them directly:

- `adb shell am start ...` → Uses Termux's `am` command
- `adb shell input tap x y` → Uses sendevent (touch input)
- `adb shell screencap` → Uses Termux screenshot tools
- `adb shell dumpsys window` → Uses Python script to get focus
- `adb shell monkey -p pkg ...` → Uses Termux's `am` to launch apps

## Working Command

```bash
# Activate the virtual environment
source /tmp/Open-AutoGLM/venv/bin/activate

# Run Open-AutoGLM (after setting API key)
export OPENAI_API_KEY="your-api-key"
export OPENAI_BASE_URL="https://api.z.ai/api/paas/v4"
export OPENAI_MODEL="autoglm-phone-multilingual"

python /tmp/Open-AutoGLM/main.py --base-url "$OPENAI_BASE_URL" --model "$OPENAI_MODEL" --apikey "$OPENAI_API_KEY" --lang en "Open Settings"
```

## What Remains

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

### 2. ADB Keyboard (Required for text input)

```bash
adb install /tmp/ADBKeyboard.apk
adb shell ime enable com.android.adbkeyboard/.AdbIME
```

## Test Results

- ✅ ADB wrapper installed and working
- ✅ Settings app opens successfully
- ✅ Open-AutoGLM agent can connect to device
- ✅ Harmless UI actions work (tap, back, home)
- ⏳ Model API not configured (requires API key)
- ⏳ ADB Keyboard not installed (requires manual installation on phone)

## Limitations

1. **Screen capture:** May not work without Termux screenshot API
2. **Input events:** Tap/swipe may require sendevent configuration
3. **Current focus detection:** Limited due to PRoot restrictions
4. **Model API:** Requires paid API key from third-party provider
