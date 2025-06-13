import logging
from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes
import random

Настройка логирования
logging.basicConfig(
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    level=logging.INFO
)
logger = logging.getLogger(__name__)

Эко советы
ECO_TIPS = [
    "♻️ Используйте многоразовые сумки вместо пластиковых пакетов",
    "💡 Выключайте свет при выходе из комнаты",
    "🚰 Установите аэраторы на краны для экономии воды",
    "🚲 Чаще ходите пешком или используйте велосипед вместо авто",
    "🌱 Начните компостировать органические отходы",
    "📱 Сдавайте старую электронику на переработку",
    "☀️ Установите солнечные батареи для получения чистой энергии",
    "🥬 Покупайте местные сезонные продукты для сокращения транспортного следа",
    "💧 Откажитесь от бутилированной воды в пользу фильтрованной",
    "📄 Используйте обе стороны бумаги при печати",
    "👕 Покупайте качественную одежду вместо быстрой моды",
    "🌳 Сажайте деревья и поддерживайте лесные инициативы",
    "🔌 Отключайте зарядные устройства из розетки после использования",
    "🛍️ Выбирайте продукты с минимальной упаковкой",
    "♨️ Утеплите окна и двери для экономии энергии на отоплении",
    "🌿 Замените одноразовые предметы многоразовыми альтернативами",
    "🏠 Установите умные термостаты для оптимизации энергопотребления",
    "🍽️ Планируйте питание, чтобы сократить пищевые отходы",
    "🔄 Сортируйте мусор и сдавайте вторсырьё на переработку",
    "🚿 Уменьшите время приёма душа на 2 минуты для экономии воды"
]

Обработчики команд
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user = update.effective_user
    await update.message.reply_html(
        f"Привет {user.mention_html()}! Я помогу тебе сохранить планету 🌍\n\n"
        "<b>Доступные команды:</b>\n"
        "/start - Начать работу\n"
        "/tip - Получить случайный совет\n"
        "/help - Помощь и информация\n"
        "/alltips - Все советы сразу\n\n"
        "Каждый маленький шаг имеет значение!"
    )

async def eco_tip(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    tip = random.choice(ECO_TIPS)
    await update.message.reply_text(f"🌟 Экосовет:\n\n{tip}")

async def all_tips(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    tips_list = "\n\n".join([f"🌿 {tip}" for tip in ECO_TIPS])
    await update.message.reply_text(f"📚 Все экосоветы ({len(ECO_TIPS)} шт):\n\n{tips_list}")

async def help_command(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(
        "❓ Как пользоваться ботом:\n"
        "/start - Начало работы\n"
        "/tip - Случайный экосовет\n"
        "/alltips - Все советы списком\n\n"
        "Версия 1.0 • Бот для устойчивого будущего!"
    )

def main() -> None:
    TOKEN = "Токен от BotFather"
    
    Создаем Application
    application = Application.builder().token(TOKEN).build()
    
    Регистрируем обработчики команд
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("tip", eco_tip))
    application.add_handler(CommandHandler("alltips", all_tips))
    application.add_handler(CommandHandler("help", help_command))
    
    Запускаем бота
    logger.info("Бот запущен...")
    application.run_polling()

if __name__ == '__main__':
    main()
