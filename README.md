# Telegram-bot
AI girlfriend roulette 
import telebot
import random
import os

# Load token from env (secure for hosting)
BOT_TOKEN = os.getenv('BOT_TOKEN', '8315880997:AAGfEHMdiSAZdl_vUgAERi55gJ-kUMEf6ZA')
AFFILIATE_LINK = 'https://www.aiallure.com/ai-influencers?ref=34t6p1IHekHcPqypSxjFqBLy757'

bot = telebot.TeleBot(BOT_TOKEN)

# SFW Influencer Profiles (add more via Imgur URLs)
influencers = [
    {
        'name': 'Luna',
        'image': 'https://i.imgur.com/9z3xK1L.jpg',
        'tease': 'Flirty DM auto-replier for IG fans – build her now!'
    },
    {
        'name': 'Mia',
        'image': 'https://i.imgur.com/XyZ456.jpg',
        'tease': 'Viral TikTok content gen – unlimited images & subs income!'
    },
    {
        'name': 'Zoe',
        'image': 'https://i.imgur.com/Def789.jpg',
        'tease': '24/7 personality chat – scale your audience on autopilot!'
    }
]

@bot.message_handler(commands=['start'])
def start(message):
    markup = telebot.types.InlineKeyboardMarkup()
    spin_btn = telebot.types.InlineKeyboardButton('🎰 SPIN MY AI INFLUENCER', callback_data='spin')
    markup.add(spin_btn)
    bot.send_message(
        message.chat.id,
        '🎰 *AI Influencer Roulette*\n\n'
        'Spin for a custom AI persona that runs your socials – FREE start! 😎\n'
        'Design looks, auto-chat fans, & grow on IG/TikTok.',
        parse_mode='Markdown',
        reply_markup=markup
    )

@bot.callback_query_handler(func=lambda call: call.data == 'spin')
def spin(call):
    pick = random.choice(influencers)
    markup = telebot.types.InlineKeyboardMarkup()
    link_btn = telebot.types.InlineKeyboardButton('👉 Build Her FREE', url=AFFILIATE_LINK)
    markup.add(link_btn)
    bot.send_photo(
        call.message.chat.id,
        pick['image'],
        caption=f'💖 *{pick["name"]} is Your New AI Star!*\n\n'
                f'{pick["tease"]}\n\n'
                f'🔥 Unlimited tools | Earn from DMs & content\n'
                f'👇 Launch her today!',
        parse_mode='Markdown',
        reply_markup=markup
    )
    bot.answer_callback_query(call.id)

if __name__ == '__main__':
    print('Bot starting...')
    bot.infinity_polling()
