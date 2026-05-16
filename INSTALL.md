# Guía Rápida de Instalación

## 🚀 Opción 1: Instalación Local

### Requisitos
- Python 3.8+
- pip
- Git

### Pasos

```bash
# 1. Clonar repositorio
git clone https://github.com/raerdi10-svg/baremo-dec-548-arg.git
cd baremo-dec-548-arg

# 2. Crear entorno virtual (opcional pero recomendado)
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

# 3. Instalar dependencias
cd backend
pip install -r requirements.txt

# 4. Inicializar base de datos
python -c "from src.database import init_db; init_db()"

# 5. Ejecutar servidor
python run.py
```

✅ Servidor disponible en: **http://localhost:5000**

---

## 🐳 Opción 2: Con Docker

### Requisitos
- Docker
- Docker Compose

### Pasos

```bash
# 1. Clonar repositorio
git clone https://github.com/raerdi10-svg/baremo-dec-548-arg.git
cd baremo-dec-548-arg

# 2. Construir y ejecutar
docker-compose up --build

# 3. (Opcional) Ejecutar en background
docker-compose up -d
```

✅ Servidor disponible en: **http://localhost:5000**

---

## 📋 Primeros Pasos en la Interfaz Web

### 1. **Crear un Paciente**
   - Completa: Nombre, Apellido, Email, DNI
   - Datos opcionales: Fecha nacimiento, Género, Teléfono, Domicilio
   - Click: "Guardar Paciente"

### 2. **Registrar Evaluación Médica**
   - Fecha de evaluación (auto-completada con hoy)
   - Datos del médico (opcional)
   - Observaciones
   - Click: "Siguiente: Deficiencias"

### 3. **Cargar Deficiencias**
   - Selecciona sistema corporal (7 opciones)
   - Ingresa porcentaje (0-100%)
   - Datos adicionales (opcional)
   - Click: "Agregar Deficiencia"
   - Repite para cada deficiencia

### 4. **Calcular**
   - Click: "Calcular Discapacidad"
   - Sistema automáticamente:
     - Calcula discapacidad base
     - Aplica factores de ponderación
     - Convierte a pesos con inflación 2026

### 5. **Ver Resultados**
   - Discapacidad Base y Total (%)
   - Quantum base y con inflación (pesos)
   - Clasificación y factores aplicados
   - Resumen del cálculo

### 6. **Consultar Historial**
   - Tab "Historial"
   - Ver todos los cálculos del paciente
   - Información: Fecha, %, Clasificación, Quantum

---

## 🔧 Archivos Principales

| Archivo | Propósito |
|---------|-----------|
| `backend/src/app.py` | Aplicación Flask |
| `backend/src/api.py` | Rutas API REST |
| `backend/src/calculator.py` | Lógica de cálculo |
| `backend/src/database.py` | Modelo de datos |
| `backend/templates/index.html` | Frontend HTML |
| `backend/static/js/app.js` | Lógica JavaScript |
| `backend/static/css/style.css` | Estilos CSS |
| `backend/requirements.txt` | Dependencias |

---

## 🧪 Probar la API con cURL

### Crear Usuario
```bash
curl -X POST http://localhost:5000/api/v1/usuarios \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Juan",
    "apellido": "Pérez",
    "email": "juan@example.com",
    "dni": "12345678"
  }'
```

### Crear Evaluación
```bash
curl -X POST http://localhost:5000/api/v1/evaluaciones \
  -H "Content-Type: application/json" \
  -d '{
    "usuario_id": 1,
    "fecha_evaluacion": "2026-05-16",
    "deficiencias": [
      {"sistema": "locomotor", "porcentaje": 45},
      {"sistema": "visual", "porcentaje": 20}
    ]
  }'
```

### Calcular Discapacidad
```bash
curl -X POST http://localhost:5000/api/v1/calcular \
  -H "Content-Type: application/json" \
  -d '{"evaluacion_id": 1}'
```

---

## 📊 Base de Datos

La base de datos SQLite se crea automáticamente en `backend/data/baremo.db`

### Tablas:
- **usuarios** - Información de pacientes
- **evaluaciones** - Evaluaciones médicas
- **deficiencias** - Deficiencias registradas
- **calculos** - Cálculos realizados
- **auditoria** - Log de operaciones

---

## ⚙️ Configuración

### Modificar inflación (2026)
Archivo: `backend/src/calculator.py`
```python
FACTOR_INFLACION = 1.15  # Cambiar este valor
```

### Cambiar puerto del servidor
Archivo: `backend/run.py`
```python
app.run(port=5000)  # Cambiar puerto
```

---

## 🐛 Solución de Problemas

### Error: "Port 5000 already in use"
```bash
# Cambiar puerto en run.py o usar:
python run.py --port 5001
```

### Error: "No module named 'flask'"
```bash
pip install -r backend/requirements.txt
```

### Base de datos corrupta
```bash
rm backend/data/baremo.db
python -c "from src.database import init_db; init_db()"
```

---

## 📚 Documentación Completa

Ver documentos en la carpeta `/docs`:
- `FORMULA.md` - Explicación detallada de la fórmula
- `API.md` - Documentación completa de API
- `EXAMPLES.md` - Casos de uso prácticos
- `TABLAS.md` - Tablas de referencia

---

## 🚀 Próximas Mejoras (Roadmap)

- [ ] Autenticación de usuarios
- [ ] Exportar resultados a PDF
- [ ] Integración con histórico médico
- [ ] Gráficos y reportes
- [ ] Versión mobile (PWA)
- [ ] Multilenguaje (EN, PT)
- [ ] API GraphQL

---

## 📞 Soporte

¿Problemas? Abre un issue en GitHub o contacta al equipo de desarrollo.

**Estado:** ✅ Funcional y en producción
**Última actualización:** 2026-05-16
