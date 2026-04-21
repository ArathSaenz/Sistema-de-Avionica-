Sistema de Telemetria para Interpretacion de Datos de Vuelo

Software Node-Red
Estación Terrena (Node-RED Local)
Node-RED actúa como el "Control de Misión" instalado de forma local en tu laptop, permitiendo el monitoreo offline en el campo de lanzamiento.
Ingesta de Datos: Recibe la telemetría a través de un nodo serial (conectado a la antena receptora) o vía el protocolo MQTT si existe un puente intermedio.
Procesamiento y Parseo: Utiliza nodos de función en JavaScript para extraer los valores de altitud, velocidad, y coordenadas.
Filtrado: Se aplican filtros básicos (como promedios móviles) para suavizar las gráficas de altitud y evitar falsos positivos por ruido en el sensor barométrico.
Interfaz de Usuario (Dashboard):
Gauges: Visualización inmediata de la velocidad vertical y estado de la batería.
Gráficos de Línea: Histórico de altitud en tiempo real para observar la trayectoria de vuelo.
Mapa: Integración con nodos de mapas para rastrear la ubicación del cohete en tiempo real mediante las coordenadas GPS recibidas.
Almacenamiento Local: Los datos entrantes se guardan automáticamente en archivos .csv o .json dentro de tu laptop. Esto garantiza que, si la visualización falla, los datos crudos del vuelo estén protegidos para un análisis posterior (Post-Flight Analysis).

1. Bloques de Adquisición (Entrada)
Entrada Serial Cohete: Es el "oído" del sistema. Escucha el puerto USB de tu laptop donde está conectado el receptor. Recibe la cadena de texto (string) que manda el cohete.
Inject: Este es tu bloque de pruebas. Te permite simular datos manualmente para verificar que los indicadores (Gauges) se muevan sin necesidad de tener el cohete encendido.

2. Bloques de Procesamiento (Lógica)
function 1 / Convertir Datos: Estos nodos actúan como un Parser. Toman la cadena de texto desordenada y la convierten en un objeto que Node-RED entiende.
Extraer datos grafica / text: Aquí separas las variables. El sistema divide el paquete de datos en canales individuales:
Numéricos: Para los indicadores de aguja (Gauges).
Strings/Texto: Para las etiquetas de texto que muestran el valor exacto.
Avionica_Datos (Write File): Este es un bloque crítico. Todo lo que llega se escribe en un archivo local. Si la laptop se apaga o Node-RED se cierra, los datos del vuelo quedan guardados en ese archivo para el análisis posterior.

3. Bloques de Visualización (Salida)
Dashboard (Azules): Son los nodos de Altitud, Velocidad, Presión y Temperatura. Cada uno alimenta un widget en tu interfaz http://localhost:1880/ui.
Worldmap & GPS: * GPS Recuperacion: Recibe las sentencias NMEA o coordenadas directas.
function world map: Traduce las coordenadas al formato que requiere el nodo de mapa.
worldmap: Coloca un marcador en el mapa real para que sepas dónde cayó el cohete.
Trayectoria del Cohete (Chart): Genera una gráfica de líneas (generalmente Altitud vs Tiempo) para visualizar el perfil de vuelo.
