---
name: rutdigital-dotnet
description: Применяй соглашения RutDigital при разработке и ревью C#-бэкендов на .NET 10, включая gRPC-микросервисы, GraphQL-шлюзы и общие контракты.
---

# Правила .NET для RutDigital

Перед изменением или ревью прочитай [общие соглашения](rules/dotnet/00-core.mdc) и [признаки стека](rules/dotnet/00-stack.mdc). В монорепозитории определяй стек отдельно для каждого затронутого сервиса.

Выбирай тематические файлы `.mdc` по полю `description` и сопоставляй их `globs` с путями в целевом проекте. Путь [rules/dotnet](rules/dotnet) отсчитывается от каталога этого навыка.

- Для изменений в C# проверь правила [языка и стиля](rules/dotnet/csharp), [домена и EF Core](rules/dotnet/domain) и [обработки ошибок](rules/dotnet/errors).
- Для точки входа и слоя приложения выбери [gRPC-микросервис](rules/dotnet/microservice) или [GraphQL-шлюз](rules/dotnet/gateway) по стеку проекта.
- Для Protobuf и событий открой [контракты](rules/dotnet/contracts), для запуска и конфигурации сервиса — [bootstrap](rules/dotnet/bootstrap).
- При изменении `.gitignore` сверяйся с [шаблоном](rules/dotnet/00-gitignore.mdc). При переносе замечаний по коду в новые правила используй [соглашения для авторов](rules/dotnet/authoring.mdc).
