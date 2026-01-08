
import time
import paho.mqtt.client as mqtt

# ---------- CONFIGURACIÓN MQTT ----------
BROKER = "broker.hivemq.com"
PORT = 8000                 # WebSocket Port
TOPIC = "raspberry/temperatura/marcos"

# ---------- FUNCIÓN TEMPERATURA ----------
def obtener_temperatura():
    with open("/sys/class/thermal/thermal_zone0/temp", "r") as f:
        temp_miligrados = int(f.read())
    return temp_miligrados / 1000.0  # °C

# ---------- CALLBACK DE CONEXIÓN ----------
def on_connect(client, userdata, flags, rc):
    if rc == 0:
        print("Conectado a HiveMQ correctamente")
    else:
        print("Error de conexión. Código:", rc)

# ---------- CLIENTE MQTT ----------
client = mqtt.Client(
    client_id="raspi-marcos",
    transport="websockets"
)

client.ws_set_options(path="/mqtt")
client.on_connect = on_connect

# Conectar al broker
client.connect(BROKER, PORT, keepalive=60)
client.loop_start()

# ---------- ENVÍO DE DATOS ----------
while True:
    temperatura = obtener_temperatura()

    mensaje = {
        "name": "Marcos Narvaez",
        "temperatura": temperatura
    }

    client.publish(TOPIC, str(mensaje))
    print("Mensaje enviado:", mensaje)

    time.sleep(5)  # envía cada 5 segundos
