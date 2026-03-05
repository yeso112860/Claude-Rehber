---
description: "API katmanı kuralları - RESTful, Controller yapısı, yetkilendirme"
paths:
  - "**/*Controller.cs"
---

# API Katmanı Kuralları

## Controller Yapısı

```csharp
[ApiController]
[Route("api/bsv")]
[Authorize]
[Produces("application/json")]
public sealed class BasvuruController : ControllerBase
{
    private readonly ISender _mediator;
    private readonly ILogger<BasvuruController> _logger;

    public BasvuruController(ISender mediator, ILogger<BasvuruController> logger)
    {
        _mediator = mediator;
        _logger = logger;
    }

    [HttpGet("basvurular")]
    [Authorize(Roles = "CevreGorevlisi,IlMudurlugu,Bakanlik")]
    [ProducesResponseType(typeof(ApiResponse<PageResponse<BasvuruDto>>), 200)]
    public async Task<IActionResult> GetBasvurular(
        [FromQuery] ListBasvuruQuery query, CancellationToken cancellationToken)
    {
        var result = await _mediator.Send(query, cancellationToken);
        return result.IsSuccess ? Ok(ApiResponse<PageResponse<BasvuruDto>>.Success(result.Value))
                                : BadRequest(ApiResponse<PageResponse<BasvuruDto>>.Failure(result.Error));
    }

    [HttpPost("basvurular")]
    [Authorize(Roles = "CevreGorevlisi")]
    [ProducesResponseType(typeof(ApiResponse<BasvuruDto>), 201)]
    public async Task<IActionResult> CreateBasvuru(
        [FromBody] CreateBasvuruCommand command, CancellationToken cancellationToken)
    {
        var result = await _mediator.Send(command, cancellationToken);
        if (!result.IsSuccess) return BadRequest(ApiResponse<BasvuruDto>.Failure(result.Error));
        return CreatedAtAction(nameof(GetBasvuruById), new { id = result.Value.Id },
            ApiResponse<BasvuruDto>.Success(result.Value));
    }
}
```

## RESTful Endpoint Adlandırma

```
GET    /api/{modul}/{kaynak}               → Listeleme (çoğul)
GET    /api/{modul}/{kaynak}/{id}           → Tekil getirme
POST   /api/{modul}/{kaynak}               → Oluşturma
PUT    /api/{modul}/{kaynak}/{id}           → Tam güncelleme
PATCH  /api/{modul}/{kaynak}/{id}           → Kısmi güncelleme
DELETE /api/{modul}/{kaynak}/{id}           → Silme

# Özel aksiyonlar
POST   /api/{modul}/{kaynak}/{id}/onayla
POST   /api/{modul}/{kaynak}/{id}/reddet
POST   /api/{modul}/{kaynak}/{id}/iptal-et
PATCH  /api/{modul}/{kaynak}/{id}/durum
```

## Endpoint Örnekleri

```
GET/POST   /api/sys/parametreler
GET/POST   /api/bsv/basvurular
POST       /api/bsv/basvurular/{id}/belgeler
POST       /api/deg/degerlendirmeler/{id}/onayla
POST       /api/deg/degerlendirmeler/{id}/eksiklik-bildir
```

## ApiResponse<T>

```csharp
public sealed record ApiResponse<T>
{
    public T? Data { get; init; }
    public bool IsSuccess { get; init; }
    public string? Message { get; init; }
    public IEnumerable<string> Errors { get; init; } = [];

    public static ApiResponse<T> Success(T data, string? message = null) =>
        new() { Data = data, IsSuccess = true, Message = message };
    public static ApiResponse<T> Failure(string error) =>
        new() { IsSuccess = false, Errors = [error] };
}
```

## Yetkilendirme Rolleri

```
SistemYoneticisi    → SYS modülü yönetimi
CevreGorevlisi     → Başvuru yapma (kendi tesisleri)
CevreDanismanFirma → ÇG yönetimi
IlMudurlugu        → EK-2 değerlendirme
BakanlikPersoneli  → EK-1 değerlendirme
SubeMuduru         → Onay/red kararı
FirmaYetkilisi     → Sadece görüntüleme
```

## Temel Kurallar

- MediatR pattern zorunlu: Controller → MediatR → Handler → Repository
- Controller'da iş mantığı YASAK
- Tüm endpoint'lerde `CancellationToken` parametresi zorunlu
- `async Task<IActionResult>` dönüş tipi standart
- Her action'da XML summary yaz (Swagger için)
- `[ValidateAntiForgeryToken]` kullanılmaz (JWT tabanlı)
