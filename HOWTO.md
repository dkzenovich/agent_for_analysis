# Как работать с репозиторием (Analysis as Code)

## Роли и этапы
1) BA → StRS: взять сырые входные (`input/`), промпт `prompts/10-gost-642-stakeholder.md`, результат в `target/docs/src/01_Stakeholder_Needs/` (Stakeholders, OpsCon, Constraints, REQ-BIZ).
2) SA → Impact: использовать StRS + `reference/` (OpenAPI, схемы БД, интеграции), промпт `prompts/20-gost-643-impact-analysis.md`, результат `00_impact_analysis.md`.
3) SA → SyRS: промпт `prompts/21-gost-643-system-spec.md`, заполнить `01_functional`, `02_quality`, `03_interfaces`, `04_verification` (все `.md`).
4) PM/SA → PDR/ADR: при развилках использовать `prompts/30-product-decision.md`, класть файл в `target/adrs/`.
5) QA/Lead → Трассировка: `prompts/90-traceability-check.md`, отчет в `target/docs/src/04_Traceability/`.

## Структура артефактов
- `input/` — сырые данные без правок.
- `reference/` — AS-IS: `openapi/`, `db_schema/`, `integrations.md`.
- `prompts/` — инструкции по ролям и процессам.
- `target/docs/src/` — итоговые документы StRS/SyRS/архитектура/трассировка (формат `.md`).
- `target/adrs/` — решения и развилки (PDR/ADR).
- `target/model.dsl` — модель Structurizr (опционально для диаграмм).

## Принципы
- Разделение BA (потребности) и SA (дельта системы).
- Brownfield: работаем от AS-IS, не ломаем существующие контракты без версии.
- Всё в git: версии, ревью, трассировка.
- Никаких технических решений в StRS: только бизнес-ценность.
- Прозрачность рисков: impact-анализ фиксирует breaking changes.

## Мини-Checklist перед ревью
- StRS: требования без технологий, есть ограничения и сценарии.
- Impact: явный вердикт по совместимости/версионированию.
- SyRS: REQ-SYS связаны с REQ-BIZ, НФТ и интерфейсы задокументированы.
- PDR/ADR: решения и последствия зафиксированы.
- Трассировка: нет сиротских требований, критерии приемки покрывают SyRS.
