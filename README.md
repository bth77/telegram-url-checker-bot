import requests
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext

# Ganti dengan token bot Telegram Anda (dari @BotFather)
TOKEN = "7579583130:AAFlYLk6hTSO8SG5pGAwYmom18bp-B1wZZc"

# Daftar URL yang akan dicek (sesuaikan dengan kebutuhan)
URL_LIST = [
    "https://probth77.info/",
    "https://bethoki-77gg.xyz/",
    "https://bethoki88.info/",
    "https://bethoki88.pro/",
    "https://bethoki88.store/",
    "https://winbethoki77.xyz/",
    "https://bethoki88.xyz/",
    "https://cuanbethoki77.store/",
    "https://bth77-vip.xyz/",
    "https://bet-hokigg.pro/",
    "https://betgg-hoki.online/"
]

def start(update: Update, context: CallbackContext) -> None:
    update.message.reply_text(
        "🤖 **Bot Pengecek URL**\n"
        "Gunakan perintah:\n"
        "- /start : Panduan\n"
        "- /checkall : Cek semua URL yang sudah ditentukan\n"
        "- /check <url> : Cek URL tertentu\n"
        "Contoh: `/check https://google.com`",
        parse_mode="Markdown"
    )

def check_url(update: Update, context: CallbackContext) -> None:
    if not context.args:
        update.message.reply_text("❌ Mohon sertakan URL. Contoh: `/check https://google.com`", parse_mode="Markdown")
        return

    url = context.args[0]
    if not url.startswith(('http://', 'https://')):
        url = 'https://' + url

    try:
        response = requests.get(url, timeout=20)
        if response.status_code == 200:
            update.message.reply_text(f"✅ **{url}** aktif (Status: 200)")
        else:
            update.message.reply_text(f"⚠️ **{url}** error (Status: {response.status_code})")
    except Exception as e:
        update.message.reply_text(f"❌ **{url}** tidak dapat diakses. Error: `{str(e)}`", parse_mode="Markdown")

def check_all_urls(update: Update, context: CallbackContext) -> None:
    update.message.reply_text("🔍 Memulai pengecekan semua URL...")
    for url in URL_LIST:
        try:
            response = requests.get(url, timeout=10)
            if response.status_code == 200:
                update.message.reply_text(f"✅ **{url}** aktif (Status: 200)")
            else:
                update.message.reply_text(f"⚠️ **{url}** error (Status: {response.status_code})")
        except Exception as e:
            update.message.reply_text(f"❌ **{url}** tidak dapat diakses. Error: `{str(e)}`", parse_mode="Markdown")
    update.message.reply_text("✅ Pengecekan selesai!")

def main():
    updater = Updater(TOKEN)
    dispatcher = updater.dispatcher

    # Tambahkan handler perintah
    dispatcher.add_handler(CommandHandler("start", start))
    dispatcher.add_handler(CommandHandler("check", check_url))
    dispatcher.add_handler(CommandHandler("checkall", check_all_urls))

    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()
