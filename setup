#!/bin/bash

set -e

echo "🔧 Step 1: Install Homebrew dependencies..."
brew update
brew install python3 libffi openssl

echo "📂 Step 2: Create Home Assistant virtual environment..."
mkdir -p ~/homeassistant
cd ~/homeassistant
python3 -m venv venv
source venv/bin/activate

echo "📦 Step 3: Install Home Assistant Core..."
pip install --upgrade pip wheel
pip install homeassistant

echo "🛠️ Step 4: Create launchctl plist for auto-start..."

USER_NAME=$(whoami)
PLIST_PATH="/Users/$USER_NAME/Library/LaunchAgents/homeassistant.core.plist"

cat > "$PLIST_PATH" <<EOF
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>homeassistant.core</string>

  <key>ProgramArguments</key>
  <array>
    <string>/Users/$USER_NAME/homeassistant/venv/bin/hass</string>
  </array>

  <key>RunAtLoad</key>
  <true/>

  <key>WorkingDirectory</key>
  <string>/Users/$USER_NAME/homeassistant</string>

  <key>StandardOutPath</key>
  <string>/tmp/homeassistant.out.log</string>

  <key>StandardErrorPath</key>
  <string>/tmp/homeassistant.err.log</string>
</dict>
</plist>
EOF

echo "🚀 Step 5: Load launch agent..."
launchctl load "$PLIST_PATH"

echo "✅ Home Assistant Core installed and set to auto-start!"
echo "🌐 Visit http://localhost:8123 after a few minutes."
