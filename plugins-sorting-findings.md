# Plugins Page — Sorting & Filtering (2026-07-03)

## Файлы

- `web/src/pages/PluginsPage.tsx` — сортировка + фильтрация
- `web/src/plugins/slots.ts` — система слотов
- `web/src/lib/api.ts:2284-2298` — интерфейс `HubAgentPluginRow`
- `hermes_cli/web_server.py:13610` — API endpoint `/api/hub`

## Ключевые выводы

- PluginSlot `name="plugins:bottom"` был удалён (источник дублирования)
- Контролы фильтрации OUTSIDE `<Card>` — в самом `<div>`
- i18n отсутствует — строки хардкодом (`source`, `status`, `name`)
- Build проходит за ~370ms (481 модулей)
- Dashboard на порту 9119

## Структура сортировки

```
sortBy: "name" | "source" | "runtime_status"
sortDir: "asc" | "desc"
```

Сортировка по `localeCompare()` на строковом представлении.

## Фильтрация

```
sources: ["all", "bundled", "user", "git"]
statuses: ["all", "enabled", "disabled", "inactive"]
```

## Бэкап

```bash
git tag plugins-sortable-backup
```

Путь: `C:\Users\Admin\AppData\Local\hermes\hermes-agent`
