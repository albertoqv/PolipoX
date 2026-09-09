# PolipoX

Proyecto de equipo — Hackathon IMIBIC 2024, en colaboración con el Hospital Universitario Reina Sofía de Córdoba.

Durante una colonoscopia, el médico detecta y describe los pólipos en voz alta, pero tiene que parar la exploración para anotarlo todo a mano después. PolipoX es un asistente de voz que permite registrar esas características —localización, tamaño, clasificación clínica (NICE, JNET, Paris)— sin soltar el endoscopio ni mirar una pantalla.

<p align="center">
  <img src="docs/hardware.png" alt="Hardware: dispositivo, Raspberry Pi y auricular" width="700">
</p>

## Cómo funciona

<p align="center">
  <img src="docs/arquitectura.png" alt="Arquitectura del sistema" width="700">
</p>

- **Dispositivo** — un Raspberry Pi con un auricular Bluetooth, alojado en una carcasa propia, captura la voz del clínico durante la exploración.
- **Captura de audio** (`utils.py`, `main.py`) — arquitectura productor-consumidor con `multiprocessing`: un proceso graba audio en continuo con PyAudio y lo encola; otro lo procesa según va llegando.
- **Transcripción** — el audio se envía por HTTP a un servidor (EC2) que ejecuta Faster-Whisper, el modelo de reconocimiento de voz de código abierto de OpenAI.
- **Interpretación de comandos** (`states.py`) — una máquina de estados interpreta la transcripción y va guiando al clínico por los distintos campos clínicos a rellenar por voz.
- **Plataforma web** — los datos recogidos se envían a una web (Vercel + PostgreSQL) donde el equipo clínico puede consultar los registros de cada exploración.

<p align="center">
  <img src="docs/software.png" alt="Plataforma web de consulta de registros" width="700">
</p>

## Mi parte en el proyecto

Fue un proyecto de equipo desarrollado en el formato de tiempo limitado propio de un hackathon, y como suele pasar en ese formato, todos acabamos metiendo mano en todas las partes. Participé en el conjunto del proyecto — desde el montaje y las pruebas del dispositivo físico hasta la integración con la plataforma web —, aunque mi foco principal estuvo en el software del propio dispositivo: la captura y el procesamiento de audio en Python (la arquitectura productor-consumidor con `multiprocessing`) y la máquina de estados que interpreta los comandos de voz y guía al clínico por los distintos campos a rellenar.

## Stack

![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/-Raspberry%20Pi-A22846?style=flat-square&logo=raspberrypi&logoColor=white)
![PyAudio](https://img.shields.io/badge/-PyAudio-306998?style=flat-square)
![Faster--Whisper](https://img.shields.io/badge/-Faster--Whisper-6A4C93?style=flat-square)
![Tkinter](https://img.shields.io/badge/-Tkinter-3776AB?style=flat-square)
![pyttsx3](https://img.shields.io/badge/-pyttsx3-777777?style=flat-square)
![Vercel](https://img.shields.io/badge/-Vercel-000000?style=flat-square&logo=vercel&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)

## Nota sobre esta versión pública

La URL del servidor de transcripción se ha sustituido por una variable de entorno (`TRANSCRIPTION_SERVER_URL`) y no se incluye ningún endpoint real, ya que el servidor del hackathon ya no está activo.
