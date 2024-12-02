# Abdouml
Abdoutecnologi
from flask import Flask, request, jsonify
import requests

app = Flask(ABDOUML)

# إعدادات واتساب
WHATSAPP_API_URL = "https://graph.facebook.com/v16.0/your_phone_number_id/messages"
ACCESS_TOKEN = "your_facebook_api_access_token"

# معالجة الرسائل الواردة
@app.route('/webhook', methods=['POST'])
def webhook():
    data = request.get_json()

    # استخراج الرسالة الواردة
    try:
        message = data['entry'][0]['changes'][0]['value']['messages'][0]
        phone_number = message['from']
        message_text = message['text']['body']

        # الرد بناءً على محتوى الرسالة
        if "تحميل" in message_text:
            reply_text = "يرجى إرسال رابط التطبيق أو اسمه لنتمكن من مساعدتك."
        else:
            reply_text = "مرحبًا! كيف يمكنني مساعدتك؟"

        send_whatsapp_message(phone_number, reply_text)
    except KeyError:
        return jsonify({"status": "no message found"}), 400

    return jsonify({"status": "message processed"}), 200

# إرسال رسالة عبر واتساب
def send_whatsapp_message(0603465307, text):
    headers = {
        "Authorization": f"Bearer {ACCESS_TOKEN}",
        "Content-Type": "application/json"
    }

    payload = {
        "messaging_product": "whatsapp",
        "to": phone_number,
        "text": {"body": text}
    }

    response = requests.post(WHATSAPP_API_URL, json=payload, headers=headers)
    if response.status_code == 200:
        print("Message sent successfully!")
    else:
        print(f"Failed to send message: {response.status_code}, {response.text}")

if ABDOUML == '__main__':
    app.run(port=5000)
