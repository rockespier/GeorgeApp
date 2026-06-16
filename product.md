#Visión del Producto: Sistema GEORGE

#1. Propósito
El sistema permite a la compañía visualizar y gestionar los vencimientos legales y operativos (Obligaciones, Permisos, etc.) asignados a los usuarios.

#2. Stack Tecnológico
Backend: .NET 9 (C# 13).
Arquitectura: Clean Architecture simplificada / Minimal APIs.
Base de Datos: Entity Framework Core 9 (Enfoque: Database First, base de datos preexistente).
Seguridad: Single Sign-On (SSO) usando Microsoft Entra ID (OAuth2/JWT).
Frontend: Angular 

#3. Especificación del Endpoint Principal (MVP)
Ruta: GET /api/vencimientos (Protegido con [Authorize]).
	Parámetro de entrada: UsuarioId (int).
	
	Contrato de Salida (DTO esperado):
		TipoVencimiento (string: Obligación, Permiso, Compromiso.)
		FechaVencimiento (DateOnly o DateTimeOffset)
		Gestion (string: Ambiental, Minera, Seguridad,etc.)
		VencimientoId (int)
		Descripcion (string)
		Unidad (string)
		Estado (string: Vencida, En Proceso,Cumplido, etc)
		Responsables Formal (string: Nombres y apellidos)
		Responsables Operativos (string: Nombres y apellidos)
		
#4. Reglas de Código Obligatorias
Usar records para los DTOs.
Usar Constructores Primarios (Primary Constructors) en C# 12/13.
NUNCA exponer las Entidades de Entity Framework directamente en la API; mapear siempre al DTO.

## 5. Mantén `product.md` separado del `README.md`

Buena práctica:

- `product.md`: visión funcional del producto.
- `README.md`: instrucciones técnicas para instalar, compilar y ejecutar.
- `docs/architecture.md`: decisiones técnicas.
- `docs/database.md`: comandos EF Core, tablas, convenciones.
- `.codex/AGENTS.md`: reglas para Codex.

---

## 6. Agrega documentación para decisiones técnicas

Puedes crear esta carpeta:

```text
docs/decisions/

Y archivos tipo:

ADR-0001-project-structure.md
ADR-0002-database-first.md
ADR-0003-authentication-entra-id.md

Ejemplo:
# ADR-0001 - Estructura del proyecto

## Estado

Aceptado

## Contexto

El proyecto usará Clean Architecture con separación entre API, Application, Domain e Infrastructure.

## Decisión

La solución se dividirá en cuatro proyectos principales:

- Api
- Application
- Domain
- Infrastructure

## Consecuencias

Mejora la separación de responsabilidades, facilita testing y permite evolución ordenada.