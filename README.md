# NetConfig Backup Tool

> Software de automatización para realizar y gestionar backups de configuraciones de dispositivos de red Mikrotik (RouterOS). Arquitectura cliente-servidor en Java con scripts nativos de RouterOS.

---

## El problema que resuelve

En entornos de red con múltiples dispositivos Mikrotik, realizar backups manuales de configuraciones es un proceso tedioso, propenso a olvidos y difícil de auditar. Si un dispositivo falla o es mal configurado, recuperar la configuración anterior depende de que alguien haya hecho el backup a tiempo.

Este software automatiza ese proceso: agenda y ejecuta backups de todos los dispositivos de la red desde un servidor centralizado, permitiendo revisar el historial de configuraciones desde una interfaz web.

---

## Arquitectura

El sistema está compuesto por tres capas:

```
┌─────────────────────────────────────────┐
│           Cliente Web (JS)              │
│     Visualización e historial de        │
│     backups por dispositivo             │
└─────────────────┬───────────────────────┘
                  │ HTTP
┌─────────────────▼───────────────────────┐
│           BackupApi (Java)              │
│     API REST que orquesta los           │
│     backups y gestiona el almacenamiento│
└─────────────────┬───────────────────────┘
                  │ SSH / API RouterOS
┌─────────────────▼───────────────────────┐
│     Dispositivos Mikrotik (RouterOS)    │
│     Scripts de exportación de           │
│     configuración (.rsc)                │
└─────────────────────────────────────────┘
```

---

## Stack tecnológico

| Componente | Tecnología |
|-----------|-----------|
| Backend / API | Java |
| Frontend cliente | JavaScript + HTML + CSS |
| Scripts de dispositivo | RouterOS Script |
| Comunicación con routers | SSH / API RouterOS |
| IDE | IntelliJ IDEA |

---

## Estructura del repositorio

```
├── BackupApi/
│   └── BackupApi/        # API REST en Java (servidor principal)
├── client/
│   └── my-app/           # Interfaz web del cliente
```

---

## Funcionalidades

**Backend (BackupApi)**
- Conexión y autenticación con dispositivos Mikrotik vía SSH o API RouterOS
- Ejecución de scripts de exportación de configuración en los routers
- Almacenamiento y versionado de los archivos de backup generados
- Exposición de endpoints REST para que el cliente consulte el historial

**Scripts RouterOS**
- Exportación de la configuración completa del dispositivo (`/export`)
- Guardado en archivos `.rsc` recuperables ante una falla

**Cliente web**
- Listado de dispositivos configurados
- Historial de backups por dispositivo con fecha y hora
- Descarga o visualización de configuraciones anteriores

---

## Instalación

### Requisitos previos
- Java 11 o superior
- Dispositivos Mikrotik accesibles en red con SSH habilitado
- Credenciales de acceso a cada dispositivo

### Backend

```bash
cd BackupApi/BackupApi

# Compilar el proyecto
javac -cp . src/**/*.java

# Ejecutar el servidor
java -jar BackupApi.jar
```

### Cliente web

```bash
cd client/my-app

# Abrir index.html en el navegador
# o servir con cualquier servidor HTTP estático
```

---

## Configuración de dispositivos Mikrotik

Para que el sistema pueda conectarse a los routers, cada dispositivo debe tener:

```routeros
# Habilitar SSH en el router
/ip service enable ssh

# Crear usuario con permisos de lectura para backups
/user add name=backup-user group=read password=tu-password
```

---

## Flujo de un backup

1. El cliente web o un scheduler interno dispara una solicitud a la API
2. La API se conecta al dispositivo Mikrotik por SSH
3. Ejecuta el script de exportación (`/export`)
4. Recibe y almacena el archivo `.rsc` generado con timestamp
5. El historial queda disponible en la interfaz web

---

## Estado del proyecto

- ✅ Conexión y autenticación con dispositivos Mikrotik
- ✅ Ejecución de scripts RouterOS y exportación de configuraciones
- ✅ API REST para gestión de backups
- ✅ Interfaz web para visualización e historial

---

## Autor

**Tobías** — [@TobyX73](https://github.com/TobyX73)
