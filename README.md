<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=24&duration=2600&pause=900&color=8A7FFF&center=true&vCenter=true&width=820&lines=Travel+Planner+Multidestino;Grafos+%C2%B7+Dijkstra+%C2%B7+TSP+con+memoizaci%C3%B3n;FastAPI+%2B+Streamlit+%2B+IA+para+recomendar+lugares" alt="Typing SVG" />

<a href="https://github.com/estebanghinamo"><img src="https://img.shields.io/badge/⬅_Perfil-181717?style=for-the-badge&logo=github&logoColor=white" alt="Perfil"/></a>

</div>

---

### 🗺️ Sobre el proyecto

Sistema de planificación de viajes multidestino: el usuario elige una o varias ciudades, un medio de transporte y un criterio de optimización (costo o tiempo), y el sistema calcula la ruta óptima y la compara con la ruta elegida manualmente.

Trabajo final de **Programación Eficiente (PEF)** — Integrantes: **Esteban Ghinamo**, Nicolás Moresco y Santiago Flores · Profesora: Valeria Daniele.

- 🧭 **Grafo de ciudades** (europeas, generadas con distancia Haversine) con **Dijkstra** para el camino más corto entre origen y destino, filtrando por transporte (auto, tren, avión).
- 🧩 **Múltiples ciudades**: resuelto como un **TSP con programación dinámica** (bitmasking, variante Held-Karp) y **memoización**.
- ⚡ **Cache LRU** local (con variante TTL) para rutas y optimizaciones repetidas, más un **cache distribuido con Redis** (opcional).
- 🔄 **Reservas asincrónicas** (`asyncio`) con estados `pending → processing → confirmed / failed / cancelled`, y **procesamiento batch** para cargas masivas de reservas.
- 🤖 **Recomendaciones con IA** (Google Gemini) de lugares relacionados según las ciudades elegidas.
- 🌐 **API REST con FastAPI**, con documentación interactiva automática en `/docs` (Swagger UI), e **interfaz gráfica con Streamlit**.
- ✅ **Testing con Pytest** (29 tests) + reporte de cobertura HTML, y profiling con `py-spy` / `memory-profiler`.

---

### 🛠️ Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![scikit--learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Pytest](https://img.shields.io/badge/Pytest-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google_Gemini-886FBF?style=for-the-badge&logo=googlegemini&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

</div>

---

### 🧩 Arquitectura

```text
travel_planner/
└── app/
    ├── core/          # graph.py (Dijkstra) · tsp_dp.py (TSP + memoización) · itinerary_validator.py
    ├── data/           # generación de rutas entre ciudades (distancia Haversine)
    ├── caches/         # lru_cache.py (LRU + TTL) · redis_cache.py (cache distribuido)
    ├── booking/        # reservations.py (async) · batching.py (procesamiento por lotes)
    ├── api/            # server.py — endpoints FastAPI
    ├── ui/             # streamlit_app.py — interfaz gráfica
    ├── ai/             # gemini_recommendations.py — recomendaciones con IA
    └── tests/          # suite de Pytest
```

### 🚀 Endpoints principales

| Método | Endpoint | Descripción |
|---|---|---|
| GET | `/health` | Estado de la API |
| GET | `/routes/matrix` | Matriz de costos/tiempos entre ciudades |
| POST | `/routes/shortest` | Camino más corto (Dijkstra) entre dos ciudades |
| GET | `/routes/compare` | Compara ruta elegida vs. ruta optimizada |
| POST | `/routes/optimize-multi` | Optimiza el orden de varias ciudades (TSP) |
| POST | `/itinerary/plan` | Arma un itinerario validado |
| POST | `/reservations` | Crea una reserva |
| POST | `/reservations/batch` | Crea reservas en lote |
| GET | `/reservations/user/{user_id}` | Reservas de un usuario |
| DELETE | `/reservations/{reservation_id}` | Cancela una reserva |
| GET | `/stats` | Estadísticas del sistema |

---

### ▶️ Cómo correrlo

```bash
cd travel_planner
# activar entorno virtual e instalar dependencias
pip install -r requirements.txt

# variable de entorno para las recomendaciones con IA (opcional)
export GOOGLE_API_KEY="tu_api_key"

# levantar la API (documentación interactiva en /docs)
uvicorn app.api.server:app --reload

# en otra terminal, levantar la interfaz
streamlit run app/ui/streamlit_app.py
```

- API: `http://127.0.0.1:8000` (docs en `http://127.0.0.1:8000/docs`)
- Interfaz: `http://localhost:8501`

### ✅ Tests

```bash
pytest
```

29 tests cubriendo Dijkstra, TSP (tour cerrado y abierto), validación de itinerarios, cache LRU/TTL, reservas asincrónicas, procesamiento batch y los endpoints principales de FastAPI. `pytest.ini` genera además un reporte de cobertura HTML (`htmlcov/index.html`).

---

<div align="center"><sub>Córdoba, Argentina 🇦🇷</sub></div>
