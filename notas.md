Manuel Martínez

Teams: https://teams.microsoft.com/meet/3446454090472870?p=vooz07mWCz7pEqR2OE

AZ-900
AI-901
AI-103
AI-200

![[Pasted image 20261105094620.png]]


DEV-Seg-Ops

campus.pue.es

## 10/11/2026

### Agentes IA

En local, siempre hay que meterlos dentro de una máquina virtual únicamente con acceso a internet para evitar que toque cosas de propio sistema local.

- Hermes Agent
- OpenClaw
- GitHub Copilot (para desarrollo de codigo)

Se suelen utilizar MCPs para ayudar a los agentes sepan donde tienen que acceder las APIs

En producción no se usan MCPs (Protocolo de contexto de modelo) para evitar gastos en IA. Si generas plantillas con el proceso que se puedan conectar con el CLI correspondiente, la ejecución no usaría IA para nada.

---

https://chat.webllm.ai/#/settings

LLM totalmente en navegador.

Le cargas en el navegador del cliente y así no gasta tokens propios.

---

Los MCPs (Protocolo de contexto de modelo) nos permite exponer las tecnologías que tenemos (APIs, BDs...) para que las IAs las consuma.

Es una manera de controlar y aislar las IAs de tu sistema.

---

examprepper.com

## 12/11/2026

## 16/11/2026

## 17/11/2026

Para modelos en local:
- LM Studio
- Ollama
- LlamaCPT


Baterías de preguntas baratas: examprepper.com

examtopics.com: más caro pero actualizadas

## 18/11/2026

Biceps es más o menos como Terraform.

Labs hasta LP04M3

## 19/11/2026

Se puede montar una federación entre los tenants de Google y EntraID

### Ejercicio práctico

**Guardar en BD PosgreSQL la temperatura de varias zonas de Barcelona. Habrá una caché que guarde los valores enteros y se modifica cuando el entero cambia. Los usuarios acceden a la información de la caché.**

- Linux+ VLLM + Qwen 3.8 flash next
- conectado con OpenAI API
- se conecta copilot de VS2026 mediante la API 

Hardware: ESP32 + DTT

ORM: se conecta a la BD y la replica en entidades
DAO: es quien hace las operaciones sobre BD

## 23/11/2026

- Instalar Azure functions en VS
- En Azure Tools viene el pack de todo lo que hace falta

Buscar en Google: configuración en VSC para Azure para saber como hacerlo.

Cuando se guarda una plantilla, después se carga con "deploy a custom template" buscando en la barra de azure.

averiguar como exportar plantillas.

Si se guardan plantillas parciales de los recursos luego hay problemas de dependencias.

## 24/11/2026

Faltan apuntes
## 26/11/2026

