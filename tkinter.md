import tkinter as tk
import psutil
import time
from threading import Thread

def actualizar_datos():
    """Actualizar cada 2 seg"""
    while True:
        # Obtener métricas del sistema
        uso_cpu = psutil.cpu_percent(interval=1)
        memoria = psutil.virtual_memory()
        disco = psutil.disk_usage('/')

        # Actualizar etiquetas en la GUI
        lbl_cpu.config(text=f"Uso CPU: {uso_cpu}%")
        lbl_memoria.config(text=f"Memoria: {memoria.percent}% usada ({memoria.used // (1024**3)} GB de {memoria.total // (1024**3)} GB)")
        lbl_disco.config(text=f"Disco: {disco.percent}% usado")

        time.sleep(2)

# Crear ventana principal
ventana = tk.Tk()
ventana.title("Monitor de Recursos del Sistema")
ventana.geometry("400x200")

# Etiquetas para mostrar métricas
lbl_cpu = tk.Label(ventana, text="Uso CPU: --%", font=("Arial", 12))
lbl_cpu.pack(pady=5)

lbl_memoria = tk.Label(ventana, text="Memoria: --%", font=("Arial", 12))
lbl_memoria.pack(pady=5)

lbl_disco = tk.Label(ventana, text="Disco: --%", font=("Arial", 12))
lbl_disco.pack(pady=5)

# Ejecutar la actualización en un hilo para no bloquear la interfaz
t = Thread(target=actualizar_datos, daemon=True)
t.start()

# Iniciar loop de Tkinter
ventana.mainloop()
