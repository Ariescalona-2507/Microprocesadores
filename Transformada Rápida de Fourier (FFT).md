# Transformada Rápida de Fourier (FFT) #

    init.py (en la raíz del paquete)

**Para qué sirve:** Este archivo indica que el directorio actual debe ser tratado como un paquete de Python. Además, controla qué elementos se exportan cuando alguien usa from package import *.

**Cómo funciona:** Cuando Python importa un paquete, primero ejecuta este archivo. Puede estar vacío o contener código de inicialización. Para controlar las importaciones, se define la lista _all_ con los nombres de los submódulos que se desean exportar. Por ejemplo: _all_ = ['fourier', 'signal'].

---

    init.py (dentro de /fourier/)

**Para qué sirve:** Hace que fourier sea un submódulo del paquete principal. Permite exponer selectivamente las funciones o clases definidas en FFT.py al nivel del submódulo.

**Cómo funciona:** Cuando se importa from package.fourier import ..., Python ejecuta este archivo. Se puede usar para importar funciones de FFT.py directamente en el espacio de nombres del submódulo, por ejemplo: from .FFT import transformar para que luego pueda usarse como package.fourier.transformar().

---

    FFT.py

**Para qué sirve:** Contiene la implementación específica de algoritmos de Transformada Rápida de Fourier (FFT).

**Cómo funciona:** Este módulo define funciones y clases relacionadas con cálculos de FFT, como calcular_fft(senal) o calcular_fft_inversa(coeficientes). Se importa desde el _init_.py de fourier o directamente como from package.fourier.FFT import calcular_fft.

---

    init.py (dentro de /signal/)

**Para qué sirve:** Similar al de fourier, convierte el directorio signal en un submódulo y controla qué partes de signal.py se exponen.

**Cómo funciona:** Se ejecuta al importar el submódulo. Puede contener, por ejemplo: from .signal import filtrar, generar_senal para que estas funciones estén disponibles directamente bajo package.signal.filtrar().

---

    signal.py

**Para qué sirve:** Contiene funciones y clases para procesamiento de señales, como generación, filtrado o análisis básico.

**Cómo funciona:** Implementa la lógica de procesamiento de señales, por ejemplo, una función filtrar(senal, frecuencia_corte) o una clase Senal. Es el núcleo funcional del submódulo signal.

---

Archivos pycache y .pyc

**Para qué sirven:** Son archivos de bytecode compilado que Python genera para acelerar la importación de módulos.


**Cómo funcionan:** Cuando se importa un módulo por primera vez, Python lo compila a bytecode (.pyc) y lo guarda en la carpeta _pycache_. En importaciones posteriores, carga el bytecode (si no hubo cambios en el fuente), reduciendo el tiempo de inicio. Son generados y gestionados automáticamente por Python.
