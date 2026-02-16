# Sistema de Gestión de Herramientas Comunitarias

Sistema completo para la gestión de herramientas compartidas en una comunidad, con control de préstamos, usuarios y reportes.

## 📋 Descripción

Este sistema permite administrar el inventario de herramientas comunitarias, gestionar usuarios (residentes y administradores) y controlar préstamos con un sistema de solicitudes que requiere aprobación administrativa.

## ✨ Características Principales

### Gestión de Herramientas
- Registro de herramientas con código único, nombre, categoría, cantidad, estado y valor
- **Validaciones:** Código sin espacios (mín. 3 caracteres), nombre (mín. 3 caracteres), categoría solo letras, cantidad no negativa, valor no negativo
- Operaciones CRUD completas (crear, listar, buscar, actualizar, inactivar)
- Control de stock y alertas de bajo inventario
- Estados: activa, en reparación, fuera de servicio

### Gestión de Usuarios
- Registro de usuarios con identificación única
- **Validaciones completas:**
  - **Identificación:** Solo números, entre 6-15 dígitos, no duplicada
  - **Nombre y Apellido:** Solo letras (permite espacios), mínimo 2 caracteres
  - **Teléfono:** Exactamente 10 dígitos numéricos
  - **Dirección:** Mínimo 5 caracteres
  - **Tipo:** Solo "residente" o "administrador"
- Dos tipos de usuario: **residente** y **administrador**
- Operaciones CRUD completas
- Sistema de credenciales para acceso al sistema

### Gestión de Préstamos
- **Para residentes**: Sistema de solicitudes que requiere aprobación
- **Para administradores**: Creación directa de préstamos
- **Validaciones de fechas:**
  - Formato obligatorio: YYYY-MM-DD
  - Validación de fechas reales (no acepta 2026-02-30)
  - Manejo correcto de años bisiestos
  - Fecha de devolución debe ser posterior a fecha de inicio
  - Rango de años: 2020-2100
- Verificación automática de disponibilidad
- Control de stock en tiempo real
- Registro de fechas de inicio y devolución estimada
- Sistema de devolución que restaura el inventario

### Reportes y Consultas
- Herramientas con stock bajo (< 3 unidades)
- Préstamos activos y vencidos
- Historial de préstamos por usuario
- Herramientas más solicitadas
- Usuarios más activos

### Sistema de Logs
- Registro automático de todos los eventos relevantes
- Tipos de eventos: INFO, ERROR, WARNING, PRESTAMO, DEVOLUCION
- Consultas por tipo de evento o usuario
- Almacenamiento en formato texto y JSON

### Sistema de Permisos
- Login obligatorio para acceder al sistema
- Permisos diferenciados según tipo de usuario
- Credenciales por defecto: usuario `admin`, contraseña `admin123`
- Cambio de contraseña desde el sistema

## 📁 Estructura del Proyecto

```
proyecto/
│
├── menu_principal.py              # Punto de entrada del sistema
├── gestor_herramientas.py         # Gestión de herramientas
├── gestor_usuarios.py             # Gestión de usuarios
├── gestor_prestamos.py            # Gestión de préstamos y solicitudes
├── gestor_reportes.py             # Reportes y consultas
├── gestor_logs.py                 # Sistema de registro de eventos
├── gestor_permisos.py             # Sistema de login y permisos
│
├── herramientas.json              # Datos de herramientas (generado)
├── usuarios.json                  # Datos de usuarios (generado)
├── prestamos.json                 # Datos de préstamos (generado)
├── solicitudes.json               # Solicitudes pendientes (generado)
├── credenciales.json              # Credenciales de acceso (generado)
├── logs.json                      # Registro de eventos (generado)
│
├── README.md                      # Este archivo
└── pruebas/                       # Casos de prueba
    ├── caso_1_registro_herramientas.txt
    ├── caso_2_registro_usuarios.txt
    ├── caso_3_prestamos.txt
    └── caso_4_reportes.txt
```

## 🚀 Instalación y Ejecución

### Requisitos
- Python 3.7 o superior
- Solo usa la librería estándar `json` (sin librerías externas)

### Ejecución

1. Asegúrate de tener todos los archivos del proyecto en el mismo directorio

2. Ejecuta el programa principal:
```bash
python menu_principal.py
```

3. En el primer inicio, usa las credenciales por defecto:
   - **Usuario**: `admin`
   - **Contraseña**: `admin123`

## 👥 Tipos de Usuario

### Administrador
**Permisos:**
- Gestión completa de herramientas (registrar, actualizar, inactivar)
- Gestión completa de usuarios (registrar, actualizar, eliminar)
- Crear préstamos directos
- Aprobar/rechazar solicitudes de préstamos
- Ver todos los reportes
- Consultar logs del sistema
- Crear credenciales para nuevos usuarios

**Flujo típico:**
1. Login como administrador
2. Registrar usuarios residentes
3. Crear credenciales para los residentes
4. Registrar herramientas en el inventario
5. Revisar solicitudes pendientes
6. Aprobar/rechazar solicitudes
7. Consultar reportes y estadísticas

### Residente
**Permisos:**
- Ver herramientas disponibles
- Buscar herramientas específicas
- Crear solicitudes de préstamo
- Devolver herramientas
- Ver sus propios préstamos

**Flujo típico:**
1. Login como residente
2. Buscar herramientas disponibles
3. Solicitar préstamo de herramienta
4. Esperar aprobación del administrador
5. Usar la herramienta
6. Devolver la herramienta

## 📊 Ejemplos de Uso

### Ejemplo 1: Registrar una herramienta (Administrador)
```
1. Login como admin
2. Seleccionar "Gestión de herramientas"
3. Seleccionar "Registrar herramienta"
4. Ingresar datos:
   - Código: TALADRO001 (se convierte automáticamente a mayúsculas)
   - Nombre: Taladro Eléctrico Dewalt
   - Categoría: construccion
   - Cantidad: 2
   - Estado: activa
   - Valor: 350000

Validaciones aplicadas:
✓ Código sin espacios y mínimo 3 caracteres
✓ Nombre mínimo 3 caracteres
✓ Categoría solo letras
✓ Cantidad mayor o igual a 0
✓ Estado válido (activa, en reparación, fuera de servicio)
✓ Valor no negativo
```

### Ejemplo 2: Solicitar préstamo (Residente)
```
1. Login como residente
2. Seleccionar "Solicitar préstamo"
3. Ingresar:
   - ID herramienta: TALADRO001
   - Cantidad: 1
   - Fecha inicio: 2026-02-17 (formato YYYY-MM-DD obligatorio)
   - Fecha devolución: 2026-02-20 (debe ser posterior a fecha inicio)
   - Observaciones: Instalación repisas
4. Esperar aprobación del administrador

Validaciones aplicadas:
✓ Herramienta existe y está activa
✓ Cantidad disponible en inventario
✓ Formato de fecha correcto (YYYY-MM-DD)
✓ Fechas válidas (no acepta 2026-02-30)
✓ Fecha devolución posterior a fecha inicio
✓ Años bisiestos calculados correctamente
```

### Ejemplo 3: Aprobar solicitud (Administrador)
```
1. Login como admin
2. Seleccionar "Gestión de solicitudes"
3. Seleccionar "Ver solicitudes pendientes"
4. Seleccionar "Aprobar solicitud"
5. Ingresar ID de la solicitud (ej: SOL0001)
```

## 📈 Reportes Disponibles

1. **Herramientas con stock bajo**: Lista herramientas con menos de 3 unidades disponibles
2. **Préstamos activos**: Muestra todos los préstamos que no han sido devueltos
3. **Préstamos vencidos**: Identifica préstamos cuya fecha de devolución ya pasó
4. **Historial por usuario**: Muestra todos los préstamos de un usuario específico
5. **Herramientas más solicitadas**: TOP 5 de herramientas con más préstamos
6. **Usuarios más activos**: TOP 5 de usuarios con más préstamos realizados

## 🔧 Detalles Técnicos de Validaciones

### Validación de Teléfonos (10 dígitos exactos)
```python
if not telefono.isdigit():
    print("Error: El telefono debe contener solo numeros.")
if len(telefono) != 10:
    print("Error: El telefono debe tener exactamente 10 digitos.")
```

### Validación de Nombres (solo letras)
```python
# Permite espacios para nombres compuestos
if not nombre.replace(" ", "").isalpha():
    print("Error: El nombre debe contener solo letras.")
```

### Validación de Fechas (formato y validez)
```python
# Valida formato YYYY-MM-DD
# Verifica que la fecha sea real (no acepta 2026-02-30)
# Calcula años bisiestos correctamente
# Compara que devolución > inicio
```

**Ejemplo de año bisiesto:**
- 2024: Es bisiesto → Febrero tiene 29 días ✓
- 2026: No es bisiesto → Febrero tiene 28 días ✓
- Acepta: 2024-02-29 ✓
- Rechaza: 2026-02-29 ✗

### Validación de Códigos (sin espacios)
```python
if " " in codigo:
    print("Error: El codigo no puede contener espacios.")
# Convierte automáticamente a mayúsculas
codigo = codigo.upper()  # "tal001" → "TAL001"
```

## 🔒 Seguridad

- Sistema de login obligatorio
- Contraseñas almacenadas en archivo JSON (en producción real usar hash)
- Validación de permisos en cada operación
- Registro de todos los intentos de acceso en logs

## ✅ Sistema de Validaciones

El sistema incluye validaciones exhaustivas para garantizar la integridad de los datos:

### Validaciones de Usuarios:
- **Identificación**: Solo números, 6-15 dígitos, sin duplicados
- **Nombre/Apellido**: Solo letras (permite espacios para nombres compuestos), mínimo 2 caracteres
- **Teléfono**: Exactamente 10 dígitos numéricos (ejemplo: 3001234567)
- **Dirección**: Mínimo 5 caracteres
- **Tipo**: Solo "residente" o "administrador"

### Validaciones de Herramientas:
- **Código**: Mínimo 3 caracteres, sin espacios, sin duplicados, convertido a mayúsculas
- **Nombre**: Mínimo 3 caracteres
- **Categoría**: Solo letras (permite espacios)
- **Cantidad**: Número entero no negativo
- **Estado**: Solo "activa", "en reparación" o "fuera de servicio"
- **Valor**: Número decimal no negativo

### Validaciones de Fechas:
- **Formato obligatorio**: YYYY-MM-DD
- **Validación de fechas reales**: Rechaza fechas inválidas como 2026-02-30
- **Días por mes**: 
  - Abril, Junio, Septiembre, Noviembre: máximo 30 días
  - Febrero: 28 días (29 en años bisiestos)
- **Años bisiestos**: Cálculo automático correcto
- **Comparación**: Fecha de devolución debe ser posterior a fecha de inicio
- **Rango válido**: Años entre 2020-2100

### Ejemplos de Validación:

✅ **Aceptados:**
```
Teléfono: 3001234567 (10 dígitos)
Nombre: Juan Carlos (letras con espacio)
Fecha: 2026-02-17 (formato válido)
Identificación: 1001234567 (10 dígitos numéricos)
Código herramienta: TALADRO001 (sin espacios)
```

❌ **Rechazados:**
```
Teléfono: 300-123-4567 (contiene guiones)
Teléfono: 123456789 (solo 9 dígitos)
Nombre: Juan123 (contiene números)
Fecha: 2026-02-30 (febrero no tiene 30 días)
Fecha: 26-02-17 (formato incorrecto)
Identificación: ABC123 (contiene letras)
Código: TAL 001 (contiene espacio)
```

## 📝 Persistencia de Datos

Todos los datos se guardan automáticamente en archivos JSON:
- `herramientas.json`: Inventario de herramientas
- `usuarios.json`: Base de datos de usuarios
- `prestamos.json`: Registro de préstamos
- `solicitudes.json`: Solicitudes pendientes y procesadas
- `credenciales.json`: Usuarios y contraseñas
- `logs.json`: Eventos del sistema

## ⚠️ Consideraciones Importantes

1. **Primer uso**: El sistema crea automáticamente el usuario admin con contraseña admin123
2. **Códigos únicos**: Cada herramienta y usuario debe tener un código/ID único
3. **Formato de fechas**: Usar siempre formato YYYY-MM-DD (ejemplo: 2026-02-17)
   - El sistema valida que las fechas sean reales (rechaza 2026-02-30)
   - Calcula correctamente años bisiestos
   - La fecha de devolución debe ser posterior a la fecha de inicio
4. **Teléfonos**: Deben tener exactamente 10 dígitos sin guiones ni espacios (ejemplo: 3001234567)
5. **Nombres y apellidos**: Solo pueden contener letras (se permiten espacios para nombres compuestos)
6. **Identificaciones**: Solo números, entre 6 y 15 dígitos
7. **Stock**: Al aprobar un préstamo, se descuenta automáticamente del inventario
8. **Devolución**: Al devolver, se restaura automáticamente el stock
9. **Logs**: Todos los eventos importantes quedan registrados
10. **Validaciones**: El sistema valida todos los datos de entrada antes de guardarlos

## 🐛 Solución de Problemas

**Problema**: No puedo iniciar sesión
- **Solución**: Verifica que existe el archivo `credenciales.json`. Si no, el sistema lo creará con usuario `admin` y contraseña `admin123`

**Problema**: No se guardan los datos
- **Solución**: Verifica que tienes permisos de escritura en el directorio del proyecto

**Problema**: Error al crear préstamo
- **Solución**: Verifica que la herramienta esté en estado "activa" y haya stock disponible

**Problema**: "Formato de fecha inválido"
- **Solución**: Usa el formato YYYY-MM-DD. Ejemplos válidos:
  - ✓ 2026-02-17
  - ✓ 2026-12-31
  - ✗ 17-02-2026 (formato incorrecto)
  - ✗ 2026-02-30 (febrero no tiene 30 días)
  - ✗ 2026/02/17 (usar guiones, no barras)

**Problema**: "El teléfono debe tener exactamente 10 dígitos"
- **Solución**: Ingresa el teléfono sin guiones ni espacios:
  - ✓ 3001234567
  - ✗ 300-123-4567
  - ✗ 300 123 4567
  - ✗ 123456789 (solo 9 dígitos)

**Problema**: "El nombre debe contener solo letras"
- **Solución**: No uses números ni caracteres especiales en nombres:
  - ✓ Juan Carlos (letras y espacios permitidos)
  - ✓ María José
  - ✗ Juan123
  - ✗ Juan@Carlos

**Problema**: "El código no puede contener espacios"
- **Solución**: Códigos de herramientas sin espacios:
  - ✓ TALADRO001
  - ✓ TAL-001
  - ✗ TAL 001

## 📞 Soporte

Para reportar errores o solicitar nuevas funcionalidades, contacta al desarrollador del proyecto.

## 📄 Licencia

Proyecto educativo - Uso libre para aprendizaje

## ✅ Checklist de Entregables

- [x] Código fuente en archivos .py organizados en módulos
- [x] Archivos de persistencia generados por el programa (.json)
- [x] Archivo de logs con eventos relevantes (logs.json)
- [x] Documento README.md con instrucciones de ejecución
- [x] Carpeta de pruebas con casos de entrada y salida
- [x] Sistema de validaciones completo
- [x] Documento de validaciones explicadas (VALIDACIONES_COMPLETAS.md)

---

**Versión**: 3.0 (con validaciones completas)  
**Última actualización**: Febrero 2026