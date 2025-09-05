import logging
import requests
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

# Enable logging
logging.basicConfig(
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
    level=logging.INFO
)

# Bot token from BotFather
BOT_TOKEN = "7804126975:AAFdylLGmXxejFgBtmgjmxjs-QXSX-6RJJA"

# Project links
WEBSITE = "https://www.vyronex.xyz"
PCS_SWAP = "https://pancakeswap.finance/swap?inputCurrency=0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c&outputCurrency=0xeb55a55c384095ced21587afbe7418b7c9ae40cb&chainId=56"
PCS_PAIR = "https://pancakeswap.finance/info/pairs/0x413a061d07bd17cdbcfaf3ab4b28d1118295136d"
BSCSCAN_CONTRACT = "https://bscscan.com/token/0xeb55A55C384095cED21587aFbe7418b7c9AE40cb"

# Start command
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        f"Welcome to VyronexVNX (VNX)!\n\n"
        f"Website: {WEBSITE}\n"
        f"Swap: {PCS_SWAP}\n"
        f"Pair Info: {PCS_PAIR}\n"
        f"Contract: {BSCSCAN_CONTRACT}"
    )

# About command
async def about(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "VyronexVNX (VNX) is a crypto token built on Binance Smart Chain (BSC).\n"
        "Total Supply: 10,000,000,000 VNX\n"
        "Decimals: 18\n"
        "Symbol: VNX"
    )

# Price command (VNX in USD)
async def price(update: Update, context: ContextTypes.DEFAULT_TYPE):
    try:
        # Get pair info from PancakeSwap GraphQL
        url = "https://open-platform.nodereal.io/6041f10687f14750b5c9bac8cd754ee6/pancakeswap-free/graphql"
        query = {
            "query": """
            {
              pair(id: "0x413a061d07bd17cdbcfaf3ab4b28d1118295136d") {
                token0Price
                reserveUSD
              }
            }
            """
        }
        r = requests.post(url, json=query)
        data = r.json()["data"]["pair"]
        vnx_in_bnb = float(data['token0Price'])

        # Fetch BNB price in USD from CoinGecko
        coingecko = requests.get("https://api.coingecko.com/api/v3/simple/price?ids=binancecoin&vs_currencies=usd").json()
        bnb_in_usd = float(coingecko['binancecoin']['usd'])

        # Calculate VNX price in USD
        vnx_in_usd = vnx_in_bnb * bnb_in_usd
        reserve_usd = float(data['reserveUSD'])

        await update.message.reply_text(
            f"📊 VNX Price Info:\n"
            f"1 VNX = ${vnx_in_usd:,.6f} USD\n"
            f"Liquidity Pool Reserve: ${reserve_usd:,.2f}"
        )
    except Exception as e:
        await update.message.reply_text(f"❌ Error fetching price data: {e}")

# Other commands
async def staking(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Staking options will be announced soon on our website.")

async def farming(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Farming pools will be listed on PancakeSwap.")

async def airdrop(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Stay tuned for upcoming airdrops! Details will be published on our website.")

async def wallet(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Wallet connection coming soon. You will be able to check your balance here.")

async def news(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Latest updates will be posted on https://www.vyronex.xyz")

async def support(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("For support, please contact our team via the website.")

async def help_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "/start - Begin your VyronexVNX journey\n"
        "/about - Learn about VyronexVNX & the bot\n"
        "/price - Check the current VNX token price in USD\n"
        "/staking - View staking options & rewards\n"
        "/farming - Explore available farming pools\n"
        "/airdrop - Get details about ongoing or upcoming airdrops\n"
        "/wallet - Connect or check your wallet balance\n"
        "/news - Latest updates & announcements\n"
        "/support - Contact the support team\n"
        "/help - Show all available commands"
    )

if __name__ == "__main__":
    app = ApplicationBuilder().token(BOT_TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("about", about))
    app.add_handler(CommandHandler("price", price))
    app.add_handler(CommandHandler("staking", staking))
    app.add_handler(CommandHandler("farming", farming))
    app.add_handler(CommandHandler("airdrop", airdrop))
    app.add_handler(CommandHandler("wallet", wallet))
    app.add_handler(CommandHandler("news", news))
    app.add_handler(CommandHandler("support", support))
    app.add_handler(CommandHandler("help", help_command))

    app.run_polling()
