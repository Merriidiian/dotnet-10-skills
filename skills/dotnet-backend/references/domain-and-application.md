# Шаблон изменения доменной сущности

У документа есть идентичность. При переименовании она сохраняется. Заголовок определяется своим текстом. Так выбираются базовые типы двух классов.

Для адаптации замени `Document` на свой агрегат, `DocumentTitle` — на проверяемое значение, `RenameDocument` — на бизнес-действие. Базовые типы домена, ошибки, контекст EF, контракты и маппер предоставляет целевой проект. Вход в этом шаблоне — идентификатор и новый заголовок. При недопустимом значении старый заголовок сохраняется. При успешной записи клиент получает обновлённый документ.

## Объект-значение

Конструктор принимает строку, проверяет её и сохраняет нормализованное значение. Это же значение определяет равенство заголовков.

```csharp
/// <summary>
/// Заголовок документа.
/// </summary>
public class DocumentTitle : ValueObject
{
    /// <summary>
    /// Создаёт заголовок из непустого текста.
    /// </summary>
    public DocumentTitle(string text)
    {
        if (string.IsNullOrWhiteSpace(text))
            throw new InvalidInputError("Укажите заголовок документа.");

        Text = text.Trim();
    }

    /// <summary>
    /// Текст заголовка без крайних пробелов.
    /// </summary>
    public string Text { get; private set; }

    /// <summary>
    /// Возвращает компоненты равенства заголовка.
    /// </summary>
    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return Text;
    }
}
```

## Изменяемая сущность

Присвоение происходит после проверки нового заголовка. Если конструктор объекта-значения отклонит пустой текст, документ сохранит прежний заголовок.

```csharp
/// <summary>
/// Документ с редактируемым заголовком.
/// </summary>
public class Document : MutableEntity
{
    /// <summary>
    /// Создаёт документ с заданным заголовком.
    /// </summary>
    public Document(string title) => Rename(title);

    private Document() { }

    /// <summary>
    /// Текущий заголовок документа.
    /// </summary>
    public DocumentTitle Title { get; private set; } = null!;

    /// <summary>
    /// Присваивает документу новый заголовок.
    /// </summary>
    public void Rename(string title) => Title = new DocumentTitle(title);
}
```

## Команда Application

В файле `RenameDocument.cs` находятся запрос и обработчик. Входной контракт передаёт строковый идентификатор и новый заголовок. Контракт ответа формирует маппер после успешной записи.

```csharp
public record RenameDocument(Contracts.RenameDocumentRequest Request)
    : IRequest<Contracts.RenameDocumentResponse>;

public class RenameDocumentHandler(DocumentDbContext context, IMapper mapper)
    : IRequestHandler<RenameDocument, Contracts.RenameDocumentResponse>
{
    public async ValueTask<Contracts.RenameDocumentResponse> Handle(
        RenameDocument command, CancellationToken cancellationToken)
    {
        var request = command.Request;
        if (!Guid.TryParse(request.Id, out var id) || id == Guid.Empty)
            throw new InvalidInputError("Укажите идентификатор документа.");

        var document = await context.Documents
            .SingleOrDefaultAsync(item => item.Id == id, cancellationToken)
            ?? throw new NotFoundError<Document>(id);

        document.Rename(request.Title);
        await context.SaveChangesAsync(cancellationToken);

        return new Contracts.RenameDocumentResponse { Document = mapper.Map(document) };
    }
}
```
