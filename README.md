# ESP32 Smart Textile Controller

This Arduino sketch controls a smart textile system with temperature and humidity monitoring, automatic relay control, and web API integration.

## Hardware Requirements

- ESP32 Development Board
- DS18B20 Waterproof Temperature Sensor
- DHT22 Temperature & Humidity Sensor
- Relay Module (for Peltier control)
- Peltier Cooling Element
- Jumper wires and breadboard

## Pin Connections

| Component | ESP32 Pin | Notes |
|-----------|-----------|-------|
| DS18B20 Data | GPIO 4 | Requires 4.7kΩ pull-up resistor |
| DHT22 Data | GPIO 5 | Built-in pull-up usually sufficient |
| Relay Control | GPIO 16 | Controls Peltier element |
| Status LED | GPIO 2 | Built-in LED for connection status |

## Required Libraries

Install these libraries through the Arduino IDE Library Manager:

1. **WiFi** (ESP32 built-in)
2. **HTTPClient** (ESP32 built-in)
3. **ArduinoJson** by Benoit Blanchon
4. **OneWire** by Jim Studt
5. **DallasTemperature** by Miles Burton
6. **DHT sensor library** by Adafruit

## Configuration

1. **WiFi Settings**: Update the WiFi credentials in the code:
   \`\`\`cpp
   const char* ssid = "YOUR_WIFI_SSID";
   const char* password = "YOUR_WIFI_PASSWORD";
   \`\`\`

2. **Server URL**: Update the server URL to match your deployment:
   \`\`\`cpp
   const char* serverURL = "http://your-server-url.com";
   \`\`\`
   
   For local development, use your computer's IP address:
   \`\`\`cpp
   const char* serverURL = "http://192.168.1.100:3000";
   \`\`\`

## Features

### Sensor Monitoring
- **DS18B20**: Waterproof digital temperature sensor
- **DHT22**: Combined temperature and humidity sensor
- **Error Detection**: Handles sensor disconnection and read failures

### Automatic Control
- **Temperature Control**: Activates Peltier when temperature exceeds threshold
- **Humidity Control**: Activates Peltier when humidity exceeds threshold
- **Smart Logic**: Uses readings from both sensors for reliable control

### Web Integration
- **Real-time Data**: Sends sensor data to web dashboard every 5 seconds
- **Remote Configuration**: Fetches updated thresholds from server
- **Health Monitoring**: Built-in connectivity and status checking

### Connection Management
- **WiFi Auto-reconnect**: Automatically reconnects if connection is lost
- **Status Indication**: Built-in LED shows connection status
- **Error Recovery**: Graceful handling of network issues

## API Endpoints Used

- `POST /api/sensor` - Sends sensor data to server
- `GET /api/esp32-config` - Fetches configuration from server
- `GET /api/health` - Health check endpoint

## Serial Monitor Output

The ESP32 provides detailed logging through the Serial Monitor (115200 baud):

\`\`\`
Smart Textile Controller Starting...
Connecting to WiFi: YourNetwork
WiFi connected successfully!
IP address: 192.168.1.150

=== Sensor Readings ===
DS18B20 Temp: 25.3 °C
DHT22 Temp: 24.8 °C
DHT22 Humidity: 45.2 %

Sending sensor data: {"ds18b20Temp":25.3,"dhtTemp":24.8,"humidity":45.2,"relayStatus":false}
HTTP Response: 200
Data sent successfully!
\`\`\`

## Troubleshooting

### WiFi Connection Issues
- Verify SSID and password are correct
- Check WiFi signal strength
- Ensure ESP32 is within range of router

### Sensor Reading Errors
- Check wiring connections
- Verify pull-up resistors (4.7kΩ for DS18B20)
- Test sensors individually

### Server Communication Issues
- Verify server URL is correct and accessible
- Check firewall settings
- Ensure server is running and API endpoints are active

### Power Issues
- Use adequate power supply (5V 2A recommended)
- Check if Peltier element draws too much current
- Consider using external power for relay/Peltier

## Customization

### Timing Intervals
\`\`\`cpp
const unsigned long SENSOR_INTERVAL = 2000;    // Sensor reading frequency
const unsigned long SEND_INTERVAL = 5000;      // Data transmission frequency
const unsigned long CONFIG_INTERVAL = 30000;   // Configuration update frequency
\`\`\`

### Threshold Values
Default thresholds can be modified in the code, but they will be overridden by server configuration:
\`\`\`cpp
float tempThreshold = 30.0;     // Temperature threshold (°C)
float humidityThreshold = 70.0; // Humidity threshold (%)
\`\`\`

### Pin Assignments
Modify pin definitions at the top of the code if using different GPIO pins:
\`\`\`cpp
#define ONE_WIRE_BUS 4    // DS18B20 data pin
#define DHTPIN 5          // DHT22 data pin
#define RELAY_PIN 16      // Relay control pin
