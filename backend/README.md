# Backend API - Baremo Decreto 548

API REST para cálculo de discapacidad según el Decreto 548 de Argentina.

## 🚀 Quick Start

### Instalación

```bash
cd backend
npm install
```

### Configuración

```bash
cp .env.example .env
# Editar .env si es necesario
```

### Desarrollo

```bash
npm run dev
```

El servidor estará disponible en `http://localhost:3000`

### Producción

```bash
npm run build
npm start
```

## 📚 API Endpoints

### Cálculo de Discapacidad

**POST** `/api/calculate`

Calcula el porcentaje total de discapacidad y el quantum (monto en pesos).

```bash
curl -X POST http://localhost:3000/api/calculate \
  -H "Content-Type: application/json" \
  -d '{
    "deficiencias": [
      {"sistema": "locomotor", "porcentaje": 45},
      {"sistema": "visual", "porcentaje": 20}
    ]
  }'
```

**Respuesta:**
```json
{
  "exito": true,
  "discapacidad": {
    "discapacidadBase": 58.9,
    "discapacidadTotal": 67.73,
    "categoria": "Discapacidad Grave",
    "numeroSistemas": 2,
    "deficienciasAjustadas": [...],
    "factoresAplicados": [...]
  },
  "cuantum": {
    "porcentajeDiscapacidad": 67.73,
    "categoria": "Grave",
    "montoBase": 75000,
    "montoAjustado": 86250,
    "validoHasta": "2026-12-31"
  }
}
```

### Cálculo de Quantum

**POST** `/api/calculate/quantum`

Calcula solo el quantum (monto en pesos) a partir de un porcentaje.

```bash
curl -X POST http://localhost:3000/api/calculate/quantum \
  -H "Content-Type: application/json" \
  -d '{"porcentajeDiscapacidad": 67.73}'
```

### Validación

**POST** `/api/validate`

Valida deficiencias sin calcular.

```bash
curl -X POST http://localhost:3000/api/validate \
  -H "Content-Type: application/json" \
  -d '{
    "deficiencias": [
      {"sistema": "locomotor", "porcentaje": 45}
    ]
  }'
```

### Tablas de Referencia

**GET** `/api/tablas`

Obtiene todas las tablas (sistemas y escala de quantum).

**GET** `/api/sistemas`

Obtiene los 7 sistemas corporales disponibles.

**GET** `/api/categorias`

Obtiene las categorías de discapacidad.

**GET** `/api/quantum-escala`

Obtiene la escala de quantum.

### Health Check

**GET** `/health`

Verifica el estado de la API.

## 📋 Sistemas Corporales

- **locomotor** - Extremidades, columna, articulaciones
- **visual** - Visión, agudeza visual
- **auditivo** - Audición, sordera
- **mental** - Cognición, inteligencia
- **visceral** - Órganos vitales, respiratorio
- **dermatologico** - Piel, cicatrices
- **raquimedular** - Médula espinal, paraplejía

## 🧮 Fórmula

La discapacidad se calcula usando:

1. **Ajuste por Sistema**: Porcentaje × Factor
2. **Resta Cruzada OMS**: D1 + D2 - (D1×D2/100)
3. **Factores de Ponderación**: Multiplicidad, Complejidad
4. **Límite**: Máximo 100%

## 🔧 Estructura

```
backend/
├── src/
│   ├── index.ts                 # Entry point
│   ├── app.ts                   # Express app
│   ├── controllers/
│   │   └── disability.controller.ts
│   ├── services/
│   │   ├── disability.service.ts
│   │   └── quantum.service.ts
│   ├── routes/
│   │   ├── disability.routes.ts
│   │   └── health.routes.ts
│   └── constants/
│       └── baremo-tables.ts
├── package.json
├── tsconfig.json
├── Dockerfile
└── .env.example
```

## 🧪 Testing

```bash
npm test
```

## 📦 Docker

```bash
docker build -t baremo-backend .
docker run -p 3000:3000 baremo-backend
```

## 📄 Licencia

MIT
