# Hallazgos y reglas de ingeniería

Revisión estática del código; no se ejecutaron pruebas.

## Backend

- **Hallazgo:** `generate_mock_movements` establece la semilla del generador aleatorio global, lo que puede afectar otras llamadas en el proceso. **Regla:** usa un generador local por llamada para conservar resultados reproducibles sin alterar estado global. [backend/app/routes.py](backend/app/routes.py), [backend/tests/test_routes.py](backend/tests/test_routes.py).
- **Hallazgo:** CORS permite cualquier origen y también credenciales. **Regla:** en despliegues reales, limita los orígenes y no combines comodín con credenciales. [backend/app/main.py](backend/app/main.py).
- **Hallazgo:** las rutas declaran modelos Pydantic y restricciones de parámetros; algunas pruebas comprueban principalmente estructura. **Regla:** conserva validación explícita y añade pruebas de resultados para filtros y cálculos. [backend/app/routes.py](backend/app/routes.py), [backend/tests/test_routes.py](backend/tests/test_routes.py).

## Frontend

- **Hallazgo:** `new Date("YYYY-MM-DD")` seguido de getters locales puede desplazar fechas al mes anterior según la zona horaria. **Regla:** agrupa fechas ISO sin conversión local ambigua y prueba fechas del primer día de mes. [frontend/src/lib/financial-utils.ts](frontend/src/lib/financial-utils.ts), [frontend/src/lib/financial-utils.test.ts](frontend/src/lib/financial-utils.test.ts).
- **Hallazgo:** el encabezado muestra un período fijo mientras los datos se generan según la fecha actual. **Regla:** deriva la etiqueta del rango real de los datos o filtros activos. [frontend/src/App.tsx](frontend/src/App.tsx), [frontend/src/components/dashboard/dashboard-header.tsx](frontend/src/components/dashboard/dashboard-header.tsx), [backend/app/routes.py](backend/app/routes.py).
- **Hallazgo:** cálculos y formatos están separados de la presentación, y la aplicación maneja carga y error de API. **Regla:** conserva estas responsabilidades y añade pruebas unitarias para la lógica aislada. [frontend/src/lib/financial-utils.ts](frontend/src/lib/financial-utils.ts), [frontend/src/lib/financial-utils.test.ts](frontend/src/lib/financial-utils.test.ts), [frontend/src/App.tsx](frontend/src/App.tsx).

## Integración

- **Hallazgo:** backend y frontend mantienen definiciones del contrato en archivos distintos. **Regla:** al cambiar campos o valores permitidos, actualiza los modelos Pydantic, los tipos TypeScript y las pruebas correspondientes. [backend/app/routes.py](backend/app/routes.py), [frontend/src/lib/financial-types.ts](frontend/src/lib/financial-types.ts).
- **Hallazgo:** el proxy de Vite apunta al servicio `backend` de Compose. **Regla:** si cambian el nombre del servicio o el puerto, actualiza ambas configuraciones y verifica `/api`. [docker-compose.yml](docker-compose.yml), [frontend/vite.config.ts](frontend/vite.config.ts).
- **Hallazgo:** la documentación describe el arranque local. **Regla:** actualiza los pasos y direcciones cuando cambien los puertos o el flujo de desarrollo. [README.es.md](README.es.md), [docker-compose.yml](docker-compose.yml), [frontend/vite.config.ts](frontend/vite.config.ts).