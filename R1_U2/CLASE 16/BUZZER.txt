from machine import Pin
import time

# Configuración de pines
trigger_pin = Pin(14, Pin.OUT)
echo_pin = Pin(27, Pin.IN)
buzzer_pin = Pin(21, Pin.OUT)

# Función para medir la distancia utilizando el sensor ultrasónico
def medir_distancia():
    # Generar un pulso corto en el pin de disparo del sensor
    trigger_pin.on()
    time.sleep_us(10)
    trigger_pin.off()

    # Esperar a que el pin de eco se active y luego medir el tiempo de duración
    while echo_pin.value() == 0:
        pulse_start = time.ticks_us()
    while echo_pin.value() == 1:
        pulse_end = time.ticks_us()

    # Calcular la duración del pulso y convertirla a distancia en centímetros
    pulse_duration = time.ticks_diff(pulse_end, pulse_start)
    distance_cm = (pulse_duration * 0.0343) / 2

    return distance_cm

try:
    while True:
        distancia = medir_distancia()
        print("Distancia:", distancia, "cm")

        # Si la distancia es menor o igual a 10cm, encender el zumbador
        if distancia <= 10:
            buzzer_pin.on()
        else:
            buzzer_pin.off()

        time.sleep(0.1)  # Esperar 0.1 segundos antes de la siguiente medición

except KeyboardInterrupt:
    print("Programa interrumpido")