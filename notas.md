Manuel Martínez

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

