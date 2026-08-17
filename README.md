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


## Wiki técnica e investigación

La pestaña **Wiki técnica** de la app resume una investigación sobre ~170 sitios
especializados (manuales OEM, dealers, foros de operadores, publicaciones del
sector y normativa argentina) aplicada a los equipos reales del parque:

- **Maquinaria**: Doosan DX225LCA, Sany SY135C/SY215C, Komatsu PC200-8,
  Bobcat E35/E50, Wacker Neuson EZ53, Volvo L110F — planes por horas
  (250/500/1000/2000), fallas típicas por modelo y reglas de demolición
  (con martillo ≥50% del tiempo, el hidráulico se cambia a mitad de intervalo).
- **Implementos**: martillos (engrase cada 2 hs con pasta de cincel, N2 semanal,
  resellado 600–1.000 hs), mordazas/cizallas, hidrogrúa Palfinger (IRAM 3927,
  Dec. 911/96), grupos y compresores.
- **Camiones**: Mercedes-Benz Axor/Atego/Accelo (planes oficiales de obra en
  horas: 350/300 hs), Scania P/G, VW 13.180, Ford F-14000, furgones; prácticas
  de volcadores en demolición y régimen regulatorio (RTO/VTV/RUTA).
- **Flota liviana**: Hilux, Amarok (correa a 60–70.000 km en flota, no 90.000),
  Cronos, Chevrolet, utilitarios chinos; VTV PBA/CABA.

## Módulos de gestión

- **Predictivo**: proyección de fecha de próximo service por equipo (lectura del
  medidor + uso mensual estimado por categoría) y técnicas recomendadas
  (análisis de aceite, consumo l/h, termografía, inspección estructural).
- **Taller**: cola de OTs; al cerrar se registra persona, horas y repuestos →
  producción por mecánico/herrero e indicadores (MTTR, % preventivo,
  reincidencia por equipo) con benchmarks de la industria.
- **Checklists**: recepción en taller, inspección en obra y carga de
  combustible en obra (con registro de horómetro y litros). Los ítems en falla
  generan OTs automáticamente.
