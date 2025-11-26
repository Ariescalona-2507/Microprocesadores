**#Importación de Módulo**
import math
import cmath

**Funcionalidad y Estructura**

**math:** Proporciona funciones matemáticas básicas (trigonométricas,logarírtmicas) para operaciones con números reales 
**cmath:** Extienden las funcionalidades matemáticas al dominio complejo, esencial para el cálculo de exponenciales complejas en la TDF

**Definición de la clase TDF:**
class TDF:
    def __init__(self, fs=100):
        self.fs = fs
**Funcionalidad y Estructura**

**Constructor __init__:** Inicializa la instancia con frecuencia de muestreo fs (valor por defecto: 100 Hz)

**Atributo self.fs:** Almacena la frecuencia de muestreo para cálculos posteriores de resolución frecuencial

**Método calcular - Núcleo de la TDF**
def calcular(self, x):
    N = len(x)
    X = []

**Funcionalidad:**

**N:** Determina el número de puntos de la transformada

**X:** Lista para almacenar los coeficientes espectrales complejos

**Algoritmo de Cálculo de TDF**

for k in range(N):
    suma = 0
    for n in range(N):
        exp = -2j * cmath.pi * k * n / N
        suma += x[n] * cmath.exp(exp)
    X.append(suma)

**Estructura y Funcionamiento:**

**Bucle Externo (k):** Itera sobre cada bin de frecuencia (0 a N-1)

**Bucle Interno (n):** Calcula la suma acumulativa para cada muestra temporal

**Exponencial Compleja:** Implementa el núcleo matemático de la TDF:

X[k] = Σ x[n] * e^(-j2πkn/N)

**Complejidad Computacional:** O(N²), característica de implementaciones directas de TDF

**Análisis Técnico:** Esta implementación es conceptualmente clara pero computacionalmente intensiva. En microprocesadores, sería recomendable considerar algoritmos FFT (O(N log N)) para N grandes.

**Separación de Coeficientes Pares e Impares**

pares = []
impares = []

for k in range(N):
    if k % 2 == 0:
        pares.append(X[k])
    else:
        impares.append(X[k])

**Funcionalidad:** Clasifica los coeficientes de frecuencia según la paridad del índice k, útil para análisis de simetrías espectrales.

 **Cálculo de Magnitud y Fase**

 mag = [abs(x) for x in X]
fase = [cmath.phase(x) * 180 / cmath.pi for x in X]

**Conversiones:**

**Magnitud:** Módulo del número complejo (abs())

**Fase:** Argumento en grados (conversión de radianes mediante * 180 / π)

 **Bloque Principal de Prueba**

**Configuración de Parámetros** 

fs = 100
N = 16
t = [i/fs for i in range(N)]

**Especificaciones:**

**Frecuencia de Muestreo:** 100 Hz

**Puntos de Muestra:** 16

**Vector Temporal:** Generación equiespaciada de 0 a (N-1)/fs

 **Generación de Señal de Prueba**

 señal = []
for t_i in t:
    componente_10Hz = 2.0 * math.cos(2 * math.pi * 10 * t_i)
    componente_25Hz = 1.0 * math.sin(2 * math.pi * 25 * t_i)
    componente_DC = 0.5
    ruido = 0.1 * math.sin(2 * math.pi * 37 * t_i)
    señal.append(componente_10Hz + componente_25Hz + componente_DC + ruido)

**Composición de la Señal:**

**Componente 10 Hz:** Coseno de amplitud 2.0

**Componente 25 Hz:** Seno de amplitud 1.0

**Componente DC:** Offset de 0.5

**Ruido:** Seno de 37 Hz con amplitud 0.1

**Análisis Espectral Esperado:** Picos en 0 Hz (DC), 10 Hz y 25 Hz, con componente menor en 37 Hz.

**Ejecución y Presentación de Resultados**

tdf = TDF(fs=fs)
mag, fase, X, pares, impares = tdf.calcular(señal)

Salida Estructurada: Presentación tabular de índices, frecuencias, magnitudes, fases y clasificación par/impar.

**Análisis Estadístico**

print(f"Magnitud promedio pares: {sum(abs(x) for x in pares)/len(pares):.4f}")
print(f"Magnitud promedio impares: {sum(abs(x) for x in impares)/len(impares):.4f}")

**Métricas Adicionales:** Proporciona información sobre la distribución espectral entre componentes pares e impares.


import math
import cmath

class TDF:
    def __init__(self, fs=100):
        self.fs = fs
    
    def calcular(self, x):
        """Calcula TDF y retorna magnitudes, fases, pares e impares"""
        N = len(x)
        X = []
        
        for k in range(N):
            suma = 0
            for n in range(N):
                exp = -2j * cmath.pi * k * n / N
                suma += x[n] * cmath.exp(exp)
            X.append(suma)
        
        
        pares = []
        impares = []
        
        for k in range(N):
            if k % 2 == 0:
                pares.append(X[k])
            else:
                impares.append(X[k])
        
        mag = [abs(x) for x in X]
        fase = [cmath.phase(x) * 180 / cmath.pi for x in X]
        
        return mag, fase, X, pares, impares


if __name__ == "__main__":
    print("=== PRUEBA DE TRANSFORMADA DISCRETA DE FOURIER ===\n")
    
    fs = 100  
    N = 16    
    t = [i/fs for i in range(N)]  
    
    señal = []
    for t_i in t:
        componente_10Hz = 2.0 * math.cos(2 * math.pi * 10 * t_i)
        componente_25Hz = 1.0 * math.sin(2 * math.pi * 25 * t_i)
        componente_DC = 0.5  
        ruido = 0.1 * math.sin(2 * math.pi * 37 * t_i) 
        señal.append(componente_10Hz + componente_25Hz + componente_DC + ruido)
    
    print(f"Señal de entrada ({N} muestras):")
    for i, valor in enumerate(señal):
        print(f"  Muestra {i:2d}: {valor:7.3f}")
    
    tdf = TDF(fs=fs)
    mag, fase, X, pares, impares = tdf.calcular(señal)
    
    print(f"\nResultados de la TDF:")
    print(f"{'k':>2} {'Frec (Hz)':>8} {'Magnitud':>10} {'Fase (°)':>10} {'Par/Impar':>10}")
    print("-" * 60)
    
    for k in range(N):
        freq = k * fs / N
        tipo = "PAR" if k % 2 == 0 else "IMPAR"
        print(f"{k:2d} {freq:8.2f} {mag[k]:10.4f} {fase[k]:10.2f} {tipo:>10}")
    
    print(f"\nResumen de pares e impares:")
    print(f"Coeficientes pares (k par): {len(pares)} elementos")
    print(f"Coeficientes impares (k impar): {len(impares)} elementos")
    
    print(f"\nMagnitud promedio pares: {sum(abs(x) for x in pares)/len(pares):.4f}")
    print(f"Magnitud promedio impares: {sum(abs(x) for x in impares)/len(impares):.4f}")