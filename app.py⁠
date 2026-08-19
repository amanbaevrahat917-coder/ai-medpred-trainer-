import streamlit as st
from openai import OpenAI

st.set_page_config(page_title="Тренажер Медпреда", page_icon="🩺", layout="centered")

st.title("🩺 Симулятор визита к врачу")
st.write("Тренажер навыков общения для медицинских представителей")

api_key = st.sidebar.text_input("Введите OpenAI API Key:", type="password")

SYSTEM_PROMPT = """
Ты — опытный врач-терапевт. К тебе на прием пришел медицинский представитель.
Твой характер: скептичный, очень занятой, требовательный к доказательной медицине и клиническим исследованиям.
Отвечай коротко (1-3 предложения), как реальный врач во время приема. Задавай вопросы по препарату, выдвигай возражения (дорого, привык к аналогам, нет времени).

ЕСЛИ ПОЛЬЗОВАТЕЛЬ НАПИШЕТ 'ФИНАЛ' ИЛИ 'ЗАВЕРШИТЬ':
Выйди из роли и дай краткий разбор визита:
1. Оценка визита (от 1 до 10)
2. Что было сделано хорошо
3. Главная ошибка медпреда
"""

if not api_key:
    st.info("💡 Введите ваш API-ключ в меню слева, чтобы начать диалог с врачом.")
    st.stop()

client = OpenAI(api_key=api_key)

if "messages" not in st.session_state:
    st.session_state.messages = [{"role": "system", "content": SYSTEM_PROMPT}]

for msg in st.session_state.messages:
    if msg["role"] != "system":
        with st.chat_message(msg["role"]):
            st.write(msg["content"])

if user_input := st.chat_input("Напишите сообщение врачу..."):
    st.session_state.messages.append({"role": "user", "content": user_input})
    with st.chat_message("user"):
        st.write(user_input)

    try:
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=st.session_state.messages,
            temperature=0.7
        )
        bot_reply = response.choices[0].message.content
        st.session_state.messages.append({"role": "assistant", "content": bot_reply})
        with st.chat_message("assistant"):
            st.write(bot_reply)
    except Exception as e:
        st.error(f"Ошибка при запросе к API: {e}")
