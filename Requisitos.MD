# Requisitos Módulo Viajes
Documentación de Ingeniería de Requisitos para la optimización del pedido del cliente.

---

## 1. Requisitos No Funcionales (RNF)
* **RNF-01 (Seguridad):** El sistema debe implementar cifrado de extremo a extremo mediante protocolos SSL/TLS para proteger la identidad de los choferes y los datos de geolocalización frente a accesos no autorizados.
* **RNF-02 (Rendimiento/Concurrencia):** La arquitectura del servidor debe soportar un mínimo de 1,000 solicitudes simultáneas por segundo (req/s) sin presentar degradación en el tiempo de respuesta.

---

## 2. Historias de Usuario (User Stories)

### US-01: Ver Viajes Cercanos en Tiempo Real
* **Como:** Chofer de la aplicación.
* **Quiero:** Visualizar en un mapa interactivo las solicitudes de viajes disponibles que estén cerca de mi ubicación actual en tiempo real.
* **Para:** Seleccionar y aceptar solicitudes de traslado de manera eficiente.

### US-02: Cálculo de la Ruta Óptima
* **Como:** Chofer de la aplicación.
* **Quiero:** Obtener la ruta de navegación más corta hacia el destino una vez aceptado un viaje.
* **Para:** Minimizar el consumo de combustible y reducir el tiempo de traslado del pasajero.

---

## 3. Criterios de Aceptación (Formato Gherkin)

### Escenario 1: Visualización correcta de solicitudes cercanas
* **Dado que** el chofer se encuentra activo en la aplicación y con el GPS encendido.
* **Cuando** un pasajero solicita un viaje en un radio menor a 5 kilómetros.
* **Entonces** el sistema debe mostrar la alerta del viaje en el mapa en un tiempo menor a 2 segundos.

### Escenario 2: Generación de ruta óptima al aceptar viaje
* **Dado que** el chofer ha presionado el botón "Aceptar viaje".
* **Cuando** el sistema procese la confirmación del viaje.
* **Entonces** la pantalla debe trazar la ruta de navegación más corta calculada mediante el API de mapas, mostrando la distancia estimada y el tiempo de arribo.
