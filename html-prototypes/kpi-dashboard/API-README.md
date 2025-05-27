# Transport Company KPI Dashboard API

This document outlines the API endpoints available for the Transport Company KPI Dashboard, providing data for all defined KPIs.

## API Overview

The API is designed with a focus on minimizing the number of endpoints while providing flexible access to all KPI data. The endpoints are structured to support both:

1. **Quick dashboard loading** - A consolidated endpoint for all summary data
2. **Detailed views** - Specific endpoints for individual KPI details
3. **Cross-KPI analysis** - Endpoints for analyzing data across business segments

## Base URL

```
https://api.transport-company.com/v1
```

## Authentication

API requests require authentication using a Bearer token in the Authorization header:

```
Authorization: Bearer your-api-token
```

## Available Endpoints

### 1. Dashboard Data

Get all KPI summary data in a single request for the main dashboard view.

```
GET /dashboard
```

**Response example:**
```json
{
  "timestamp": "2025-03-15T12:05:00Z",
  "kpis": {
    "ventas": {
      "id": "ventas",
      "title": "Ventas Totales",
      "value": "4.85M$",
      "numericValue": 4850000,
      "status": "good",
      "statusText": "7.8% sobre meta",
      "context": "Mes Actual (Meta: 4.5M$)"
    },
    "margen": {
      "id": "margen",
      "title": "Margen Operativo",
      "value": "12.5%",
      "numericValue": 12.5,
      "status": "good",
      "statusText": "Tendencia Positiva",
      "context": "Mes Acumulado (Ayer: 11.8%)"
    }
    // ... other KPIs
  },
  "businessSegments": ["Foráneo", "Cruce", "Local", "Marítimo"],
  "overallPerformance": {
    "financialScore": 82,
    "operationalScore": 91,
    "safetyScore": 78
  }
}
```

### 2. KPI List

Get a list of all available KPIs with summary data.

```
GET /kpis
```

### 3. KPI Detail

Get detailed information for a specific KPI.

```
GET /kpis/{kpiId}
```

Parameters:
- `kpiId` (required): ID of the KPI to retrieve (e.g., "ventas", "margen", "utilizacion")

**Response example:**
```json
{
  "id": "ventas",
  "title": "Ventas Totales",
  "value": "4.85M$",
  "numericValue": 4850000,
  "status": "good",
  "statusText": "7.8% sobre meta",
  "context": "Mes Actual (Meta: 4.5M$)",
  "description": "Total de ingresos generados por fletes en el periodo.",
  "segments": [
    {"name": "Foráneo", "value": "2.6M$", "trend": "up", "color": "--foraneo-color"},
    {"name": "Cruce (Transfronterizo)", "value": "1.7M$", "trend": "neutral", "color": "--cruce-color"},
    {"name": "Local", "value": "0.55M$", "trend": "up", "color": "--local-color"}
  ],
  "trendsBySegment": {
    "labels": ["Ene", "Feb", "Mar", "Abr", "May", "Jun", "Jul", "Ago", "Sep", "Oct", "Nov", "Dic"],
    "series": [
      {
        "name": "Foráneo",
        "color": "--foraneo-color",
        "data": [2.4, 2.45, 2.5, 2.52, 2.55, 2.58, 2.6, 2.6, 2.6, 2.6, 2.6, 2.6]
      },
      // ... other series
    ]
  },
  "metrics": [
    {"label": "Meta anual", "value": "58M$"},
    {"label": "YTD", "value": "14.6M$ (25.2%)"},
    {"label": "Proyección anual", "value": "62.4M$ (+7.6%)"}
  ],
  "additionalData": {
    // KPI-specific additional data
  }
}
```

### 4. Segment Performance

Get performance data grouped by business segments.

```
GET /kpis/segments
```

Parameters:
- `segment` (optional): Filter for specific segment ("Foráneo", "Cruce", "Local", "Marítimo")

### 5. Metric Trends

Get time-based trend data for KPIs.

```
GET /metrics/trends
```

Parameters:
- `kpiIds` (optional): Comma-separated list of KPI IDs to retrieve trends for
- `period` (optional): Time period for trends ("monthly", "quarterly", "yearly")
- `segment` (optional): Filter by business segment

## KPI IDs

The following KPI IDs are available:

- `ventas` - Ventas Totales (Total Sales)
- `margen` - Margen Operativo (Operating Margin)
- `utilizacion` - Utilización de Flota (Fleet Utilization)
- `otd` - Entregas a Tiempo (On-Time Delivery)
- `cruce` - Tiempo Promedio Cruce Fronterizo (Average Border Crossing Time)
- `costo-km` - Costo por Kilómetro (Cost Per Kilometer)
- `rpm` - Ingreso por Kilómetro (Revenue Per Mile)
- `ingreso-camion` - Ingreso Promedio por Camión (Average Revenue per Truck)
- `costo-mtto` - Costo Mantenimiento por Kilómetro (Maintenance Cost per Kilometer)
- `dso` - Días Cuentas por Cobrar (Days Sales Outstanding)
- `seguridad` - Incidentes Seguridad / MMkm (Safety Incidents per Million Kilometers)
- `satisfaccion` - Satisfacción Operador (Operator Satisfaction)

## Error Handling

All endpoints return standard HTTP status codes:

- `200 OK` - The request was successful
- `400 Bad Request` - The request was invalid or cannot be served
- `401 Unauthorized` - Authentication is required
- `404 Not Found` - The requested resource does not exist
- `500 Internal Server Error` - An error occurred on the server

Error responses include a JSON object with an error message:

```json
{
  "error": "KPI not found"
}
```

## Implementation Example

A sample implementation of the API can be found in the `api-example.js` file, which demonstrates how to structure the API responses with mock data. 