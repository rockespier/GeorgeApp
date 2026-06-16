# AGENTS.md

## Objetivo del proyecto
Este proyecto usa Codex con skills especializadas para construir APIs backend en .NET usando Clean Architecture, Minimal APIs, Entity Framework Core con enfoque Database First y Microsoft Entra ID.

Codex debe apoyarse en las skills ubicadas en `.codex/skills/` según el tipo de tarea solicitada.

## Skills disponibles

### $arquitecto-dotnet
Usar cuando la tarea involucre arquitectura, estructura de solución, Clean Architecture, Minimal APIs, Microsoft Entra ID, inyección de dependencias, configuración de `Program.cs`, middleware, autenticación, autorización o separación de capas.

### $dba-ef-core
Usar cuando la tarea involucre SQL Server, Entity Framework Core, Database First, scaffolding, DbContext, entidades generadas desde base de datos, optimización de consultas LINQ, índices, performance, migración de consultas SQL a LINQ o revisión de acceso a datos.

### $desarrollador-backend
Usar cuando la tarea involucre implementación de lógica de negocio, servicios de aplicación, endpoints, DTOs, validaciones, handlers, mapeo entre entidades y DTOs, resultados HTTP, errores controlados o código C# 13.

## Reglas generales de trabajo

- Mantener una arquitectura limpia y separada por responsabilidades.
- No mezclar lógica de negocio dentro de `Program.cs`.
- No exponer directamente entidades de EF Core en las respuestas HTTP; usar DTOs.
- Preferir endpoints pequeños, claros y testeables.
- Evitar consultas LINQ que generen carga innecesaria en memoria.
- Usar async/await para operaciones de I/O.
- No colocar cadenas de conexión, secretos o datos sensibles en el código fuente.
- Documentar decisiones técnicas importantes cuando afecten arquitectura, seguridad o rendimiento.

## Orden recomendado para nuevas funcionalidades

1. Definir o revisar estructura con `$arquitecto-dotnet`.
2. Revisar acceso a datos y consultas con `$dba-ef-core`.
3. Implementar endpoint y lógica con `$desarrollador-backend`.
