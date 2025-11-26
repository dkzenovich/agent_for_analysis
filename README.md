# Analysis as Code (ГОСТ Р 57193): шаблон репозитория

Методология: требования ведутся как код, в git, с четким разделением StRS (BA) и SyRS (SA), опорой на текущую архитектуру (AS-IS), и автоматическими проверками целостности.

## Что внутри
- `input/` — сырые входные данные без правок.
- `reference/` — база знаний AS-IS: OpenAPI, схемы БД, реестр интеграций.
- `prompts/` — инструкции для агентов (BA/SA/PM/QA).
- `target/docs/src/` — результат: StRS, SyRS, логическая архитектура, трассировка.
- `target/adrs/` — PDR/ADR по продуктовым/архитектурным развилкам.
- `target/model.dsl` — общая модель Structurizr (по необходимости).
- `docker-compose.yml` — опционально для локального сервера документации.

## Рабочий процесс (кратко)
1) BA: заполняет `input/`, запускает промпт `10-gost-642-stakeholder.md`, получает StRS в `01_Stakeholder_Needs/` (`*.md`).
2) SA: использует утвержденный StRS + `reference/`, запускает `20-gost-643-impact-analysis.md`, пишет `00_impact_analysis.md`.
3) SA: по результату impact — `21-gost-643-system-spec.md`, заполняет SyRS (`01_functional.md`, `02_quality.md`, `03_interfaces.md`, `04_verification.md`).
4) PM/SA: при развилках — `30-product-decision.md`, кладет PDR/ADR в `target/adrs/`.
5) QA/Lead: `90-traceability-check.md` — отчет о целостности в `04_Traceability/`.

## Быстрый старт
- Скопируйте входные данные в `input/`.
- Обновите AS-IS (OpenAPI/схемы) в `reference/`.
- Используйте промпты по этапам (BA → SA → PM → QA).
- Поддерживайте все артефакты в git, как код.
