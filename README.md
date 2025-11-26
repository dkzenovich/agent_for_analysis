# Analysis as Code

Шаблон репозитория для ведения требований как кода: роли и промпты зафиксированы, артефакты версионируются в git, StRS и SyRS разделены, все опирается на текущую архитектуру (AS-IS) и проверяется трассировкой.

## Идея и ценность
- Единый конвейер «сырые входы → промпты → StRS/SyRS → архитектура → трассировка».
- Четкое разграничение ролей (BA/SA/PM/QA) и артефактов ГОСТ Р 57193.
- Репродуцируемость: каждый запуск крупного промпта оставляет PDR-лог, артефакты лежат в заранее описанной структуре.

## Как выглядит рабочий процесс
1. **Подготовка AS-IS**: обновить `reference/` (OpenAPI, схемы БД, интеграции) и сложить сырые материалы в `input/`.
2. **StRS (BA)**: промпт `prompts/10-gost-642-stakeholder.md` → папка `target/docs/src/01_Stakeholder_Needs/` (`stakeholders`, `ops_concepts`, `constraints`, `requirements`).
3. **Impact (SA)**: промпт `prompts/20-gost-643-impact-analysis.md` с StRS + AS-IS → `target/docs/src/02_System_Requirements/00_impact_analysis.md` (вердикт по совместимости/версионированию).
4. **SyRS (SA)**: промпт `prompts/21-gost-643-system-spec.md` → `01_functional.md`, `02_quality.md`, `03_interfaces.md`, `04_verification.md` в той же папке.
5. **PDR/ADR (PM/SA)**: решения и развилки через `prompts/30-product-decision.md` → `target/adrs/` + reasoning-лог в `target/pdrs/`.
6. **Трассировка (QA/Lead)**: `prompts/90-traceability-check.md` → отчет в `target/docs/src/04_Traceability/` (покрытие StRS↔SyRS↔AC, сиротские требования).
7. **Визуализации (SA)**: по необходимости править `target/docs/src/03_Logical_Architecture/` (`c4_context.dsl`, `sequences.mermaid`) и `target/model.dsl`; итоговые диаграммы собираются из DSL/mermaid.

## Структура репозитория
- `input/` — сырые материалы без правок.
- `reference/` — база знаний AS-IS (`openapi/`, `db_schema/`, `integrations.md`).
- `prompts/` — пошаговые инструкции для ролей (10 StRS → 90 Traceability).
- `target/docs/src/` — результат: StRS, SyRS, логическая архитектура, трассировка.
- `target/adrs/` — продуктовые/архитектурные решения (ADR/PDR).
- `target/pdrs/` — reasoning-логи запусков ключевых промптов (формат `YYYYMMDD-HHMM-<prompt>.md`).
- `target/model.dsl` — общая модель Structurizr (для генерации диаграмм).
- `architecture.png` — обзорный рисунок процесса Analysis as Code.
- `docker-compose.yml` — опционально, локальный сервер документации/визуализаций.

## Быстрый старт
1. Скопируйте исходные материалы в `input/` и актуализируйте AS-IS в `reference/`.
2. Следуйте промптам по ролям: BA → SA → PM/SA → QA (см. шаги выше).
3. После каждого существенного прогона сохраняйте PDR в `target/pdrs/` и фиксируйте изменения в git.
4. Для ревью пролистайте `target/docs/src/04_Traceability/` — там видно покрытие и сиротские требования.

## Принципы
- Строгое разделение StRS (что нужно бизнесу) и SyRS (как изменяется система).
- Brownfield-подход: отталкиваемся от текущих контрактов, явно фиксируем breaking changes и версии.
- Все артефакты — код: версионирование, ревью, воспроизводимость через промпты и PDR.
