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

