# FAL AI MCP — Підключення до Cursor

## Що це дає

Генерація зображень прямо в Cursor через FAL AI:
- Іконки для iOS додатків
- Onboarding ілюстрації
- Outfit flatlay фото
- App Store скріншоти
- Будь-які зображення для проекту

**Вартість:** ~$0.001-0.003 за зображення (FLUX schnell)  
**$10 вистачить на ~3000-10000 зображень**

---

## Вимоги

- Mac з macOS 12+
- Cursor з Pro або Pro+ планом
- FAL AI акаунт

---

## Крок 0 — Встановлення Node.js

Перед підключенням потрібен Node.js. Є два способи:

### Спосіб 1 — Через NVM (рекомендовано)

Відкрий **Terminal** і виконай по черзі:

```bash
# 1. Встанови NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 2. Перезапусти термінал або виконай:
source ~/.zshrc

# 3. Встанови останню LTS версію Node.js
nvm install --lts

# 4. Перевір що встановилось
node --version
npm --version
```

Має вивести щось типу:
```
v24.14.0
10.9.0
```

### Спосіб 2 — Пряме встановлення

1. Зайди на [nodejs.org](https://nodejs.org)
2. Завантаж **LTS** версію (зелена кнопка)
3. Відкрий `.pkg` файл і встанови
4. Перевір в терміналі:

```bash
node --version
npm --version
```

---

## Крок 1 — Реєстрація на FAL AI

1. Зайди на [fal.ai](https://fal.ai)
2. Зареєструйся
3. Перейди в `Dashboard` → `API Keys`
4. Натисни **Create API Key**
5. Скопіюй ключ — виглядає так: `fal-xxxxxxxxxxxxx`

> **Поповнення балансу:** мінімум $10  
> `Dashboard` → `Billing` → `Add Credits`

---

## Крок 2 — Підключення до Cursor

Відкрий Cursor → `Settings` → `Tools & MCP` → **Add Custom MCP**

Додай до конфігу `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "fal-ai": {
      "command": "npx",
      "args": ["-y", "fal-ai-mcp-server"],
      "env": {
        "FAL_KEY": "твій_ключ_сюди"
      }
    }
  }
}
```

Збережи файл → перезапусти Cursor.

---

## Крок 3 — Перевірка

В `Settings → Tools & MCP` має з'явитись **зелена крапка** біля `fal-ai`.

В логах побачиш:
```
fal.ai MCP Server v2.1.1 running on stdio
Successfully connected
```

---

## Крок 4 — Використання

Відкрий **Agent режим** в Cursor і пиши:

### Одне тестове зображення:
```
Use fal-ai with model "fal-ai/flux/schnell" to generate
one outfit flatlay photo, clean white background,
800x600px, save to ./Assets/test_001.jpg
```

### Масова генерація (наприклад 40 зображень):
```
Use fal-ai with model "fal-ai/flux/schnell" to generate 
exactly 40 outfit flatlay photos.

Requirements for each image:
- Clean white studio background
- No people, no logos, no text
- Flat lay style from above
- 800x600px JPEG

Save files to: ./Assets/OutfitCovers/
Named: outfit_001.jpg through outfit_040.jpg

Generate one by one, confirm each saved file,
do not stop until all 40 are completed.
```

---

## Моделі FAL AI

| Модель | Ціна | Якість | Швидкість |
|--------|------|--------|-----------|
| `fal-ai/flux/schnell` | ~$0.003 | ⭐⭐⭐ | ⚡ Швидко |
| `fal-ai/flux/dev` | ~$0.012 | ⭐⭐⭐⭐ | Середня |
| `fal-ai/flux-pro` | ~$0.03 | ⭐⭐⭐⭐⭐ | Повільніше |

**Рекомендація:**
- Для тестування і плейсхолдерів → `flux/schnell`
- Для фінальних App Store матеріалів → `flux-pro`

---

## Workflow для iOS проекту

```
1. Cursor генерує UI код
2. FAL AI генерує зображення через MCP
3. Зображення зберігаються прямо в Xcode проект
4. XcodeBuildMCP білдить і запускає на симуляторі
5. Готовий додаток з реальними зображеннями
```

---

## Поради

- Використовуй **новий чат** для великих генерацій (контекст росте)
- Розбивай на частини: спочатку 1-20, потім 21-40
- Завжди тестуй одне зображення перед масовою генерацією
- Вказуй точний шлях збереження в промті

---

## Корисні посилання

- [fal.ai](https://fal.ai) — головний сайт
- [fal.ai/models](https://fal.ai/models) — всі доступні моделі
- [fal.ai/dashboard](https://fal.ai/dashboard) — баланс і API ключі
- [cursor.directory](https://cursor.directory/plugins) — інші MCP плагіни
