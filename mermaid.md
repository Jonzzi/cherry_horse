# Анализатор репозиториев кода

## Описание проекта

Это проект автоматического агента для анализа репозиториев кода. Агент использует подход ReAct (Reason + Act) для исследования структуры проекта, анализа кода и генерации документации. Он может читать файлы, искать паттерны, анализировать Python-код через AST и создавать диаграммы в формате Mermaid.

## Структура проекта

```
.
├── agent.py # Ядро агента: ReAct loop, управление инструментами
├── main.py # Точка входа
├── tools.py # Инструменты для работы с файловой системой
├── analyze_tool.py # Инструмент AST-анализа Python-файлов
├── mermaid_tool.py # Инструмент для генерации Mermaid-диаграмм
├── config.yaml # Конфигурация агента
├── prompts/ # Файлы с промптами для агента
├── requirements.txt # Зависимости проекта
└── docs/ # Документация
```

## Архитектура

### Основные классы

```mermaid
class Tool {
    +execute()
    +__init__()
}

class Agent {
    +get_system_prompt()
    +think_and_act()
    +run()
}

ReadFileTool <|-- Tool
ListDirTool <|-- Tool
GrepTool <|-- Tool
WriteFileTool <|-- Tool
AnalyzePythonTool <|-- Tool
GenerateMermaidTool <|-- Tool

Agent --> Tool : использует
Agent --> AnalyzePythonTool : использует
Agent --> GenerateMermaidTool : использует

Tool <|-- ReadFileTool
Tool <|-- ListDirTool
Tool <|-- GrepTool
Tool <|-- WriteFileTool
Tool <|-- AnalyzePythonTool
Tool <|-- GenerateMermaidTool
```

### Описание компонентов

**Tool** — базовый класс для всех инструментов. Содержит метод `execute()` для выполнения действий.

**Agent** — основной класс агента. Содержит методы `get_system_prompt()` для получения системного промпта и `think_and_act()` для цикла размышления и действия.

**ReadFileTool** — инструмент для чтения содержимого файлов.

**ListDirTool** — инструмент для перечисления содержимого директорий.

**GrepTool** — инструмент для поиска паттернов в Python-файлах.

**WriteFileTool** — инструмент для записи текста в файлы.

**AnalyzePythonTool** — инструмент для анализа структуры Python-файлов через AST.

**GenerateMermaidTool** — инструмент для генерации Mermaid-диаграмм.

## Зависимости

- `httpx` — асинхронный HTTP-клиент
- `yaml` — парсинг YAML-конфигурации
- `jinja2` — шаблонизация промптов
- `rich` — красивая консольная вывод
- `ast` — стандартная библиотека для анализа Python-кода

## Быстрый старт

### Установка

```bash
pip install -r requirements.txt
```

### Конфигурация

Файл `config.yaml` содержит настройки агента:

```yaml
# Путь к промптам
prompts_dir: "./prompts"

# Включить Mermaid-диаграммы в вывод
include_mermaid: true
```

### Запуск

```bash
python main.py
```

Агент начнёт анализировать репозиторий, используя инструменты для исследования структуры, чтения файлов и генерации документации.
