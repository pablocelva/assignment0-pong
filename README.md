# Pong (CS50 Games - Assignment 0)

Remake en 2D del clásico videojuego Pong de Atari (1972), desarrollado para el curso CS50's Introduction to Game Development. Incluye soporte para dos jugadores locales y modo individual contra la computadora (IA básica).

## Stack Tecnológico

- **Lenguaje:** Lua (compatible con LuaJIT / Lua 5.1+)
- **Motor:** LÖVE (Love2D) 0.10.x / 11.x
- **Librerías:**
  - `push.lua`: Gestión de resolución virtual fija (432x243) con renderizado escalado a pantalla completa o ventana redimensionable con aspecto 16:9.
  - `class.lua`: Sistema de clases y herencia en Lua basado en metatablas (librería HUMP).

## Arquitectura

El proyecto está estructurado alrededor de los ciclos de ejecución del motor LÖVE y la separación de entidades:

- `main.lua`: Punto de entrada principal. Configura gráficos, fuentes y sonidos; administra la máquina de estados; captura entradas de teclado y delega la actualización y renderizado.
- `Paddle.lua`: Clase que encapsula dimensiones, posición, velocidad vertical y renderizado de las paletas de juego.
- `Ball.lua`: Clase que modela la bola, sus componentes de velocidad vectorial (dx, dy), lógica de rebote y algoritmo de detección de colisiones.
- `push.lua`: Capa intermedia gráfica que proyecta la resolución interna retro con filtrado de píxeles nearest-neighbor.
- `sounds/`: Efectos de audio en formato WAV (`paddle_hit`, `score`, `wall_hit`).
- `font.ttf`: Tipografía de estilo retro cargada a diferentes escalas de tamaño.

## Patrones de Diseño

- **Game Loop:** Ciclo clásico desacoplado por tiempo delta (`dt`) a través de las funciones `love.load`, `love.update(dt)` y `love.draw`. Permite movimiento independiente del framerate del hardware.
- **Máquina de Estados Finita (FSM):** Flujo de control del juego centralizado en cuatro estados discretos:
  - `start`: Menú inicial de bienvenida y selección de modo (vs Jugador o vs IA).
  - `serve`: Preparación del saque determinando el vector inicial de la bola según el jugador que recibió el último punto.
  - `play`: Partida en curso con cálculo de físicas, rebotes contra muros/paletas y cómputo de puntos.
  - `done`: Fin de partida cuando un jugador alcanza 10 puntos, con opción de reinicio.
- **Programación Orientada a Objetos (OOP):** Modularización en clases (`Ball`, `Paddle`) mediante el módulo `class.lua`, implementando constructores (`init`), métodos de actualización de estado (`update`) y métodos de dibujo (`render`).
- **AABB (Axis-Aligned Bounding Box):** Algoritmo de detección de colisiones basado en cajas alineadas con los ejes para resolver el contacto entre la bola y las paletas en dos dimensiones.

## Controles

- **Jugador 1:** `W` (arriba) / `S` (abajo)
- **Jugador 2:** `Flecha Arriba` / `Flecha Abajo`
- **Modo vs IA:** Presionar `A` en la pantalla de inicio (`start`).
- **Confirmar / Servir / Reiniciar:** `Enter` o `Return`
- **Salir:** `Escape`

## Ejecución del Proyecto

### Opción 1: Arrastrar la carpeta al ejecutable (Windows)

1. Abrir la carpeta contenedora de este proyecto en el Explorador de archivos de Windows.
2. Arrastrar la carpeta raíz del proyecto (la carpeta que contiene `main.lua`) y soltarla directamente sobre el archivo ejecutable `love.exe` (o su acceso directo).

### Opción 2: Linux

Abrir una terminal en el directorio raíz del proyecto y ejecutar:

```bash
love .
```

O pasando la ruta absoluta del directorio:

```bash
love /ruta/al/proyecto
```

### Opción 3: Bash (macOS / Git Bash / WSL)

Desde la raíz del proyecto con LÖVE en el `PATH`:

```bash
love .
```

Si se ejecuta desde Git Bash en Windows con la ruta por defecto:

```bash
"/c/Program Files/LOVE/love.exe" .
```

En macOS mediante la aplicación instalada:

```bash
open -n -a love "--args" .
```

### Opción 4: CMD (Símbolo del sistema de Windows)

Si LÖVE está registrado en el `PATH`:

```cmd
love .
```

Si no está en el `PATH`, usar la ruta absoluta del ejecutable:

```cmd
"C:\Program Files\LOVE\love.exe" .
```

### Opción 5: PowerShell

Si LÖVE está registrado en el `PATH`:

```powershell
love .
```

Si no está en el `PATH`, invocar el ejecutable mediante el operador `&`:

```powershell
& "C:\Program Files\LOVE\love.exe" .
```

