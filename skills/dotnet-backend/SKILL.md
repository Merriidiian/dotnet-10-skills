---
name: dotnet-backend
description: Применяй при разработке и ревью C#-бэкендов на .NET 10, включая gRPC-микросервисы, GraphQL-шлюзы и общие контракты.
---

# Разработка бэкендов на .NET 10

Перед изменением или ревью прочитай [общие соглашения](rules/dotnet/00-core.mdc) и [признаки стека](rules/dotnet/00-stack.mdc). В монорепозитории определяй стек отдельно для каждого затронутого сервиса.

Для изменений в Domain и Application используй [шаблон операции переименования](references/domain-and-application.md). В нём показаны объект-значение, изменяемая сущность и команда, которая загружает документ, вызывает доменный метод, сохраняет результат и возвращает ответ. В шаблонах доменные базовые классы, ошибки и контракты обозначают роли типов из целевого проекта. Для регистрации и вызовов используй API установленных пакетов.

Выбирай тематические файлы `.mdc` по полю `description` и сопоставляй их `globs` с путями в целевом проекте. Путь [rules/dotnet](rules/dotnet) отсчитывается от каталога этого навыка.

- Для C# ориентируйся на [правила читаемости](rules/dotnet/csharp/simplicity.mdc) и [обработки ошибок](rules/dotnet/errors).
- В Domain проверь [трёхстрочный XML-комментарий](rules/dotnet/domain/summaries.mdc), [базовый тип каждого класса](rules/dotnet/domain/entity-or-vo.mdc) и [пример перечисления с явными значениями](rules/dotnet/domain/enums.mdc).
- В Application сверяйся с [формой команды или запроса](rules/dotnet/microservice/application-mediatr.mdc), [порядком действий в обработчике](rules/dotnet/microservice/handler-returns.mdc) и [границей маппинга](rules/dotnet/microservice/mapper.mdc). Для внешней точки входа выбери [gRPC-адаптер](rules/dotnet/microservice/api-adapter.mdc) или [GraphQL-шлюз](rules/dotnet/gateway) по стеку проекта.
- Для Protobuf и событий открой [контракты](rules/dotnet/contracts), для запуска и конфигурации сервиса — [bootstrap](rules/dotnet/bootstrap).
- При изменении `.gitignore` сверяйся с [шаблоном](rules/dotnet/00-gitignore.mdc). При переносе замечаний по коду в новые правила используй [соглашения для авторов](rules/dotnet/authoring.mdc).
