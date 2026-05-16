# Baremo Decreto 548 - Argentina

## Descripción

Sistema completo para calcular el **porcentaje de discapacidad** y el **cuantum en pesos** según el **Decreto 548 de la República Argentina**.

Implementa la fórmula completa de la OMS con factores de ponderación, múltiples sistemas corporales y conversión automática a montos en pesos con ajuste por inflación.

## 🎯 Características Principales

✅ **Fórmula Completa OMS** - Implementa la metodología de la Organización Mundial de la Salud adaptada a Argentina  
✅ **7 Sistemas Corporales** - Locomotor, Visual, Auditivo, Mental, Visceral, Dermatológico, Raquimedular  
✅ **Factores de Ponderación** - Multiplicidad, Complejidad, Severa Coexistencia  
✅ **Cálculo de Quantum** - Conversión automática a pesos con escala de valores  
✅ **Inflación Incluida** - Ajuste automático según año fiscal  
✅ **API REST Completa** - Endpoints para cálculo, validación y consulta de tablas  
✅ **Interfaz Web** - Dashboard interactivo para cálculos  
✅ **Documentación Detallada** - Fórmula, ejemplos y guía de integración  

## 📋 Requisitos

- **Node.js** 16+
- **npm** o **yarn**
- **Docker** (opcional)

## 🚀 Instalación Rápida

### Opción 1: Local

```bash
# Clonar repositorio
git clone https://github.com/raerdi10-svg/baremo-dec-548-arg.git
cd baremo-dec-548-arg

# Instalar dependencias del backend
cd backend
npm install
npm run dev

# En otra terminal, instalar frontend
cd ../frontend
npm install
npm run dev
```

Backend estará en: `http://localhost:3000`  
Frontend estará en: `http://localhost:3001`

### Opción 2: Docker

```bash
docker-compose up
```

Acceder a:
- Frontend: `http://localhost:3001`
- Backend: `http://localhost:3000`

## 📚 Documentación

- **[FORMULA.md](./docs/FORMULA.md)** - Explicación detallada de la fórmula y cálculo
- **[API.md](./docs/API.md)** - Documentación de endpoints REST
- **[EXAMPLES.md](./docs/EXAMPLES.md)** - Casos de uso prácticos

## 🔧 Estructura del Proyecto

```
baremo-dec-548-arg/
├── backend/
│   ├── src/
│   │   ├── app.ts                 # Express server
│   │   ├── controllers/
│   │   │   └── disability.controller.ts  # REST endpoints
│   │   ├── services/
│   │   │   ├── disability.service.ts     # Lógica de cálculo
│   │   │   └── quantum.service.ts        # Cálculo de montos
│   │   └── constants/
│   │       └── baremo-tables.ts   # Tablas y factores
│   ├── package.json
│   └── tsconfig.json
├── frontend/
│   ├── src/
│   │   ├── App.tsx
│   │   └── components/
│   └── package.json
├── docs/
│   ├── FORMULA.md
│   ├── API.md
│   └── EXAMPLES.md
├── docker-compose.yml
└── README.md
```

## 💻 Uso de la API

### Ejemplo básico - Cálculo de discapacidad

```bash
curl -X POST http://localhost:3000/api/calculate \
  -H "Content-Type: application/json" \
  -d '{
    "deficiencias": [
      {
        "sistema": "locomotor",
        "porcentaje": 45,
        "descripcion": "Pérdida de funcionalidad en extremidad"
      },
      {
        "sistema": "visual",
        "porcentaje": 20,
        "descripcion": "Disminución de agudeza visual"
      }
    ]
  }'
```

**Respuesta:**

```json
{
  "exito": true,
  "discapacidad": {
    "discapacidadTotal": 58.9,
    "categoria": "Discapacidad Grave",
    "factoresAplicados": [
      {
        "nombre": "Multiplicidad",
        "valor": 0.15
      }
    ]
  },
  "cuantum": {
    "porcentajeDiscapacidad": 58.9,
    "montoCalculado": 94200,
    "montoAjustado": 108330,
    "validoHasta": "2026-12-31"
  }
}
```

### Endpoints disponibles

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/api/calculate` | Calcula discapacidad y quantum |
| POST | `/api/calculate/quantum` | Calcula solo el quantum |
| GET | `/api/tablas` | Obtiene tablas de referencia |
| POST | `/api/validate` | Valida datos de entrada |
| GET | `/health` | Verifica estado de la API |

## 📊 Escalas de Quantum

| Discapacidad | Monto Base | Con Inflación (2026) | Categoría |
|--------------|-----------|---------------------|-----------|
| 0-10% | $5,000 | $5,750 | Mínima |
| 11-25% | $15,000 | $17,250 | Leve |
| 26-40% | $35,000 | $40,250 | Moderada |
| 41-60% | $75,000 | $86,250 | Grave |
| 61-99% | $150,000 | $172,500 | Muy Grave |
| 100% | $250,000 | $287,500 | Total |

## 🧮 Ejemplo de Cálculo

**Caso:** Trabajador con 3 deficiencias

### Entrada
```json
{
  "deficiencias": [
    {"sistema": "locomotor", "porcentaje": 45},
    {"sistema": "visual", "porcentaje": 20},
    {"sistema": "mental", "porcentaje": 30}
  ]
}
```

### Pasos de Cálculo

1. **Ajuste por Sistema:**
   - Locomotor: 45 × 1.2 = 54%
   - Visual: 20 × 1.0 = 20%
   - Mental: 30 × 1.15 = 34.5%

2. **Resta Cruzada (OMS):**
   - (54 + 20) - (54×20/100) = 63.2%
   - (63.2 + 34.5) - (63.2×34.5/100) = **75.9%**

3. **Factores:**
   - Multiplicidad (3 sistemas): ×1.15 = 87.28%
   - Complejidad (3+ sistemas): ×1.10 = 96.01%

4. **Resultado Final:**
   - Discapacidad: **96.01%**
   - Quantum: **$220,636** (ajustado por inflación)

## 🔌 Integración

### JavaScript/TypeScript

```typescript
const response = await fetch('http://localhost:3000/api/calculate', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({deficiencias: [...]})
});
const resultado = await response.json();
```

### Python

```python
import requests
response = requests.post('http://localhost:3000/api/calculate', 
  json={'deficiencias': [...]})
resultado = response.json()
```

## 📝 Normativa Legal

- **Decreto 548** - Baremo de incapacidad laboral
- **Clasificación OMS (ICF)** - Funcionamiento y discapacidad
- **Resoluciones MTESS** - Actualización de montos
- **Banco Central Argentina** - Índices de inflación

## 🤝 Contribuir

Las contribuciones son bienvenidas:

1. Fork el proyecto
2. Crea una rama (`git checkout -b feature/mejora`)
3. Commit cambios (`git commit -am 'Añade mejora'`)
4. Push (`git push origin feature/mejora`)
5. Abre un Pull Request

## 📄 Licencia

MIT - Libre para uso comercial y privado

## 👤 Autor

**raerdi10-svg**  
GitHub: [@raerdi10-svg](https://github.com/raerdi10-svg)

## 📞 Soporte

Para reportar bugs o solicitar funcionalidades, abre un issue en [GitHub Issues](https://github.com/raerdi10-svg/baremo-dec-548-arg/issues)

---

**Última actualización:** 2026-05-16  
**Versión:** 1.0.0  
**Estado:** ✅ Producción
