---
name: product-planner
description: >-
  Stage-gated product planning before coding: pick communication language;
  collect optional context; interview for intent/MVP; lock project state or
  greenfield stack; emit a finely granulated OpenSpec-ready change plan for
  weak LLMs and Multica; hand off first change and offer AGENTS.md links.
  Advance only via the word next; go back via prev. Use when the user wants
  product planning, MVP scope, roadmap, stack discovery, or pre-coding
  decomposition. Do not use for OpenSpec propose/apply, writing tasks.md, or
  implementing code.
license: MIT
metadata:
  version: "2.0.1"
---

# Product Planner

Провести пользователя от идеи до **одного списка мелких OpenSpec-пригодных изменений** (под слабые LLM и Multica). Код и `openspec/changes/*` **не** создавать; `/opsx:*` только предлагать.

Источник модели: `scratch_02.md` в корне репозитория skill (если доступен).

## Artifact root

Все артефакты сессии писать **в папку этого skill** (рядом с `SKILL.md`), не в `planning/` целевого проекта:

| Файл / папка | Этап |
| --- | --- |
| `00_pipeline_state.md` | 0+ |
| `02_context/` | 1 |
| `03_context_summary.md` | 1 (всегда) |
| `04_interview.md` | 2 |
| `01_project_state.md` | 3 |
| `05_change_plan.md` | 4 |

Шаблоны: `templates/`. Slash-команда старта: `.cursor/commands/product-planner.md` (и копия в `commands/`). Слова `next` / `prev` — **не** slash-команды, а ключевые слова в чате.

## Pipeline

`0 → 1 → 2 → 3 → 4 → 5`

| Stage | Name | Output |
| --- | --- | --- |
| 0 | Launch + language | `00_pipeline_state.md` |
| 1 | Context | `02_context/`, `03_context_summary.md` |
| 2 | Interview | `04_interview.md` |
| 3 | Project state / architecture | `01_project_state.md` |
| 4 | Change plan | `05_change_plan.md` |
| 5 | Handoff + AGENTS.md offer | chat |

Ревью репо / выбор стека — **после** интервью.

## Keywords (hard gates)

Это **слова в сообщении пользователя**, не Cursor slash-commands. Не писать их как `/next` или `/prev`.

| Keyword | Effect |
| --- | --- |
| `/product-planner` | Старт / перезапуск с этапа 0 (реальная slash-команда) |
| `next` | Единственный способ **вперёд** на следующий этап |
| `prev` | Назад на `current − 1` (не ниже 1) |
| `prev <N>` | На этап N (`1…5`), только назад или на текущий |

«Нет, дополнений нет» **не** двигает пайплайн — только слово `next`.

Когда пользователь пишет `next` или `prev` / `prev N`: прочитать этот SKILL + `00_pipeline_state.md` и выполнить правила ниже.

### End-of-stage (обязательно)

После завершения работы этапа:

> Есть ли у вас что дополнить в рамках этого этапа? Если нет — переход на следующий этап словом `next`.

Стой. Жди `next` | `prev` | дополнения.

### Start-of-stage (обязательно)

В начале каждого этапа — **одна короткая фраза**: зачем этап и что на выходе.

### Keyword `next`

1. Проверить: текущий этап завершён (`done` / `skipped`) или этап 0 только что отработал вход в 1.
2. Если нет — отказать и сказать, чего не хватает.
3. `current_stage += 1`; если следующий был `stale` — **перевыполнить**, не переиспользовать слепо.
4. Бриф сути → работа этапа → end-of-stage вопрос.

### Keywords `prev` / `prev <N>`

1. Вычислить N; вперёд через `prev` — **отклонить**.
2. `current_stage = N`; все этапы `> N` → `stale`.
3. Сообщить, что поздние артефакты устарели; бриф этапа N; продолжить **на N**; не автопрогонять до конца.
4. End-of-stage вопрос (или продолжение работы этапа, если ещё `in_progress`).

### Sync rules (`00_pipeline_state.md`)

Поддерживать файл по шаблону `templates/00_pipeline_state.md`.

- Статусы: `pending` → `in_progress` → `done` | `skipped`; откат → поздние `stale`.
- План (4) и handoff (5) невалидны, пока 2 или 3 не `done`.
- Stale-файлы не удалять сразу; при перезаписи можно пометить `supersedes`.
- Существенное изменение на текущем этапе при уже `done` поздних → пометить поздние `stale` (спросить; по умолчанию да для intent/стека/MVP).

## Language (stage 0)

Явно спросить язык переписки и артефактов → `communication_language` в `00_pipeline_state.md`.  
Дальше **вся** переписка и содержимое md — на этом языке (имена файлов и ключевые слова `next` / `prev` можно не переводить).  
Смена mid-pipeline — только по явной просьбе.

## Closed-question format

```text
Вопрос: …

Варианты:
1. [проверенные] …  ← рекомендуется: <причина>
2. [проверенные] …
3. [в тренде] …
4. …
5. …
6. Другое (опишите)
```

- ≤5 содержательных + 1 «Другое».
- Preferred с причиной; иначе «без рекомендации».
- На этапе 3 ветка B: маркеры `[проверенные]` / `[в тренде]` обязательны, где уместно.
- На этапе 2 маркеры групп опциональны.

## Stages

### 0 — Launch

1. Создать/сбросить `00_pipeline_state.md`.
2. Спросить язык (≤5 + Другое; preferred ≈ язык первого сообщения).
3. На выбранном языке объяснить правила: пайплайн; слова `next` / `prev`; end-of-stage вопрос; этап 1 можно пропустить (`03` всё равно); не кодим / не создаём OpenSpec; в конце предложим ссылки в `AGENTS.md`.
4. Сразу войти в этап 1 с брифом (отдельный `next` с 0 не нужен).

### 1 — Context

**Суть:** материалы в `02_context/` → `03_context_summary.md`; можно пропустить.

- Попросить положить файлы; сказать, что можно «пропустить» / skip.
- Есть файлы → саммари по каждому в `03`.
- Skip / пусто → всё равно создать `03` (`Status: empty|skipped`, `skip_context: true`) по шаблону.
- При `skip_context: true` дальше не опираться на inbox.

→ итог + end-of-stage → ждать `next`.

### 2 — Interview

**Суть:** intent / MVP / предпочтения → `04_interview.md`.

Опора: идея + `03` (если не skip). Стек репо — на этапе 3.

1. ≤5 **открытых** вопросов про intent.
2. Дальше закрытые (формат выше), последовательно.
3. После ответов сверять с `03`; противоречия уточнять; без silent-merge.
4. Tech-стек не форсировать здесь.
5. Писать в `04_interview.md`.

→ summary Confirmed / Assumption / Open + end-of-stage → ждать `next`.

### 3 — Project state / architecture

**Суть:** канон репо или целевой стек → `01_project_state.md`.

Сначала инспекция целевого проекта.

#### Branch A — не пустой репо

1. Снимок: языки, инфра/БД/данные/Docker/пакеты, фреймворки, агентские артефакты.
2. Вопрос: текущий стек — **канон**? (канон / канон+overrides / меняем / Другое).
3. При «меняем» — уточнения.
4. Запись `01` + сверка с `04`.

#### Branch B — empty / greenfield

Не угадывать стек. Воронка (каждый шаг ≤5 + Другое):

1. Архитектура → 2. языки/рантаймы (FE/BE по необходимости) → 3. фреймворки/подход → 4. БД/auth/инфра только если влияет на план.

**Рекомендации из контекста:** preferred и варианты из `04`, `03` (если был), предыдущих ответов воронки; назвать причину.

**Группы:** смешивать `[проверенные]` и `[в тренде]`; preferred обычно из проверенных; если из тренда — предупредить о риске.

**Поиск:** если есть web-search — перед шагами языков/фреймворков/UI kit/инфра поискать актуальные варианты под задачу; наполнить `[в тренде]`; одна строка источников в `01` или чате. Без поиска — честно сказать и опереться на знание + контекст.

Липкое (framework, форма BE/FE, UI kit если UI) — Confirmed, не тихий Assumption.

→ итог + end-of-stage → ждать `next`.

### 4 — Change plan (weak-LLM grain)

**Суть:** мелкий список changes → `05_change_plan.md`.

Вход: `04` + `01` + `03` (если не skip). Если 2/3 stale — не строить план.

#### Split flags (обязательная самопроверка)

Если срабатывает **любой** флаг — разрезать, **перелинковать**, задать порядок:

| Flag | When |
| --- | --- |
| `SPLIT_FILES` | > 3 файлов в одном change |
| `SPLIT_BE_FE` | один change трогает и BE, и FE |
| `SPLIT_DOMAIN` | несколько доменов |
| `SPLIT_FUNC` | разный функционал / две user-facing цели |

Избегать `full-stack` карточек — пара связанных BE/FE.  
Exception `thin-vertical` только если иначе нет проверяемого user-facing результата; пометить явно.

Карточка: ID `CH-00N`, slug, Goal, Scope, Acceptance, Category `BE`|`FE`, estimated files/areas, split flags, Depends-on / Enables / Related, reuse из `01`.

Отдельно: очередь реализации; что параллельно в Multica и почему.

→ итог + end-of-stage → ждать `next`.

### 5 — Handoff

**Суть:** первое (или первая параллельная пачка) change(s); стоп без кода/OpenSpec.

1. Goal / Scope / Acceptance / Deps / порядок.
2. Следовать `01` + `04` (+ `03` если был).
3. Предложить `/opsx:propose` для slug.
4. **Предложить** (не молча) добавить в `AGENTS.md` ссылки на артефакты; при согласии — короткий блок с фактическими путями:

```markdown
## Product Planner artifacts
- skills/product-planner/00_pipeline_state.md
- skills/product-planner/01_project_state.md
- skills/product-planner/03_context_summary.md
- skills/product-planner/04_interview.md
- skills/product-planner/05_change_plan.md
```

5. End-of-stage: дополнения к handoff / AGENTS.md? Если нет — skill завершён.

## Hard boundaries

**Do:** этапы по порядку; артефакты в skill folder; спрашивать до выдумок; грануляция + `SPLIT_*`; цитировать канон репо; предлагать AGENTS.md ссылки в конце.

**Do not:** создавать OpenSpec changes / tasks / код без явной просьбы вне этого skill; silent-merge противоречий; закрывать липкий стек Assumption; автодвигать пайплайн без слова `next`; прыгать вперёд через `prev`; оформлять `next`/`prev` как slash-команды с `/`.

## Decomposition principles

- Грануляция под слабые LLM важнее крупного epic.
- Перелинковка + порядок обязательны после сплита.
- Не tech-tasks («добавить схему») без user-facing/acceptance-связи.
- Не перепланировать существующее при canon без нужды.
