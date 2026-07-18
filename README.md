# Vibecoding Course Skills

Набор скиллов курса по вайб-кодингу, упакованный как плагин Claude Code.
Проводит проект по цепочке: **спецификация → инициализация → дизайн → экраны**,
плюс аккуратные коммиты. Скиллы написаны на русском и объясняют каждый шаг
человеческим языком, прежде чем что-то делать.

## Что внутри

### Спецификация — план продукта до начала кода

| Скилл | Что делает |
|-------|------------|
| `create-spec` | Оркестратор: проводит идею через все три слоя спеки подряд с паузой на одобрение после каждого. Результат — три файла в `docs/spec/` |
| `spec-requirements` | Слой 1 — функциональные требования: «ЧТО продукт делает и ДЛЯ КОГО» → `docs/spec/01-functional-requirements.md` |
| `spec-architecture` | Слой 2 — техническая архитектура: «НА ЧЁМ собран, из каких компонентов» → `docs/spec/02-tech-architecture.md` |
| `spec-dev-architecture` | Слой 3 — девелоперская архитектура: «КАК запускать и как ИИ проверяет сам себя» → `docs/spec/03-dev-architecture.md` |
| `spec-user-flows` | Развёртка спеки в user flows: экраны, состояния, тексты ошибок → `docs/spec/04-user-flows.md` |
| `gather-context` | Скилл-интервьюер: задаёт вопросы по одному, к каждому предлагает рекомендованный ответ. Вызывается этапами спеки, работает и сам по себе |

Черновики каждого слоя проверяет отдельный агент-критик **`spec-critic`** (идёт в комплекте).

### Инициализация проекта

| Скилл | Что делает |
|-------|------------|
| `init-project` | Превращает спеку в живое рабочее место: проверка инструментов, git, каркас приложения, локальное окружение, `CLAUDE.md`, первый коммит. Безопасность — с первого дня |

### Дизайн и экраны

| Скилл | Что делает |
|-------|------------|
| `setup-ui` | Оркестратор дизайна: оба этапа подряд с паузой между ними |
| `choose-ui-kit` | Этап 1 — выбор и установка UI kit (против неконсистентности интерфейса) |
| `customise-ui-kit` | Этап 2 — тема, шрифты, паспорт внешности `DESIGN.md` (против «ИИшного» дизайна) |
| `build-screens` | Сборка экранов по user flows на заглушках — рабочий UI раньше бэкенда, с UI-тестом на каждый поток |

### Git

| Скилл | Что делает |
|-------|------------|
| `commit` | Аккуратные коммиты: только изменения сессии, несвязанные правки — раздельно, проверка на секреты и мусор до `git add`, план коммитов на подтверждение |

## Установка

Плагин ставится с GitHub через маркетплейс: маркетплейс подключается один раз,
затем из него ставится плагин.

### Шаг 1 — подключить маркетплейс

Внутри Claude Code:

```
/plugin marketplace add a-v-ershov/vibecoding_course_skills
```

### Шаг 2 — установить плагин

**Глобально** (для всех проектов на этой машине — это скоуп по умолчанию):

```
/plugin install vibecoding-course@vibecoding-course
```

**Только в текущий проект** — из терминала, с флагом `--scope`:

```bash
# в .claude/settings.json — попадёт в git, увидит вся команда
claude plugin install vibecoding-course@vibecoding-course --scope project

# в .claude/settings.local.json — только у вас, в git не попадёт
claude plugin install vibecoding-course@vibecoding-course --scope local
```

Тот же флаг работает и для маркетплейса:
`claude plugin marketplace add a-v-ershov/vibecoding_course_skills --scope project` —
тогда маркетплейс пропишется в настройки проекта, и Claude Code предложит его коллегам
при первом запуске.

После установки перезапустите Claude Code. Скиллы появятся с префиксом `vibecoding-course:`.

### Управление

- `/plugin` — интерактивное меню: включить/выключить плагин, посмотреть состав.
- `claude plugin disable vibecoding-course` / `claude plugin enable vibecoding-course` — выключить и включить, не удаляя.
- `claude plugin uninstall vibecoding-course` — удалить.

## Использование

Полный путь нового проекта:

```
/vibecoding-course:create-spec «идея продукта в одном-двух предложениях»
/vibecoding-course:spec-user-flows
/vibecoding-course:init-project
/vibecoding-course:setup-ui
/vibecoding-course:build-screens
/vibecoding-course:commit
```

Каждый скилл работает и сам по себе — можно запускать любой этап отдельно
(например, только `/vibecoding-course:commit` в существующем проекте).

## Обновление

Установленный плагин подхватывает изменения после повышения `version`
в `.claude-plugin/plugin.json` и `.claude-plugin/marketplace.json`:

```
/plugin marketplace update vibecoding-course
/plugin update vibecoding-course@vibecoding-course
```

И перезапустите Claude Code. Либо включите автообновление: `/plugin` →
**Marketplaces** → `vibecoding-course` → **Enable auto-update**.

Пока плагин активно дорабатывается, поле `version` можно убрать из обоих
манифестов — тогда новой версией считается каждый git-коммит, и bump не нужен.

## Структура репозитория

```
.claude-plugin/
  plugin.json        # манифест плагина
  marketplace.json   # манифест маркетплейса (репозиторий сам себе маркетплейс)
skills/              # 12 скиллов, по папке на скилл (SKILL.md + references/)
agents/
  spec-critic.md     # агент-критик, проверяет черновики спеки
```
