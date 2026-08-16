# Mitre Mantenimiento

Sistema de gestión de mantenimiento del parque de bienes de **Grupo Mitre**
(demoliciones, movimiento de suelos y alquiler de maquinaria vial).

## El problema

Hoy el mantenimiento del parque (~150+ bienes entre maquinaria pesada, camiones,
camionetas, robots Brokk y grupos electrógenos) se maneja disperso entre un
software, planillas, WhatsApp y papel. No hay una vista única de:

- qué service le toca a cada equipo y cuándo (por horas, km o fecha),
- qué documentación está por vencer (VTV, seguros, matafuegos, RUTA),
- qué se le hizo a cada bien, quién lo hizo y cuánto costó,
- qué órdenes de trabajo tiene pendientes el taller.

## La solución

Una bitácora digital por bien + mantenimiento preventivo + órdenes de trabajo,
con tres puntos de vista:

| Usuario | Pantalla | Qué hace |
|---|---|---|
| Gerencia / administración | **Tablero** | Alertas del día, vencimientos, gasto, estado del parque |
| Taller propio | **Taller / OTs** | Cola de órdenes de trabajo; cada cierre queda en la bitácora |
| Operador / capataz en obra | **Parte de obra** | Desde el celular: carga lecturas de horómetro/km o reporta fallas |

### Conceptos clave del modelo

- **Bien**: cada máquina o vehículo, con número de interno (`EX-012`), categoría,
  obra/ubicación y estado (operativo, en taller, alquilado).
- **Medidor**: horómetro (hs) para maquinaria, odómetro (km) para vehículos.
  Las lecturas llegan desde el parte de obra.
- **Plan preventivo**: service cada X hs/km + vencimientos por calendario.
  El sistema calcula cuánto falta y dispara las alertas.
- **Bitácora**: historial cronológico e inmutable por bien — services,
  reparaciones, fallas, inspecciones y lecturas, con responsable y costo.
- **Orden de trabajo (OT)**: una falla reportada desde obra crea una OT
  automáticamente; el taller la trabaja y su cierre se registra en la bitácora.

## Demo

`index.html` es una demo interactiva autocontenida (sin backend, datos de
ejemplo): abrila en cualquier navegador. Simula un parque de 156 bienes con
casos reales del rubro: excavadora con service de horas vencido, camión con la
VTV por vencer, cargadora en taller con OT en curso, minicargadora alquilada.

Flujo para probar: **Parte de obra → reportar falla → aparece la OT en
Taller → queda la entrada en la bitácora de la ficha.**

## Roadmap propuesto

1. **Validación** (esta demo): ajustar categorías, obras, planes de service y
   circuito de OTs con el taller y gerencia.
2. **MVP real**: backend (Supabase), usuarios y roles, alta del parque con
   importación desde las planillas actuales, fotos y adjuntos (facturas,
   pólizas) en cada entrada de bitácora.
3. **Operación**: notificaciones de vencimientos (email/WhatsApp), reportes de
   costos por bien/obra/mes, código QR en cada máquina que abre su ficha.
4. **Integraciones**: proveedores externos que firman sus trabajos en la
   bitácora, y cruce con los sistemas existentes de Mitre (certificación de
   obra, tableros Qlik).
