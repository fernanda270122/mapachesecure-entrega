# 🦝 MapacheSecure — Control Parental Inteligente

> Plataforma móvil de control parental que permite a padres supervisar y gestionar el uso del dispositivo de sus hijos mediante bloqueo de apps, desafíos, recompensas y seguimiento de actividad en tiempo real.

---

## 🔗 Links del proyecto

| Recurso | URL |
|---|---|
| 🌐 Landing Page | https://raccu.cl |
| 📱 Repositorio Frontend | https://github.com/fernanda270122/mapachesecure-app |
| ⚙️ Repositorio Backend | https://github.com/fernanda270122/mapachesecure-backend |

---

## 📁 Estructura del repositorio

```
mapachesecure-entrega/
├── Documentacion/       # Informes y documentación del proyecto
├── Evidencias/          # Evidencias de pruebas y funcionalidades
├── Gestion/             # Documentos de gestión y registro del equipo
├── Producto/            # Repositorios del frontend y backend
├── Integrantes.txt      # Integrantes del equipo
└── README.md
```

---

## 📱 Frontend — Flutter App

Aplicación móvil desarrollada en **Flutter 3.44.4**, con soporte principal para Android. Implementa dos roles diferenciados: **padre** e **hijo**, cada uno con su propio flujo y pantallas.

### 👨‍👧 Funcionalidades por rol

**Para el padre:**
- Panel de supervisión con actividad del hijo en tiempo real
- Creación y asignación de desafíos con evidencias revisables
- Tienda de recompensas y gestión de canjes pendientes
- Bloqueo remoto de aplicaciones en el dispositivo del hijo
- Configuración de perfil del hijo con avatar personalizable
- Consejos y guías para padres

**Para el hijo:**
- Pantalla de inicio con resumen de desafíos y puntos
- Selección de avatar y temas visuales personalizados
- Vista de desafíos activos con subida de evidencias
- Tienda de recompensas canjeables con puntos ganados
- Pantalla de bloqueo cuando una app está restringida
- Evolución visual de la mascota (Mapache 🦝)

### 🛠️ Tecnologías (Frontend)

| Tecnología | Uso |
|---|---|
| Flutter 3.44.4 / Dart ^3.11.5 | Framework principal |
| Firebase (Core + Messaging) | Autenticación y notificaciones push |
| Provider | Gestión de estado |
| flutter_background_service | Servicio guardian en segundo plano |
| usage_stats | Estadísticas de uso de apps del hijo |
| system_alert_window | Pantalla de bloqueo sobre otras apps |
| flutter_local_notifications | Notificaciones locales |
| flutter_screenutil | Diseño responsivo |
| table_calendar | Calendario de actividad |
| shared_preferences | Persistencia local |

### ⚙️ Instalación (Frontend)

```bash
# Clonar el repositorio
git clone https://github.com/fernanda270122/mapachesecure-app
cd mapachesecure-app

# Instalar dependencias
flutter pub get

# Ejecutar en modo debug
flutter run
```

> ⚠️ La app requiere el archivo `lib/firebase_options.dart` generado con `flutterfire configure`. No se incluye en el repositorio por seguridad.

### 🔐 Permisos requeridos (Android)

| Permiso | Motivo |
|---|---|
| `PACKAGE_USAGE_STATS` | Leer el uso de apps del hijo |
| `SYSTEM_ALERT_WINDOW` | Mostrar pantalla de bloqueo sobre otras apps |
| `POST_NOTIFICATIONS` | Notificaciones locales y push |

### 🏗️ Arquitectura (Frontend)

```
lib/
├── main.dart                  # Entrada, rutas y splash
├── models/                    # Entidades de dominio (Usuario, Desafio, Recompensa…)
├── providers/                 # Estado global con Provider
├── screens/
│   ├── auth/                  # Login, registro, recuperación de contraseña
│   ├── onboarding/            # Flujo de bienvenida por rol
│   ├── padre/                 # Pantallas del rol padre
│   └── hijo/                  # Pantallas del rol hijo
├── services/                  # Comunicación con API, Firebase y guardian
├── theme/                     # Colores, paletas y fondos
└── utils/                     # Utilidades de responsividad
```

### 🔗 Deep Links

La app responde al esquema `mapachesecure://` para el flujo de restablecimiento de contraseña:

```
mapachesecure://reset-password#access_token=<token>&type=recovery
```

---

## ⚙️ Backend — FastAPI

API REST desarrollada en **Python 3.11 + FastAPI**, desplegada en Render. Gestiona toda la lógica de negocio: autenticación, desafíos, recompensas, bloqueos y generación de contenido con IA.

### 🛠️ Tecnologías (Backend)

| Tecnología | Uso |
|---|---|
| Python 3.11 + FastAPI 0.135 | Framework principal |
| Supabase | Base de datos y autenticación |
| Google GenAI / Groq / Anthropic | Generación de desafíos con IA |
| Firebase Admin | Notificaciones push |
| Resend | Envío de correos transaccionales |
| Render | Despliegue en producción |

### ⚙️ Instalación (Backend)

```bash
# Clonar el repositorio
git clone https://github.com/fernanda270122/mapachesecure-backend
cd mapachesecure-backend

# Crear entorno virtual e instalar dependencias
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Crear un archivo `.env` en la raíz del proyecto:

```env
SUPABASE_URL=https://<tu-proyecto>.supabase.co
SUPABASE_KEY=<tu-service-role-key>
GOOGLE_API_KEY=<clave-google-genai>
ANTHROPIC_API_KEY=<clave-anthropic>
GROQ_API_KEY=<clave-groq>
FIREBASE_CREDENTIALS=<json-credenciales-firebase>
RESEND_API_KEY=<clave-resend>
```

```bash
# Ejecutar el servidor
uvicorn app.main:app --reload
```

- API disponible en: `http://localhost:8000`
- Documentación interactiva: `http://localhost:8000/docs`

### 📡 Endpoints principales

| Módulo | Ruta base | Descripción |
|---|---|---|
| Autenticación | `/auth` | Registro, login, logout, recuperación de contraseña |
| Usuarios | `/usuarios` | Gestión de perfiles de padres e hijos |
| Desafíos | `/desafios` | CRUD de desafíos asignados |
| Recompensas | `/recompensas` | Definición y gestión de recompensas |
| Canjes | `/canjes` | Canje de recompensas por puntos |
| Apps | `/apps` | Registro de aplicaciones monitoreadas |
| Bloqueos | `/bloqueos` | Control parental de aplicaciones |
| IA | `/ia` | Generación personalizada de desafíos con IA |
| Notificaciones | `/notificaciones` | Envío de push notifications |
| Actividad | `/actividad` | Historial de actividad digital del hijo |

### 🏗️ Arquitectura (Backend)

```
app/
├── main.py              # Punto de entrada FastAPI
├── database.py          # Conexión Supabase
├── dependencies.py      # Autenticación JWT
├── routers/             # Endpoints por módulo
│   ├── auth.py
│   ├── usuarios.py
│   ├── desafios.py
│   ├── recompensas.py
│   ├── canjes.py
│   ├── apps.py
│   ├── bloqueos.py
│   ├── ia.py
│   ├── notificaciones.py
│   └── actividad.py
├── services/            # Lógica de negocio
└── repositories/        # Acceso a datos (Supabase)
```

### 🔒 Seguridad

- Dependencias auditadas sin vulnerabilidades conocidas (`pip-audit`)
- Autenticación mediante JWT validado en cada endpoint protegido
- Variables sensibles gestionadas exclusivamente por variables de entorno

---

## 🧪 Testing

El proyecto cuenta con **834 pruebas en total**, distribuidas según la pirámide de testing:

| Nivel | Tipo | Cantidad |
|---|---|---|
| 🟢 Base | Unitarias (526 Flutter + 129 Backend pytest) | 655 |
| 🟡 Medio | Integración, Funcionales, Regresión, Seguridad y Carga | 164 |
| 🔴 Cima | Smoke / E2E en dispositivo físico (4 con API real) | 15 |

### Correr tests

**Frontend:**
```bash
flutter test --coverage
```

**Backend:**
```bash
pip install -r requirements-test.txt
pytest tests/ -v
```

---

## 🔄 CI/CD — GitHub Actions

### Flutter CI
Se ejecuta en cada push o PR a `main` y `dev`:

- ✅ Verificación de formato (`dart format`)
- ✅ Análisis estático (`flutter analyze`, 0 warnings)
- ✅ 558 pruebas con cobertura (`flutter test --coverage`)
- ✅ Reporte `lcov.info` subido como artefacto (retención 7 días)

### Backend CI/CD
Se ejecuta en cada push o PR a `main` y `dev`:

- ✅ 276 pruebas con `pytest` en Python 3.11 entorno controlado
- ✅ Deploy automático a producción en Render si todo está en verde

---

## 👥 Equipo

Ver archivo `Integrantes.txt`

---

## 📌 Versión

**Backend:** v1.1.6
