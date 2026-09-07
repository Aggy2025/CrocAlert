# Croc Alert – Prototipo 5

![Croc Alert – Prototipo 5](images/components-overview.png)

Guía técnica de **programación, preparación electrónica, soldadura, ensamblaje y puesta en funcionamiento** del Prototipo 5 de Croc Alert.

---

## 1. Descripción

El Prototipo 5 es una evolución del sistema Croc Alert orientada a mejorar la captura de imágenes en condiciones de baja iluminación y a integrar todos los componentes electrónicos dentro de una carcasa más compacta.

Esta versión incorpora una **ESP32-CAM**, una cámara con lente para visión nocturna, una placa personalizada de **24 LED infrarrojos de 850 nm**, una placa de control CRCibernetica, un sistema de alimentación mediante baterías y una carcasa diseñada para integrar el conjunto.

El diseño del Prototipo 5 también incorpora una modificación de la placa temporizadora para añadir un puerto lógico y permitir que los LED sean controlados desde la ESP32.

---

## 2. Objetivos del Prototipo 5

El desarrollo de esta versión busca:

- Mejorar la iluminación en condiciones nocturnas.
- Integrar una mayor cantidad de LED infrarrojos.
- Utilizar iluminación infrarroja de 850 nm.
- Controlar la iluminación desde la ESP32.
- Reducir el tamaño de la solución mediante una carcasa más compacta.
- Integrar cámara, iluminación, control y alimentación en un solo dispositivo.
- Proteger el sistema frente a la humedad interna mediante material absorbente.

---

## 3. Arquitectura general

```text
                       ┌───────────────────┐
                       │     ESP32-CAM     │
                       └─────────┬─────────┘
                                 │
                                 │ Control / lógica
                                 │
                ┌────────────────▼────────────────┐
                │       Placa CRCibernetica      │
                └────────────────┬────────────────┘
                                 │
                                 │ Señal de control
                                 ▼
                ┌────────────────────────────────┐
                │    Placa de 24 LED IR 850 nm   │
                └────────────────────────────────┘

ESP32-CAM ───────────────────────► Cámara

Batería ────────────────────────► Sistema de alimentación
```

La iluminación infrarroja es controlada desde la ESP32 mediante la electrónica de control del prototipo.

### Circuito general

El siguiente diagrama muestra la conexión general del sistema de iluminación infrarroja y los principales elementos que intervienen en su funcionamiento. Se utiliza como referencia antes de comenzar el proceso de soldadura.

![Circuito general del Prototipo 5](images/circuito-general.png)

---

## 4. Componentes

### 4.1 Componentes electrónicos

| Componente | Función |
|---|---|
| ESP32-CAM | Control principal y ejecución del programa |
| Cámara | Captura de imágenes |
| Módulo programador | Programación temporal de la ESP32-CAM |
| Placa CRCibernetica | Integración y control electrónico |
| Placa de LED IR | Iluminación infrarroja |
| 24 LED IR 850 nm | Iluminación en condiciones nocturnas |
| 2 baterías de 10 000 mAh | Alimentación del prototipo |
| Cables | Interconexión eléctrica |
| Tampox | Absorción de humedad dentro de la carcasa |

### 4.2 Componentes mecánicos

- Carcasa del Prototipo 5.
- Soporte de cámara.
- Soporte de la placa de LED.
- Espacios internos para electrónica y batería.
- Elementos de fijación.

### 4.3 Vista general de los componentes

![Componentes principales](images/components-overview.png)

---

## 5. Herramientas necesarias

- Computadora.
- Cable USB.
- Módulo programador para ESP32-CAM.
- Cautín.
- Estaño.
- Flux, cuando sea necesario.
- Multímetro.
- Pinzas.
- Destornilladores y herramientas de montaje.

---

## 6. Reglas importantes antes de comenzar

> [!WARNING]
> **La batería se conecta de último.**
>
> La batería debe permanecer desconectada durante la programación, la soldadura y el montaje inicial.

Antes de energizar el sistema:

- Revisar todas las soldaduras.
- Comprobar polaridad.
- Verificar continuidad.
- Descartar cortocircuitos.
- Comprobar que los cables no estén atrapados o tensionados.

---

# 7. Proceso general

El armado completo debe seguir este orden:

```text
1. Preparar componentes
        ↓
2. Conectar cámara a ESP32-CAM
        ↓
3. Colocar ESP32-CAM en módulo programador
        ↓
4. Programar
        ↓
5. Probar ESP32-CAM
        ↓
6. Desconectar del módulo programador
        ↓
7. Soldar TODOS los componentes
        ↓
8. Revisar soldaduras
        ↓
9. Preparar la carcasa
        ↓
10. Introducir primero la placa de LED
        ↓
11. Introducir la cámara
        ↓
12. Colocar Tampox
        ↓
13. Conectar la batería
        ↓
14. Cerrar la carcasa
        ↓
15. Realizar pruebas finales
```

El orden físico más importante es:

**LED → cámara → Tampox → batería.**

---

# 8. Etapa 1 – Preparación de la ESP32-CAM

## 8.1 Conectar la cámara

1. Tomar la ESP32-CAM.
2. Localizar el conector de la cámara.
3. Insertar cuidadosamente el cable flex.
4. Verificar que la orientación sea correcta.
5. Cerrar el seguro del conector.
6. Comprobar que la conexión quede firme.

![ESP32-CAM](images/hardware/esp32-cam.png)

![Cámara](images/hardware/camara.png)

> No forzar el cable flex ni el conector de la cámara.

---

## 8.2 Colocar la ESP32-CAM en el módulo programador

La ESP32-CAM se utiliza inicialmente conectada al módulo programador para cargar el firmware y realizar las primeras pruebas.

1. Alinear los pines de la ESP32-CAM con el módulo programador.
2. Insertar cuidadosamente la ESP32-CAM.
3. Conectar el módulo programador a la computadora mediante USB.
4. Identificar el puerto COM asignado por el sistema operativo.

En esta etapa la ESP32-CAM **todavía no se instala dentro de la carcasa**.

---

# 9. Programación con PlatformIO

PlatformIO se puede utilizar como entorno principal para compilar y cargar el programa de la ESP32-CAM.

## 9.1 Preparar el entorno

1. Instalar Visual Studio Code.
2. Instalar la extensión PlatformIO IDE.
3. Abrir Visual Studio Code.
4. Abrir el proyecto de Croc Alert.
5. Esperar a que se descarguen o reconozcan las dependencias del proyecto.

## 9.2 Estructura recomendada del proyecto

```text
CrocAlert/
├── src/
│   └── main.cpp
├── include/
├── lib/
├── platformio.ini
└── README.md
```

## 9.3 Revisar `platformio.ini`

El archivo `platformio.ini` define la plataforma, la placa y el framework utilizados por el proyecto.

Ejemplo general:

```ini
[env:esp32cam]
platform = espressif32
board = esp32cam
framework = arduino

monitor_speed = 115200
```

> La configuración debe coincidir con el modelo exacto de ESP32-CAM utilizado.

## 9.4 Compilar

Antes de cargar el código:

1. Abrir PlatformIO.
2. Ejecutar **Build**.
3. Esperar a que termine la compilación.
4. Confirmar que no existan errores.

## 9.5 Seleccionar el puerto

Conectar el módulo programador y seleccionar el puerto COM que corresponda a la ESP32-CAM.

Ejemplo:

```text
COM3
COM5
COM11
```

El número cambia según la computadora.

## 9.6 Cargar el programa

Ejecutar:

```text
PlatformIO → Upload
```

Esperar a que finalice la carga del firmware.

## 9.7 Verificar la carga

Confirmar que el proceso termine sin errores y que la ESP32-CAM pueda iniciar el programa.

---

# 10. Programación con Arduino IDE

Arduino IDE puede utilizarse como alternativa para programar la ESP32-CAM.

## 10.1 Instalar soporte para ESP32

1. Abrir Arduino IDE.
2. Ir a **Archivo → Preferencias**.
3. Agregar la URL correspondiente al gestor de tarjetas de ESP32.
4. Ir a **Herramientas → Placa → Gestor de tarjetas**.
5. Buscar e instalar el soporte para ESP32.

## 10.2 Seleccionar la placa

Seleccionar la tarjeta correspondiente al hardware utilizado.

En caso de utilizar una AI Thinker ESP32-CAM:

```text
AI Thinker ESP32-CAM
```

## 10.3 Seleccionar el puerto

Ir a:

```text
Herramientas → Puerto
```

Seleccionar el puerto COM de la ESP32-CAM.

## 10.4 Cargar el código

1. Abrir el código del proyecto.
2. Compilar.
3. Seleccionar **Upload**.
4. Esperar a que finalice la carga.
5. Probar la ESP32-CAM.

---

# 11. Prueba de la ESP32-CAM antes del montaje

Antes de continuar con el proceso de soldadura, comprobar:

- La ESP32-CAM enciende.
- El programa inicia correctamente.
- La cámara responde.
- La captura de imágenes funciona.
- No existen reinicios inesperados.

> **No continuar con el ensamblaje permanente hasta completar esta prueba.**

---

# 12. Etapa 2 – Desconectar del módulo programador

Cuando la programación y las pruebas hayan finalizado:

1. Desconectar el cable USB.
2. Retirar cuidadosamente la ESP32-CAM del módulo programador.
3. Colocar la ESP32-CAM en una superficie segura.
4. Mantener la batería desconectada.
5. Preparar los componentes para la soldadura permanente.

A partir de aquí comienza el ensamblaje definitivo del dispositivo.

---

# 13. Etapa 3 – Soldadura de todos los componentes

## Regla principal

> **TODAS las conexiones deben quedar preparadas y soldadas antes de introducir los componentes dentro de la carcasa.**

La soldadura no debe realizarse con la batería conectada.

El objetivo de esta etapa es dejar listo el sistema electrónico completo antes de comenzar el montaje físico.

---

# 14. Placa de LED infrarrojos

La placa de iluminación está diseñada para integrar **24 LED infrarrojos de 850 nm**.

![Placa de LED infrarrojos](images/hardware/placa-led.png)

## 14.1 Preparación

1. Colocar la placa sobre una superficie estable.
2. Identificar sus puntos de conexión.
3. Preparar los cables de acuerdo con el esquema del proyecto.
4. Pelar los extremos.
5. Estañar los cables.

## 14.2 Soldadura

1. Colocar cada cable en su punto correspondiente.
2. Realizar la soldadura.
3. Dejar enfriar la unión.
4. Revisar visualmente.
5. Repetir para las conexiones restantes.

## 14.3 Verificación

Comprobar:

- Soldaduras firmes.
- Ausencia de puentes de estaño.
- Continuidad.
- Polaridad correcta.

> Los puntos exactos de conexión deben coincidir con el esquema eléctrico utilizado por el proyecto. No asumir conexiones únicamente por la apariencia de la placa.

---

# 15. Placa CRCibernetica

La placa CRCibernetica forma parte del sistema de control e integración electrónica.

En el Prototipo 5 se utiliza una versión modificada de la placa temporizadora para incorporar una interfaz lógica que permite controlar la iluminación desde la ESP32.

![Placa CRCibernetica](images/hardware/placa-crcibernetica.png)

## 15.1 Identificación visual

La placa muestra puntos y conectores identificados para señales y alimentación. Entre las etiquetas visibles se encuentran:

- `GND`
- `Vcc`
- `GPIO13`
- `GPIO3`
- `GPIO1`
- `LED Enable`
- `Trim Enable`

## 15.2 Preparación

1. Identificar las conexiones necesarias según el esquema eléctrico.
2. Preparar los cables.
3. Pelar los extremos.
4. Estañar los cables.

## 15.3 Soldadura

1. Colocar cada cable en el punto correspondiente.
2. Soldar.
3. Esperar a que la unión se enfríe.
4. Inspeccionar la soldadura.
5. Repetir para cada conexión necesaria.

## 15.4 Verificación

Comprobar:

- Continuidad.
- Polaridad.
- Soldaduras firmes.
- Ausencia de cortocircuitos.

> Las conexiones definitivas deben seguir el esquema eléctrico del prototipo y no deben inferirse únicamente a partir de las etiquetas visibles.

---

# 16. Conexiones entre los módulos

Una vez preparadas las placas, se realizan las conexiones entre ellas.

```text
ESP32-CAM
    │
    │ señal / control
    ▼
CRCibernetica
    │
    │ control de iluminación
    ▼
Placa de LED IR
```

## Procedimiento para cada cable

1. Medir la longitud requerida.
2. Cortar el cable.
3. Pelar el extremo.
4. Estañar.
5. Identificar el punto de conexión.
6. Soldar.
7. Dejar enfriar.
8. Revisar la unión.

---

# 17. Inspección electrónica antes del montaje

Antes de introducir cualquier componente en la carcasa:

### Inspección visual

- Revisar todas las soldaduras.
- Buscar puentes de estaño.
- Buscar cables sueltos.
- Confirmar que no haya componentes dañados.

### Prueba eléctrica

Con un multímetro comprobar:

- Continuidad.
- Polaridad.
- Posibles cortocircuitos.
- Integridad de las conexiones.

> **La batería continúa desconectada.**

---

# 18. Etapa 4 – Preparación de la carcasa

Antes de instalar los componentes:

1. Limpiar el interior de la carcasa.
2. Eliminar residuos de fabricación.
3. Revisar los soportes internos.
4. Identificar la posición de la placa de LED.
5. Identificar la posición de la cámara.
6. Identificar el espacio destinado para la alimentación.

![Modelo de carcasa](images/assembly/case-modelo.png)

---

# 19. Orden de instalación dentro del case

El orden físico obligatorio es:

```text
1. PLACA DE LED
        ↓
2. CÁMARA
        ↓
3. TAMPOX
        ↓
4. BATERÍA
```

No cambiar este orden durante el ensamblaje.

---

# 20. Instalación de la placa de LED

La **placa de LED se instala primero** dentro de la carcasa.

## Procedimiento

1. Tomar la placa LED ya soldada.
2. Revisar sus conexiones.
3. Introducirla cuidadosamente en la carcasa.
4. Colocarla en su posición de montaje.
5. Asegurar que los LED queden orientados hacia el exterior.
6. Acomodar el cableado.
7. Confirmar que la placa no interfiera con la posición de la cámara.

![Placa LED instalada](images/assembly/montaje-led.jpg)

---

# 21. Instalación de la cámara

Después de instalar la placa LED, se instala la cámara.

## Procedimiento

1. Tomar la ESP32-CAM ya programada y previamente soldada.
2. Introducirla en la posición correspondiente.
3. Colocar la cámara en su soporte.
4. Orientar el lente hacia el exterior.
5. Verificar que quede centrado.
6. Acomodar el cableado sin forzar el cable flex.
7. Confirmar que la placa de LED no bloquee el campo de visión.

![ESP32-CAM](images/hardware/esp32-cam.png)

---

# 22. Vista interna del ensamblaje

La fotografía interna final debe utilizarse como referencia para comprobar la distribución de todos los componentes antes del cierre.

> **Agregar aquí la fotografía real del interior completamente ensamblado.**
>
> Archivo recomendado: `images/assembly/interior-prototipo5.jpg`

La imagen debe mostrar, en la medida de lo posible:

- Placa de LED.
- Cámara.
- ESP32-CAM.
- Placa CRCibernetica.
- Cableado.
- Espacio de batería.
- Ubicación del Tampox.

---

# 23. Instalación del Tampox

Antes de cerrar la carcasa se coloca el material absorbente de humedad.

El **Tampox** se utilizará dentro del case para ayudar a absorber la humedad acumulada en el interior.

## Procedimiento

1. Preparar el material absorbente.
2. Identificar un espacio libre dentro de la carcasa.
3. Colocar el Tampox en esa zona.
4. Evitar que presione los componentes.
5. Evitar que bloquee la cámara.
6. Evitar que bloquee los LED.
7. Evitar que interfiera con los cables.
8. Comprobar que el case pueda cerrarse correctamente.

> La posición exacta debe mantenerse de forma que no interfiera con ningún componente ni con el cierre de la carcasa.

---

# 24. Conexión de la batería

## ⚠️ LA BATERÍA SE CONECTA DE ÚLTIMO

Este es el último paso eléctrico del montaje.

Antes de conectar la batería deben cumplirse todas estas condiciones:

- Todas las soldaduras están terminadas.
- Todas las conexiones fueron revisadas.
- La placa LED está instalada.
- La cámara está instalada.
- El Tampox está colocado.
- No existen cortocircuitos.
- La polaridad fue comprobada.

## Procedimiento

1. Colocar la batería en su posición.
2. Acomodar el cableado.
3. Revisar nuevamente la polaridad.
4. Conectar la batería.
5. Confirmar el encendido del sistema.

![Batería](images/hardware/bateria-10000mah.png)

---

# 25. Organización interna final

Antes de cerrar la carcasa verificar:

### Placa LED

- Posición correcta.
- Orientación correcta.
- Cables organizados.

### Cámara

- Lente centrado.
- Sin obstrucciones.
- Cable flex protegido.

### CRCibernetica

- Posicionada correctamente.
- Sin cables sueltos.

### Tampox

- Correctamente ubicado.
- Sin interferencia con los componentes.

### Baterías

- Correctamente ubicadas.
- Conectadas correctamente.
- Sin presión sobre otros componentes.

---

# 26. Cierre de la carcasa

Antes de cerrar:

```text
[ ] LED instalados
[ ] Cámara instalada
[ ] ESP32-CAM instalada
[ ] CRCibernetica instalada
[ ] Todas las soldaduras verificadas
[ ] Cableado organizado
[ ] Tampox colocado
[ ] Batería conectada
[ ] No hay cables atrapados
[ ] No hay componentes sueltos
```

Una vez completada la lista:

1. Colocar la tapa.
2. Alinear las piezas.
3. Cerrar la carcasa.
4. Instalar los elementos de fijación.
5. Revisar visualmente el cierre.

---

# 27. Resultado final

La siguiente fotografía debe mostrar el Prototipo 5 completamente ensamblado.

![Prototipo 5](images/assembly/prototipo-frontal.png)

> Esta imagen se utiliza como referencia visual del resultado final del ensamblaje.

---

# 28. Pruebas finales

## 28.1 Prueba de encendido

Comprobar:

- Encendido del sistema.
- Inicio de la ESP32-CAM.
- Ausencia de reinicios inesperados.

## 28.2 Prueba de cámara

Comprobar:

- Captura de imágenes.
- Orientación.
- Calidad de imagen.

## 28.3 Prueba de iluminación infrarroja

Realizar pruebas con:

- Luz ambiental.
- Poca iluminación.
- Oscuridad.

Verificar que los LED infrarrojos se activen correctamente y que la cámara pueda capturar la escena.

---

# 29. Evidencia de pruebas

## Prueba nocturna

![Prueba nocturna 1](images/tests/prueba-nocturna-1.png)

![Prueba nocturna 2](images/tests/prueba-nocturna-2.png)

## Prueba diurna

![Prueba diurna 1](images/tests/prueba-diurna-1.png)

![Prueba diurna 2](images/tests/prueba-diurna-2.png)

La documentación del proyecto registra pruebas del prototipo a diferentes distancias, incluyendo 3 m y 7 m.

---

# 30. Solución de problemas

## La ESP32-CAM no programa

### Revisar

- Puerto COM.
- Cable USB.
- Módulo programador.
- Placa seleccionada.
- Configuración del proyecto.

## La cámara no funciona

### Revisar

- Cable flex.
- Orientación del cable.
- Conector de la cámara.
- Configuración de cámara en el código.

## Los LED no encienden

### Revisar

- Soldaduras.
- Polaridad.
- Alimentación.
- Señal de control.
- Conexiones entre placas.

## El sistema no enciende

### Revisar

- Baterías.
- Polaridad.
- Soldaduras.
- Continuidad.
- Posibles cortocircuitos.

## La carcasa no cierra correctamente

### Revisar

- Posición de la batería.
- Organización del cableado.
- Posición de la placa LED.
- Posición de la cámara.
- Ubicación del Tampox.

---

# 31. Checklist de armado

## Programación

- [ ] Cámara conectada a la ESP32-CAM.
- [ ] ESP32-CAM colocada en el módulo programador.
- [ ] Proyecto configurado.
- [ ] Código compilado.
- [ ] Código cargado.
- [ ] Cámara probada.
- [ ] ESP32-CAM retirada del programador.

## Soldadura

- [ ] Placa LED preparada.
- [ ] CRCibernetica preparada.
- [ ] Cables preparados.
- [ ] Todas las conexiones soldadas.
- [ ] Soldaduras revisadas.
- [ ] Continuidad comprobada.
- [ ] Polaridad comprobada.
- [ ] Cortocircuitos descartados.

## Montaje

- [ ] Placa LED instalada primero.
- [ ] Cámara instalada después.
- [ ] Tampox colocado.
- [ ] Batería instalada.
- [ ] Batería conectada de último.
- [ ] Cableado organizado.
- [ ] Carcasa cerrada.

## Pruebas

- [ ] Encendido.
- [ ] Cámara.
- [ ] Captura de imágenes.
- [ ] LED infrarrojos.
- [ ] Prueba con luz.
- [ ] Prueba con poca iluminación.
- [ ] Prueba nocturna.
- [ ] Prueba final completa.

---

# 32. Estructura recomendada del repositorio

```text
Croc-Alert/
│
├── README.md
│
├── code/
│   ├── platformio/
│   │   ├── platformio.ini
│   │   ├── src/
│   │   │   └── main.cpp
│   │   ├── include/
│   │   └── lib/
│   │
│   └── arduino/
│       └── CrocAlert.ino
│
├── images/
│   ├── components-overview.png
│   │
│   ├── hardware/
│   │   ├── placa-led.png
│   │   ├── placa-crcibernetica.png
│   │   ├── esp32-cam.png
│   │   ├── camara.png
│   │   └── bateria-10000mah.png
│   │
│   ├── assembly/
│   │   ├── case-modelo.png
│   │   ├── montaje-led.jpg
│   │   ├── interior-prototipo5.jpg
│   │   └── prototipo-frontal.png
│   │
│   └── tests/
│       ├── prueba-nocturna-1.png
│       ├── prueba-nocturna-2.png
│       ├── prueba-diurna-1.png
│       └── prueba-diurna-2.png
│
└── docs/
```

---

# 33. Galería

## Componentes

![Componentes](images/components-overview.png)

## Placa LED

![Placa LED](images/hardware/placa-led.png)

## Placa CRCibernetica

![CRCibernetica](images/hardware/placa-crcibernetica.png)

## ESP32-CAM

![ESP32-CAM](images/hardware/esp32-cam.png)

## Cámara

![Cámara](images/hardware/camara.png)

## Carcasa

![Carcasa](images/assembly/case-modelo.png)

## Prototipo ensamblado

![Prototipo](images/assembly/prototipo-frontal.png)

---

# 34. Equipo de trabajo

**Universidad CENFOTEC**

- Jorge Ortega Badilla
- Fiorella Pérez
- Pablo Hernandez
- Danny Arias
- Gabriela Urbina

---

# 35. Nota de mantenimiento y documentación

Cuando se modifique el hardware o el software del Prototipo 5, actualizar:

- El código en `code/`.
- La descripción de conexiones.
- Las fotografías del ensamblaje.
- La lista de materiales.
- El procedimiento de programación.
- Las pruebas realizadas.

Esto permite que futuras personas puedan reproducir y mantener el prototipo sin depender únicamente de información oral o de la presentación del proyecto.
