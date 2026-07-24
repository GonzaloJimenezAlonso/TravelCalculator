 # ⛽ Despesas Viagem

Aplicación web para calcular el coste de un viaje en coche teniendo en cuenta el consumo del vehículo, el precio del combustible, los peajes y la distancia del recorrido. La aplicación funciona íntegramente en el navegador, sin necesidad de un servidor propio, y está optimizada para dispositivos móviles, pudiendo instalarse como una **Progressive Web App (PWA)**.

---

# Características

- 🚗 Gestión de múltiples vehículos con consumos personalizables.
- 📍 Selección de origen y destino mediante direcciones o lugares favoritos.
- 🔍 Autocompletado de direcciones usando OpenStreetMap.
- 🔄 Intercambio rápido entre origen y destino.
- 🛣️ Cálculo automático de distancia y duración de la ruta.
- ⛽ Consulta del precio del combustible en tiempo real.
- 💶 Gestión automática de peajes por trayecto.
- 🔁 Cálculo de viajes de ida o de ida y vuelta.
- 💾 Almacenamiento local de configuraciones y preferencias.
- 📱 Diseño responsive compatible con iOS y Android.

---

# Tecnologías utilizadas

- HTML5
- CSS3
- JavaScript (ES6)
- LocalStorage
- Fetch API
- OpenStreetMap Nominatim
- OSRM Routing API
- Precioil API

---

# Estructura del proyecto

```text
/
├── index.html
├── assets/
│   └── icon.png
└── README.md
```

Todo el proyecto se encuentra contenido en un único archivo `index.html`, que incluye el HTML, el CSS y la lógica JavaScript.

---

# Funcionamiento

## 1. Selección de origen y destino

El usuario puede elegir una ubicación de dos formas:

- Mediante los lugares predefinidos.
- Escribiendo una dirección y utilizando el autocompletado.

Los lugares favoritos actualmente incluidos son:

- Casa
- Sevilla
- Vila Nova

Cada uno dispone de:

- Nombre
- Dirección
- Coordenadas GPS

---

## 2. Geocodificación

Cuando el usuario introduce una dirección manualmente, la aplicación consulta la API de **OpenStreetMap Nominatim** para obtener sus coordenadas geográficas.

Las búsquedas realizadas se almacenan en caché mediante `LocalStorage`, evitando consultas repetidas.

---

## 3. Cálculo de la ruta

Una vez obtenidas las coordenadas del origen y el destino, la aplicación consulta la API pública de **OSRM (Open Source Routing Machine)** para calcular:

- Distancia del recorrido.
- Tiempo estimado.
- Ruta por carretera.

---

## 4. Vehículos

Actualmente se incluyen dos vehículos preconfigurados:

| Vehículo | Consumo |
|----------|---------:|
| SEAT Arona | 6.2 L/100 km |
| VW T-Roc | 5.5 L/100 km |

El consumo puede modificarse libremente y queda almacenado para futuras sesiones.

---

## 5. Precio del combustible

El precio puede introducirse manualmente o actualizarse automáticamente utilizando la API de **Precioil**.

Las estaciones configuradas son:

### España

- Repsol Cartaya (A49)

### Portugal

- Moeve Almodôvar (A2)
- Repsol Porto Salvo

Cuando se consulta el precio:

- Se obtiene el precio actual.
- Se almacena localmente.
- Se muestra la hora de la última actualización.

---

## 6. Gestión de peajes

La aplicación permite asociar peajes específicos para determinadas rutas.

Ejemplo:

| Trayecto | Peaje |
|----------|-------:|
| Casa → Sevilla | 23.80 € |
| Sevilla → Casa | 26.05 € |
| Casa → Vila Nova | 9.05 € |
| Vila Nova → Casa | 9.05 € |

Los peajes son editables y cualquier modificación queda almacenada automáticamente.

---

## 7. Viajes de ida y vuelta

Cuando se activa la opción **Ida y vuelta**, la aplicación:

- Duplica la distancia.
- Duplica el consumo.
- Permite introducir peajes independientes para la ida y para la vuelta.

Esto permite representar correctamente trayectos donde los peajes cambian según el sentido de circulación.

---

# Fórmulas utilizadas

## Combustible consumido

```text
Litros = (Kilómetros × Consumo) / 100
```

## Coste del combustible

```text
Coste combustible = Litros × Precio por litro
```

## Coste total

```text
Coste total = Combustible + Peajes
```

Para viajes de ida y vuelta:

```text
Distancia = Distancia × 2

Combustible = Combustible × 2

Peajes = Ida + Vuelta
```

---

# APIs utilizadas

## OpenStreetMap Nominatim

Se utiliza para:

- Buscar direcciones.
- Obtener coordenadas GPS.

No requiere autenticación.

---

## OSRM Routing

Se utiliza para:

- Calcular rutas.
- Obtener distancia.
- Obtener duración del recorrido.

No requiere autenticación.

---

## Precioil

Se utiliza para:

- Consultar el precio actualizado del combustible.
- Obtener información de estaciones de servicio.

Requiere una API Key.

---

# Persistencia de datos

Toda la información del usuario se almacena mediante **LocalStorage**, incluyendo:

- Consumo personalizado de cada vehículo.
- Precio del combustible.
- Caché de direcciones.
- Identificadores de estaciones de servicio.
- Peajes personalizados.

No se utiliza ninguna base de datos ni servidor externo.

---

# Arquitectura

```text
Usuario
    │
    ▼
Interfaz HTML
    │
    ▼
JavaScript
    │
 ├───────────────┐
 │               │
 ▼               ▼
Nominatim      OSRM
 │               │
 └───────┬───────┘
         ▼
 Cálculo del viaje
         │
         ▼
     Precioil API
         │
         ▼
 Resultado final
```

---

# Personalización

## Añadir un nuevo vehículo

Modificar el objeto:

```javascript
const CARS = {
    nuevo:{
        label:"Mi coche",
        consumo:5.8
    }
}
```

---

## Añadir un nuevo lugar favorito

Añadir una nueva entrada en el objeto `PLACES` indicando:

- Nombre.
- Dirección.
- Latitud.
- Longitud.

---

## Añadir una nueva estación de servicio

Será necesario:

1. Añadir el precio por defecto.
2. Configurar la estación dentro de `PRECIOIL_STATIONS`.
3. Añadir la opción correspondiente en el selector HTML.

---

# Posibles mejoras futuras

- ✅ Historial de viajes.
- 📊 Estadísticas mensuales de gasto.
- 👥 Reparto automático entre pasajeros.
- ⚡ Compatibilidad con vehículos eléctricos.
- 🗺️ Visualización de la ruta sobre un mapa.
- 🌍 Cálculo automático de peajes mediante API.
- ☁️ Sincronización entre dispositivos.
- 📄 Exportación de viajes en PDF o CSV.
- 🌙 Modo oscuro/claro configurable.

---

# Licencia

Proyecto desarrollado como herramienta personal para el cálculo de costes de viajes por carretera.

Puede utilizarse, modificarse y ampliarse libremente para uso personal.