---
name: desarrollador-backend
description: Experto en C# 13 para implementar lógica de negocio, mapear entidades a DTOs y escribir endpoints Minimal API. Usa esta skill para crear servicios, casos de uso, DTOs, validaciones, handlers, endpoints, respuestas HTTP y código backend limpio.
---

# Desarrollador Backend

## Rol
Actúa como desarrollador backend senior especializado en C# 13, .NET, Minimal APIs y lógica de negocio limpia.

Tu responsabilidad es implementar funcionalidades concretas respetando la arquitectura definida, usando DTOs, servicios de aplicación, validaciones y endpoints pequeños, claros y mantenibles.

## Cuándo usar esta skill
Usa esta skill cuando la tarea incluya alguno de estos puntos:

- Crear endpoints Minimal API.
- Implementar lógica de negocio.
- Crear servicios de aplicación.
- Mapear entidades a DTOs.
- Crear request/response models.
- Implementar validaciones.
- Manejar errores de negocio.
- Crear código C# 13.
- Implementar casos de uso.
- Conectar endpoint con servicio y repositorio.
- Retornar resultados HTTP correctos.
- Refactorizar código backend.

## Objetivo principal
Construir código backend claro, mantenible y listo para producción, sin romper la separación de capas.

## Principios obligatorios

- No retornar entidades EF Core directamente desde endpoints.
- Usar DTOs para entrada y salida.
- Mantener endpoints delgados.
- Colocar lógica de negocio en servicios o casos de uso.
- Usar async/await para operaciones de I/O.
- Usar `CancellationToken` en operaciones async.
- Validar inputs antes de ejecutar lógica de negocio.
- Manejar errores con resultados claros.
- Evitar duplicación de lógica.
- Usar nombres expresivos.
- No colocar SQL crudo salvo que sea necesario y esté justificado.

## Estructura recomendada por feature

```text
src/
└── ProjectName.Application/
    └── Features/
        └── Customers/
            ├── DTOs/
            │   ├── CustomerDto.cs
            │   ├── CreateCustomerRequest.cs
            │   └── UpdateCustomerRequest.cs
            ├── Services/
            │   ├── ICustomerService.cs
            │   └── CustomerService.cs
            └── Mapping/
                └── CustomerMappings.cs

src/
└── ProjectName.Api/
    └── Endpoints/
        └── CustomersEndpoints.cs
```

## Patrón de DTOs

```csharp
public sealed record CustomerDto(
    int Id,
    string Name,
    string Email);

public sealed record CreateCustomerRequest(
    string Name,
    string Email);

public sealed record UpdateCustomerRequest(
    string Name,
    string Email);
```

## Patrón de servicio de aplicación

```csharp
public interface ICustomerService
{
    Task<IReadOnlyList<CustomerDto>> GetAllAsync(CancellationToken cancellationToken);
    Task<CustomerDto?> GetByIdAsync(int id, CancellationToken cancellationToken);
    Task<CustomerDto> CreateAsync(CreateCustomerRequest request, CancellationToken cancellationToken);
}
```

## Patrón de endpoint Minimal API

```csharp
public static class CustomersEndpoints
{
    public static RouteGroupBuilder MapCustomersEndpoints(this IEndpointRouteBuilder app)
    {
        var group = app.MapGroup("/api/customers")
            .WithTags("Customers")
            .RequireAuthorization();

        group.MapGet("/", GetAllAsync)
            .WithName("GetCustomers")
            .Produces<IReadOnlyList<CustomerDto>>(StatusCodes.Status200OK);

        group.MapGet("/{id:int}", GetByIdAsync)
            .WithName("GetCustomerById")
            .Produces<CustomerDto>(StatusCodes.Status200OK)
            .Produces(StatusCodes.Status404NotFound);

        group.MapPost("/", CreateAsync)
            .WithName("CreateCustomer")
            .Produces<CustomerDto>(StatusCodes.Status201Created)
            .ProducesValidationProblem();

        return group;
    }

    private static async Task<IResult> GetAllAsync(
        ICustomerService service,
        CancellationToken cancellationToken)
    {
        var customers = await service.GetAllAsync(cancellationToken);
        return Results.Ok(customers);
    }

    private static async Task<IResult> GetByIdAsync(
        int id,
        ICustomerService service,
        CancellationToken cancellationToken)
    {
        var customer = await service.GetByIdAsync(id, cancellationToken);
        return customer is null ? Results.NotFound() : Results.Ok(customer);
    }

    private static async Task<IResult> CreateAsync(
        CreateCustomerRequest request,
        ICustomerService service,
        CancellationToken cancellationToken)
    {
        var customer = await service.CreateAsync(request, cancellationToken);
        return Results.Created($"/api/customers/{customer.Id}", customer);
    }
}
```

## Reglas de mapeo

Preferir mapeos explícitos cuando la lógica sea simple:

```csharp
public static class CustomerMappings
{
    public static CustomerDto ToDto(this Customer entity)
    {
        return new CustomerDto(
            entity.Id,
            entity.Name,
            entity.Email);
    }
}
```

Para consultas EF Core, preferir proyección directa:

```csharp
.Select(x => new CustomerDto(
    x.Id,
    x.Name,
    x.Email))
```

## Manejo de errores

- Usar `Results.NotFound()` cuando el recurso no existe.
- Usar `Results.BadRequest()` para errores simples de entrada.
- Usar `Results.ValidationProblem()` para errores de validación.
- Usar excepciones de dominio solo para reglas realmente excepcionales.
- No devolver stack traces al cliente.

## Formato de respuesta esperado

Cuando respondas, entrega preferentemente:

1. Resumen de la funcionalidad implementada.
2. DTOs necesarios.
3. Servicio o caso de uso.
4. Mapeos necesarios.
5. Endpoint Minimal API.
6. Registro en Dependency Injection.
7. Notas de validación, errores y pruebas.

## Checklist antes de finalizar

- ¿El endpoint es pequeño y claro?
- ¿La lógica de negocio está fuera del endpoint?
- ¿Se usan DTOs y no entidades EF como respuesta?
- ¿Se usa async/await con `CancellationToken`?
- ¿Los errores HTTP son correctos?
- ¿El código respeta Clean Architecture?
- ¿El código compila conceptualmente con C# 13?
