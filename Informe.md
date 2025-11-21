# CONVERTIDOR DE UNA SEÑAL ANALÓGICA A DIGITAL TOMANDO LA SEÑAL EXISTENTE DEL RASPBERRY PI PICO RP2040
 # Importación de módulos y Dependencias
 from machine import ADC, Pin 
 import time 
 
 **Análisis Técnco**
**machine:** Módulo fundamental de MicroPython que proporciona acceso directo al hardware y periférico del microcontrolador. 
**ADC:** Clase especializada para controlar el conversor analógico-digital del sistema.
**Pin:** Clase para gestión de pines de E/S, esencial para configurar el canal ADC. 
**time:** Módulo estándar que proporciona funciones de temporización, crucial para el muestreo periódico.

# Definición de la Clase ConversorADC
**Constructor e Inicialización**
def __init__(self, Pin_adc=26):
    self.adc = ADC(Pin(Pin_adc))
    self.resolucion = 16
    self.vref = 3.3

**Funcionamiento y estructura:**
**Parámetros pin_adc:** Especifíca el pin GPIO conectadon al ADC (valor por defecto 26, común en Raspberry Pi Pico).
**Instalación del ADC:** ADC(Pin(Pin_adc)) configura el hardware para funcionar como entradaanalógica.
**Resolución:** Establecida en 16 bits, correspondiente al rango completo del ADC (0-65535).
**Voltaje de referencia (vref):** 3.3V, valor típico en microcontroladores de 3.3V, determina el rango de medida.

**Método leer raw()**
def leer_raw(self):
    """Lee valor ADC sin procesar (0-65535)"""
    return self.read_u16()

**Características técnicas:**
**read_u16():** Método nativo que retorna un valor enero sin rango de 16 bits.
**Rango de salida:** 0 a 65535(2^16-1), representando la cuanización completa de ADC.
**Resolución efectiva:** Depende del hardware subyacente (generalmente 12 bits reales escaladas a 16).

**Método lee voltaje()**
def leer_voltaje(self):
    """Convierte a voltaje (0-3.3V)
    raw = self.leer:raw()
    return (raw * self.vref) / 65535

**Procedimiento de señal:**
**Fórmula de conversión: V =(valor_raw x Vref) / (2n-1)
**Precisión:** La división por 65535 (216-1) normaliza el valor raw al rango[0, Vref]
**Aplicación:** Útil para interpretación directa de magnitudes físicas en voltios.

**Método leer porcentaje()**
def leer_porcentaje(self):
    """Convierte a porcentaje (0-100%)"""
    raw = self.leer_raw()
    return (raw / 65535) * 100

Transformación matemática:
**Normalización:** raw / 65535 produce valor en rango [0,1]
**Escalado lineal:** Multiplicación por 100 para conversión a porcentaje 
**Caso de uso:** Ideal paa potenciómetro, sensores de posición e indicadores 

**Método leer 12bits()**
def leer_12bits(salef):
    """Convierte a resolución nativa de 12 bits (0-4095)"""
    raw = self.leer_raw()
    return raw >> 4

**Operación a nivel de bits**
**Desplazamiento derecho:** >> 4 equivale a división entera por 16(2^4)
**Justificación:** Muchos ADCs internos tienen resolución nativa de 12 bits (0-4095)
**Optimización:** Operación computacionalmente eficiente versus división 

**Implementación del objetivo**
adc = conversorADC(26)

**Canal ADC:** Pin 26 seleccionado para adquisición 
**Inicialización implícita:** Todos los párametros configurados con valores por defecto


**Bucle de Muestreo Continuo**
while True:
     raw = adc.leer_raw()
     voltaje = adc.leer_voltaje()
     porcentaje = adc.leer_porcentaje()
     bits12 = adc.leee_12bits()
     print(f"Raw: ´{raw:5d} | {voltaje:.2f}v | {porcentaje:.1f}% | 12b: {bits12:4d}")

**Arquitectura de adquisición:**

**Muestreo síncrono:** Lectura secuencial de todas las representaciones
**Frecuencia de Muestreo:** 2 Hz (periódo de 500 ms)
**Formateo de salida:** Presentación estructurada con especifícadores de formato
**Persistencia:** Bucle infinitos para monitorización continua.



```python 
from machine import ADC, Pin
import time

class ConversorADC:
    def __init__(self, pin_adc=26):
        self.adc = ADC(Pin(pin_adc))
        self.resolucion = 16  
        self.vref = 3.3       
    
    def leer_raw(self):
        """Lee valor ADC sin procesar (0-65535)"""
        return self.adc.read_u16()
    
    def leer_voltaje(self):
        """Convierte a voltaje (0-3.3V)"""
        raw = self.leer_raw()
        return (raw * self.vref) / 65535
    
    def leer_porcentaje(self):
        """Convierte a porcentaje (0-100%)"""
        raw = self.leer_raw()
        return (raw / 65535) * 100
    
    def leer_12bits(self):
        """Convierte a resolución nativa de 12 bits (0-4095)"""
        raw = self.leer_raw()
        return raw >> 4  

adc = ConversorADC(26)

while True:
    raw = adc.leer_raw()
    voltaje = adc.leer_voltaje()
    porcentaje = adc.leer_porcentaje()
    bits12 = adc.leer_12bits()
    
    print(f"RAW: {raw:5d} | {voltaje:.2f}V | {porcentaje:.1f}% | 12b: {bits12:4d}")
    time.sleep(0.5)

```