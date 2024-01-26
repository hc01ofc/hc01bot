import os
import discord
import requests

# Importa a biblioteca do OpenAI
from openai import OpenAI

# Token do bot do Discord
TOKEN = os.getenv("MTIwMDIxNTUzNDg2NDY5OTQwMg.Gki_xQ.h8MVX8kiAmcLdDAXkZXfxHPhncoOGWjwzaIYuI")

# Cria uma instância do bot
intents = discord.Intents.default()
client = discord.Client(intents=intents)

# Função para enviar uma mensagem
def send_message(channel, message):
    channel.send(message)

# Função para banir um usuárioprompt = message.content
def ban_user(guild, user):
    guild.ban(user)

# Função para expulsar um usuário
def kick_user(guild, user):
    guild.kick(user)

# Função para desbanir um usuário
def unban_user(guild, user):
    guild.unban(user)

# Função para desexpulsar um usuário
def unkick_user(guild, user):
    guild.unban(user)

# Função para iniciar uma conversa com o bot
def chat_with_bot(channel, prompt):
    # Inicializa o OpenAI
    openai = OpenAI(api_key="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")

    # Gera uma resposta do bot
    response = openai.create(prompt=prompt, engine="davinci")

    # Envia a resposta do bot para o canal
    send_message(channel, response)

# Função para gerar uma imagem
def image_gen(channel, prompt):
    # Inicializa o OpenAI
    openai = OpenAI(api_key="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")

    # Gera uma imagem do prompt
    response = openai.create(prompt=prompt, engine="davinci", media_type="image")

    # Envia a imagem para o canal
    send_message(channel, response)

# Função para tocar uma música
def play_music(channel, url):
    # Abre uma conexão com o YouTube
    response = requests.get(url)

    # Extrai o código da música
    code = response.text.split("=")[1]

    # Toca a música
    channel.play(code)

# Função para parar a música
def stop_music(channel):
    channel.stop()

# Função para pular a música
def skip_music(channel):
    channel.skip()

# Eventos do bot
@client.event
async def on_ready():
    print("Bot pronto!")

@client.event
async def on_message(message):
    # Verifica se o comando é /say
    if message.content.startswith("/say"):
        # Obtém o texto da mensagem
        text = message.content[4:]

        # Envia a mensagem para o canal
        send_message(message.channel, text)

    # Verifica se o comando é /ban
    if message.content.startswith("/ban"):
        # Obtém o nome do usuário
        user = message.content[4:]

        # Banir o usuário
        ban_user(message.guild, user)

    # Verifica se o comando é /kick
    if message.content.startswith("/kick"):
        # Obtém o nome do usuário
        user = message.content[5:]

        # Expulsar o usuário
        kick_user(message.guild, user)

    # Verifica se o comando é /unban
    if message.content.startswith("/unban"):
        # Obtém o nome do usuário
        user = message.content[6:]

        # Desbanir o usuário
        unban_user(message.guild, user)

    # Verifica se o comando é /unkick
    if message.content.startswith("/unkick"):
        # Obtém o nome do usuário
        user = message.content[7:]
        # Desexpulsar o usuário
        unkick_user(message.guild, user)
