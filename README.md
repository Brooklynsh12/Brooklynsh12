import pygame
import random

# --- Constantes ---
ANCHO_PANTALLA = 800
ALTO_PANTALLA = 600
COLOR_FONDO = (255, 255, 255)  # Blanco

# --- Clases ---
class Caballo:
    def __init__(self, nombre, velocidad, resistencia, salto):
        self.nombre = nombre
        self.velocidad = velocidad
        self.resistencia = resistencia
        self.salto = salto
        self.estado = "Saludable"  # Puede ser "Cansado", "Herido", etc.

class Jugador:
    def __init__(self, nombre, dinero=0, experiencia=0):
        self.nombre = nombre
        self.dinero = dinero
        self.experiencia = experiencia
        self.inventario = {}  # Diccionario para guardar objetos
        self.caballo = None  # El caballo actual del jugador
        self.posicion_jugador = (1, 1)  # Posición inicial en el mapa

# --- Mapa ---
mapa = [
    ["Bosque", "Bosque", "Rio", "Pueblo"], 
    ["Bosque", "Montana", "Montana", "Bosque"],
    ["Bosque", "Bosque", "Rio", "Bosque"],
]

# --- Funciones ---
def mover_arriba(jugador):
    nueva_fila = jugador.posicion_jugador[1] - 1
    if nueva_fila >= 0:
        jugador.posicion_jugador = (jugador.posicion_jugador[0], nueva_fila)

def mover_abajo(jugador):
    nueva_fila = jugador.posicion_jugador[1] + 1
    if nueva_fila < len(mapa):
        jugador.posicion_jugador = (jugador.posicion_jugador[0], nueva_fila)

def mover_derecha(jugador):
    nueva_columna = jugador.posicion_jugador[0] + 1
    if nueva_columna < len(mapa[0]):
        jugador.posicion_jugador = (nueva_columna, jugador.posicion_jugador[1])

def mover_izquierda(jugador):
    nueva_columna = jugador.posicion_jugador[0] - 1
    if nueva_columna >= 0:
        jugador.posicion_jugador = (nueva_columna, jugador.posicion_jugador[1])

def encuentro_animal(jugador, tipo_animal):
    print(f"¡Has encontrado un {tipo_animal}!")
    # ... lógica para el encuentro (ataque, huida, etc.)

def actualizar_pantalla(jugador):
    pantalla.fill(COLOR_FONDO)  # Limpia la pantalla
    # ... código para dibujar el jugador, el caballo, el mapa, etc.

# --- Inicialización ---
pygame.init()
pantalla = pygame.display.set_mode((ANCHO_PANTALLA, ALTO_PANTALLA))
pygame.display.set_caption("Aventura de Caballos")

# --- Creación de objetos ---
jugador = Jugador("Juan")
caballo = Caballo("Relámpago", 10, 8, 5)
jugador.caballo = caballo

# --- Bucle principal del juego ---
en_ejecucion = True
while en_ejecucion:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            en_ejecucion = False
        if evento.type == pygame.KEYDOWN:
            if evento.key == pygame.K_UP:
                mover_arriba(jugador)
            elif evento.key == pygame.K_DOWN:
                mover_abajo(jugador)
            elif evento.key == pygame.K_RIGHT:
                mover_derecha(jugador)
            elif evento.key == pygame.K_LEFT:
                mover_izquierda(jugador)

    # --- Lógica del juego ---
    tipo_casilla = mapa[jugador.posicion_jugador[1]][jugador.posicion_jugador[0]]
    if random.random() < 0.1:  # 10% de probabilidad de encuentro con animal
        if tipo_casilla == "Bosque":
            encuentro_animal(jugador, random.choice(["Venado", "Zorro", "Oso"]))
        elif tipo_casilla == "Rio":
            encuentro_animal(jugador, "Pez")

    # --- Actualización de la pantalla ---
    actualizar_pantalla(jugador)
    pygame.display.flip()  # Actualiza la pantalla

# --- Salida del juego ---
pygame.quit()
