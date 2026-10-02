# Roblox: заработок на картах и локациях и сборка игр вместе с Claude

_Исследование от 2 октября 2026 г. Источники указаны в конце каждого раздела. Цифры и правила Roblox меняются часто, поэтому перед решениями сверяйтесь с create.roblox.com._

---

## 1. Короткий вывод

1. **Отдельной официальной программы «Roblox платит за карту внутри чужой игры» нет.** Ни в материалах RDC 2025/2026, ни в документации такой программы я не нашёл. Слух, скорее всего, смешивает четыре реальных способа заработка (см. раздел 2).
2. **Реальные способы получать деньги за локации и карты:**
   - **Своя игра (experience)**: Game Passes, Developer Products, Creator Rewards, Roblox Plus, платный доступ. Это основной и самый масштабируемый путь.
   - **Продажа моделей и карт в Creator Store за доллары (USD)** с декабря 2025 г.: 100% чистой выручки, минимальная цена $2.99.
   - **Карты на заказ (commissions / Talent Hub)**: студии платят билдерам за готовые карты.
   - **Roblox Build**: мобильное приложение, где ИИ собирает игру по промпту; пока это alpha в отдельных странах.
3. **Claude может строить карты напрямую.** С февраля 2026 г. в Roblox Studio есть **встроенный MCP-сервер**, и в нём есть кнопка Quick connect для **Claude Code** и **Claude Desktop**. Claude может читать и менять открытый плейс, выполнять Luau, генерировать меши и материалы, вставлять ассеты, запускать плейтест и управлять персонажем.
4. **Ограничение облачной сессии.** Roblox Studio работает только на Windows и macOS. В облачном Linux-контейнере этой сессии Studio не запустится. Поэтому есть две схемы:
   - **Живая сборка**: Studio и Claude Code/Desktop на вашем компьютере, подключённые через MCP.
   - **Код в репозитории**: я пишу скрипты, генераторы карт и системы в этом репо (структура Rojo), а вы синхронизируете их в Studio.

---

## 2. Как на Roblox зарабатывают деньги (актуально на 2026 г.)

### 2.1 DevEx: вывод Robux в реальные деньги
- Курс: **$0.0038 за 1 Earned Robux** (с 5 сентября 2025 г.; раньше было $0.0035). 30 000 R$ = $114.
- Повышенный курс **$0.0054** для части покупок от игроков из США 18+ с подтверждённым возрастом.
- Минимум для вывода: 30 000 Earned Robux. Нужны верификация, возраст 13+ и аккаунт в хорошей репутации.
- **Roblox Wallet** (анонс на RDC 2026): ежедневные выплаты в долларах для независимых креаторов из США 18+, запуск во второй половине 2026 г. Другие страны и местные валюты запланированы на 2027 г. **Roblox Card** выйдет в 2027 г.

### 2.2 Монетизация своей игры
| Механика | Что получает автор |
|---|---|
| Game Passes, Developer Products | 70% Robux (30% забирает маркетплейс) |
| Подписки внутри игры | 70% в первый месяц, потом 100% |
| Платный доступ (paid access, оплата в местной валюте, только на ПК и в вебе) | 50% при цене $9.99, 60% при $29.99, 70% при $49.99 |
| Private servers | Robux от продажи доступа и выплаты от Roblox Plus (см. ниже) |

### 2.3 Creator Rewards (с 24 июля 2025 г., заменили Premium Payouts)
- **Daily Engagement**: **5 R$ в день** за каждого Active Spender, который провёл в вашей игре 10 минут и больше, если это одна из первых трёх игр, которые он запустил за день. Active Spender — игрок, потративший $9.99 и больше за последние 60 дней.
- **Audience Expansion**: **35% от первых $100 покупок** нового или вернувшегося игрока, которого привела ваша игра (share-ссылка, прямая ссылка или поиск). Условие: в игре в среднем 100+ DAU на протяжении 60 дней. Нужны ID-верификация и DevEx-аккаунт.
- Холд 60 дней. Потолка выплат нет.

### 2.4 Roblox Plus (с 30 апреля 2026 г., $4.99 в месяц, заменил Premium)
- Скидки подписчикам на покупки в играх оплачивает Roblox, автор получает полную цену.
- Выплаты за время, которое подписчики провели в платных приватных серверах (до 100 R$ за подписчика).
- **До 750 R$ за каждого нового подписчика**, который оформил подписку через `PromptRobloxSubscriptionPurchase` в вашей игре (250 R$ в месяц в течение 3 месяцев).

### 2.5 Creator Store: продажа моделей и карт за USD (с 10 декабря 2025 г.)
- Продавать можно модели, в том числе целые карты и наборы окружения. Цена только в USD, **минимум $2.99**.
- **100% чистой выручки** (вычитаются только налоги и комиссия платёжной системы). Нет комиссии маркетплейса и нет курса DevEx.
- Требования: ID-верификация, 18+ (или 13–17 с согласием родителей), страна, которую поддерживает **Stripe**, ассет принадлежит пользователю, а не группе, меньше 15 000 зависимостей, нельзя вкладывать чужие restricted меши и картинки.
- **Минус**: DRM нет, украденные ассеты можно оспорить только через Rights Manager.
- ⚠️ Для России: Stripe не работает с резидентами РФ. Нужно проверить, в какой стране у вас есть право открыть Stripe-аккаунт.

### 2.6 Карты на заказ
- Заказы ищут на DevForum (раздел Talent Hub / Collaboration) и в Discord-сообществах билдеров.
- Ориентиры по ценам из открытых портфолио: простые карты от 50 до 300 R$ у новичков; у опытных билдеров от **$100–500+** за карту или **50 000+ R$** за крупную карту.
- Комиссии дают быстрый доход, но не масштабируются. Хороший вариант для портфолио и первых денег.

### 2.7 Реалистичные ожидания
- За год до 30.06.2026 через DevEx выплачено **≈ $1.7 млрд**, это +50% год к году.
- **Медианный участник DevEx получил ≈ $1 500 за год.** Топ-1000 в среднем получили $1.4 млн, топ-10 — $65.7 млн.
- Заработок распределён по степенному закону: решают удержание игроков (retention), частота обновлений и продвижение, а не сама карта.

Источники: [DevEx docs](https://create.roblox.com/docs/en-us/production/monetization/developer-exchange.md) · [Creator Rewards docs](https://create.roblox.com/docs/creator-rewards) · [Introducing Creator Rewards](https://devforum.roblox.com/t/introducing-creator-rewards-earn-more-by-growing-the-community/3777628) · [Roblox Plus docs](https://create.roblox.com/docs/en-us/production/monetization/roblox-plus.md) · [Tubefilter: Roblox Plus payouts](https://tubefilter.com/2026/04/13/roblox-plus-creator-payouts) · [Buy and Sell Models on the Creator Store](https://devforum.roblox.com/t/buy-and-sell-models-on-the-creator-store/4142112) · [Creator Store docs](https://create.roblox.com/docs/production/creator-store) · [Paid access rev share](https://devforum.roblox.com/t/earn-a-higher-revenue-share-for-999-paid-access-experiences/3294010) · [RDC 2026 newsroom](https://about.roblox.com/newsroom/2026/09/rdc-2026-the-world-needs-more-play) · [GamesBeat: Wallet & Card](https://gamesbeat.com/roblox-unveils-creator-wallets-and-physical-cards-rdc-2026/) · [91mobiles: RDC 2026](https://www.91mobiles.com/gaming/roblox-announces-standalone-game-apps-at-rdc-2026/) · [RDC 2025](https://about.roblox.com/newsroom/2025/09/roblox-rdc-2025)

---

## 3. Что нового для создания карт (RDC 2025–2026)

- **Roblox Build**: мобильное приложение (появляется и десктоп-доступ), где игра собирается по промпту. Из ≈9 000 опубликованных игр 71% сделали люди, которые раньше не пользовались Studio. Public alpha пока доступна в **Новой Зеландии, Сербии и Сингапуре**. Обещана связка с Studio.
- **Scene Generator** (запуск во второй половине 2026 г., в Build и Studio): по тексту и референс-картинкам генерирует функциональную раскладку сцены, которую потом дорабатывают в Studio. **Это ключевой инструмент для генерации локаций.**
- **Cube 3D / 4D**: генерация мешей и «функциональных объектов» (например, машина, на которой можно ехать, или дверь, которая открывается). Есть in-experience creation: игроки создают объекты прямо в игре по тексту.
- **Roblox Everywhere**: игру можно опубликовать как отдельное приложение для мобильных устройств, ПК и консолей.
- **Moments**: лента коротких видео из игр для продвижения, полностью запущена в США.

Источники: [allthings.how: RDC 2026](https://allthings.how/roblox-rdc-every-major-announcement-for-players-and-creators/) · [PocketGamer.biz: RDC 2026](https://www.pocketgamer.biz/roblox-unveils-new-play-creation-and-monetisation-tools-at-rdc-2026/) · [4D early access](https://devforum.roblox.com/t/early-access-introducing-in-experience-4d-functional-objects-and-enhanced-3d-generation/4050893)

---

## 4. Как Claude строит карты в Roblox Studio (официальный MCP)

### 4.1 Настройка (на вашем ПК)
1. Обновите Roblox Studio до последней версии и откройте плейс.
2. Откройте **Assistant**, затем **… → Manage MCP Servers** и включите **Enable Studio as MCP server**.
3. Нажмите **Quick connect** для Claude Code или Claude Desktop и при необходимости перезапустите клиент. Можно подключить и вручную:

**Windows** (`claude_desktop_config.json` или `.mcp.json` для Claude Code):
```json
{
  "mcpServers": {
    "Roblox_Studio": {
      "command": "cmd.exe",
      "args": ["/c", "%LOCALAPPDATA%\\Roblox\\mcp.bat"]
    }
  }
}
```
**macOS:**
```json
{
  "mcpServers": {
    "Roblox_Studio": {
      "command": "/Applications/RobloxStudio.app/Contents/MacOS/StudioMCP"
    }
  }
}
```
4. Если в настройках MCP горит зелёный индикатор со счётчиком клиентов, подключение работает.

> Безопасность: MCP-клиент получает полный доступ на чтение и запись к открытому плейсу. Подключайте только доверенные клиенты и делайте бэкапы (File → Save to File) перед крупными правками.

### 4.2 Инструменты, которые получает Claude
| Группа | Инструменты | Зачем для карт |
|---|---|---|
| Скрипты | `script_read`, `multi_edit`, `script_search`, `script_grep` | читать и править код игры |
| Генерация | `generate_mesh`, `generate_material`, `generate_procedural_model`, `wait_job_finished` | ИИ-меши, материалы, процедурные модели |
| Ассеты | `search_asset`, `insert_asset`, `upload_image`, `store_image` | искать модели в Creator Store и вставлять в карту |
| Исследование плейса | `search_game_tree`, `inspect_instance`, `subagent` | понять структуру Workspace |
| Выполнение | `execute_luau` | **главный инструмент**: процедурная генерация террейна, построек, расстановки объектов |
| Тестирование | `get_studio_state`, `start_stop_play`, `get_console_output`, `screen_capture` | запуск плейтеста, логи, **скриншоты для визуальной проверки** |
| Симуляция игрока | `character_navigation`, `user_keyboard_input`, `user_mouse_input` | пройти obby или карту, проверить UI |
| Справка | `http_get`, `skill` | документация |
| Сессии | `list_roblox_studios`, `set_active_studio` | несколько окон Studio сразу |

### 4.3 Цикл работы, который мы будем использовать
1. **Спецификация**: жанр, размер карты, биомы, точки интереса, маршруты игрока, где стоят монетизационные точки.
2. **Генерация блокаута** через `execute_luau`: Terrain API (`Terrain:FillBlock`, `FillBall`, `WriteVoxels`), шумовые карты высот, расстановка Part/Model.
3. **Наполнение**: `search_asset` + `insert_asset` (бесплатные или купленные модели), `generate_mesh` и `generate_material` для уникальных объектов. После запуска Scene Generator будем пробовать его.
4. **Освещение и атмосфера**: Lighting, Atmosphere, Sky, PostEffects.
5. **Проверка**: `screen_capture` с разных ракурсов, `start_stop_play` и `character_navigation`, чтобы проверить, что маршрут проходим. Затем читаем `get_console_output` и чиним ошибки.
6. **Оптимизация**: число инстансов, StreamingEnabled, коллизии, LOD. Это важно для мобильных устройств, а там большинство игроков.

### 4.4 Альтернативы и дополнения
- **Rojo**: синхронизирует код из файлов (этот Git-репозиторий) в Studio. Код и генераторы карт можно версионировать и ревьюить, и я могу писать их из облачной сессии без Studio.
- Комьюнити-MCP (например, `boshyxd/robloxstudio-mcp`) и старый `Roblox/studio-rust-mcp-server` устарели по сравнению со встроенным сервером. Использовать встроенный.
- Ограничения встроенного MCP: контекст ограничен (большие документы не влезут), `character_navigation` двигает персонажа в обход реального ввода, а длинные автономные итерации со скриптами лучше проверять вручную.

Источники: [Studio MCP docs](https://create.roblox.com/docs/studio/mcp) · [Assistant updates: built-in MCP & playtest automation](https://devforum.roblox.com/t/assistant-updates-studio-built-in-mcp-server-and-playtest-automation/4474643) · [MCP updates & external LLM support](https://devforum.roblox.com/t/studio-mcp-server-updates-and-external-llm-support-for-assistant/4415631) · [Luis Mori: Roblox + MCP + Claude Code](https://luismori.dev/article/roblox-game-development-with-mcp/) · [Loadout: MCP setup](https://useloadout.com/blog/roblox-mcp-server-setup/) · [obby.fun: Claude for Roblox Studio](https://www.obby.fun/blog/claude-ai-roblox-studio)

---

## 5. Рекомендуемая стратегия заработка

**Этап 1. Быстрые деньги и портфолио (недели 1–4)**
- Вместе с Claude сделать 3–5 качественных **наборов карт и окружения** (лобби, obby-уровни, город, лес, хоррор-коридоры) и продавать их в **Creator Store по $3–15**, если есть доступ к Stripe. Параллельно предлагать карты на заказ на DevForum и в Discord.

**Этап 2. Своя игра (месяцы 1–3)**
- Выбрать жанр, в котором **контент = карты**. В таком жанре наша сильная сторона (быстрая генерация локаций) напрямую увеличивает удержание игроков:
  - **Obby / tower** (много уровней, дешёвые в производстве, хорошо монетизируются через пропуск стадий);
  - **Хоррор и исследование** (атмосферные локации);
  - **Tycoon и simulator с новыми зонами** (каждая новая зона = обновление и всплеск игроков).
- Монетизация с первого дня: Developer Products (skip stage, жизни, бусты), Game Passes, промпт подписки Roblox Plus (до 750 R$ за подписчика), private servers.
- Метрики, которые решают в алгоритме: D1/D7 retention, время сессии, конверсия в платящих. Creator Rewards платит за 10+ минут игры Active Spender'а.

**Этап 3. Масштабирование**
- Регулярные обновления с новыми картами (это сильная сторона ИИ-генерации), реклама Roblox Ads, Moments-клипы, а позже Roblox Everywhere.

---

## 6. Что нужно от вас

1. **Windows или Mac** с Roblox Studio и **Claude Code или Claude Desktop** на этой же машине. Включить MCP-сервер в Studio (раздел 4.1).
2. Аккаунт Roblox с **ID-верификацией**, возраст 13+ (для DevEx), лучше 18+.
3. Решить вопрос со **Stripe** для Creator Store и с выводом DevEx: в какой стране можно получать выплаты.
4. Выбрать стартовый жанр (рекомендация: obby или хоррор с упором на локации).
5. По желанию можно поставить **Rojo** (VS Code-плагин и Studio-плагин), чтобы код из этого репозитория синхронизировался в Studio.

Когда первые пункты будут готовы, я подготовлю в этом репозитории Rojo-проект с генераторами карт на Luau. Вы будете запускать их в Studio, а на машине с MCP я смогу строить и тестировать карты напрямую.
