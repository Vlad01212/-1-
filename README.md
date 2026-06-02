from flask import Flask, render_template, request
import random

app = Flask(__name__)

# Данные по RDR2
characters = [
    {"name": "Артур Морган", "role": "Главный герой", "image": "🤠"},
    {"name": "Датч Ван дер Линде", "role": "Лидер банды", "image": "👨"},
    {"name": "Джон Марстон", "role": "Член банды", "image": "🤠"},
    {"name": "Сэди Адлер", "role": "Отважная участница", "image": "👩"},
    {"name": "Чарльз Смит", "role": "Опытный боец", "image": "💪"}
]

locations = [
    {"name": "Блэкуотер", "description": "Место неудачного ограбления", "image": "🏘️"},
    {"name": "Сен-Дени", "description": "Крупный город в стиле Нового Орлеана", "image": "🌆"},
    {"name": "Гуарма", "description": "Тропический остров", "image": "🌴"},
    {"name": "Бивер-Холлоу", "description": "Последняя база банды", "image": "⛺"},
    {"name": "Амбарино", "description": "Снежные горы", "image": "❄️"}
]

activities = [
    "Охота на диких животных",
    "Ограбление поездов",
    "Покер с бандитами",
    "Рыбалка на реке",
    "Скачки на лошадях",
    "Поиск сокровищ",
    "Дуэли на улицах"
]

@app.route('/')
def home():
    featured_character = random.choice(characters)
    featured_location = random.choice(locations)
    return render_template('index.html',
                      character=featured_character,
                      location=featured_location)

@app.route('/characters')
def characters_page():
    return render_template('characters.html', characters=characters)

@app.route('/locations')
def locations_page():
    return render_template('locations.html', locations=locations)

@app.route('/activities')
def activities_page():
    random_activity = random.choice(activities)
    return render_template('activities.html', activity=random_activity)

@app.route('/trivia', methods=['GET', 'POST'])
def trivia():
    questions = [
        {"question": "Как зовут главного героя RDR2?", "answer": "артур морган"},
        {"question": "Кто лидер банды Ван дер Линде?", "answer": "датч ван дер линде"},
        {"question": "В каком году происходит действие игры?", "answer": "1899"},
        {"question": "Как называется последняя глава игры?", "answer": "бивер-холлоу"}
    ]
    
    result = None
    if request.method == 'POST':
        user_answer = request.form['answer'].lower().strip()
        correct_answer = request.form['correct_answer']
        if user_answer == correct_answer:
            result = "Правильно! Молодец, настоящий ковбой!"
        else:
            result = f"Неверно! Правильный ответ: {correct_answer.title()}"
    
    question_data = random.choice(questions)
    return render_template('trivia.html', question=question_data, result=result)

if __name__ == '__main__':
    app.run(debug=True)

