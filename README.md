# 🔐 Como o Hirunlocker funciona na prática
O Hirunlocker realiza ataque de força bruta para recuperar senhas de arquivos .7z, com escalonamento inteligente de tentativas.

Charset usado: abcdefghijklmnopqrstuvwxyz0123456789!@#$% (40 caracteres)
Tenta senhas de 1 até 10 caracteres
Suporte a até ProcessorCount * 8 threads, com fallback automático para a CPU
Taxa real de verificação: ~2.777 tentativas/segundo (com base em benchmark real)
Otimizado para velocidade e controle manual de carga

# ⏱️ Estimativa de tempo (exemplo com CPU):
Senha de 4 caracteres → ~15 minutos
Senha de 5 → ~10 horas
Senha de 6 → ~17 dias
Senha de 10 → ~119.000 anos (inviável sem GPU)

O Hirunlocker será adaptado para usar GPUs via OpenCL com fallback para CPU, mas uso de GPU só torna viável até ~7-8 caracteres

# ✅ Estimativa de tempo para todos tamanhos de senha em base ao Ryzen 7 7800X3D (8/16). 
Charset: 40 caracteres | 2777/s
A média por caracteres na senha de 1 a 10 caracteres:

| Tamanho | Combinações            | Tempo estimado (@ 2777/s) |
| ------: | ---------------------- | ------------------------- |
|       1 | 40                     | 0.014 segundos            |
|       2 | 1.600                  | 0.576 segundos            |
|       3 | 64.000                 | 23.05 segundos            |
|       4 | 2.560.000              | 15 min 21 s               |
|       5 | 102.400.000            | 10 h 15 min               |
|       6 | 4.096.000.000          | 17 dias                   |
|       7 | 163.840.000.000        | 684 dias (\~1.87 anos)    |
|       8 | 6.553.600.000.000      | 2.7 séculos (\~237 anos)  |
|       9 | 262.144.000.000.000    | \~3.0 mil anos            |
|      10 | 10.485.760.000.000.000 | \~119 mil anos            |


# ⏱️ TEMPO ESTIMADO PARA 10 CARACTERES (40¹⁰)
| GPU                     | Est. Desempenho Realista (tentativas/s) | Estimativa de Tempo (completo) |
| ----------------------- | --------------------------------------- | -----------------------------: |
| **RTX 3060**            | \~600 mil/s                             |                     \~202 anos |
| **RTX 4060**            | \~900 mil/s                             |                     \~134 anos |
| **RTX 5060** (previsto) | \~1,300 mil/s                           |                      \~96 anos |
| **RTX 4090**            | \~9.500 mil/s (\~9.5 milhões)           |                      \~34 anos |
| **RTX 5090** (previsto) | \~14 milhões/s                          |                      \~23 anos |
| **RX 5500 XT**          | \~300 mil/s                             |                     \~404 anos |
| **RX 6600 XT**          | \~800 mil/s                             |                     \~164 anos |
| **RX 7900 XTX**         | \~8.000 mil/s                           |                      \~41 anos |
| **RX 9070 XT** (prev.)  | \~11 milhões/s                          |                      \~30 anos |

# ⚡ Suporte a GPU:
Drivers atualizados com suporte a OpenCL 1.2 ou superior

# NVIDIA (OpenCL) – compatível com:
- GTX 10xx (1050, 1060, 1070, 1080)
- GTX 16xx (1650, 1660 Super, etc.)
- RTX 20xx (2060, 2070, 2080)
- RTX 30xx (3060, 3070, 3080, 3090)
- RTX 40xx (4060, 4070, 4080, 4090)
- RTX 50xx (5060, 5070, 5090) – estimado compatível
- Quadro e Tesla modernos

# AMD (OpenCL) – compatível com:
- RX 500 (550, 560, 570, 580)
- RX 5000 (5500 XT, 5600 XT, 5700 XT)
- RX 6000 (6600 XT, 6700 XT, 6800, 6900 XT)
- RX 7000 (7600, 7700 XT, 7900 XTX)
- RX 9000 (9070 XT) – estimado compatível
- Vega, Fury, Radeon VII

# Intel (OpenCL) – compatível com:
- Intel Iris Xe, UHD integradas modernas (desempenho limitado)
- Intel Arc A380, A750, A770

# 🟡 CUDA (via ManagedCUDA)
Somente NVIDIA com CUDA Compute Capability ≥ 3.0
Placas com driver atualizado e suporte a CUDA Toolkit 10–12
Compatível com CUDA:
- GTX 600 em diante (mínimo: Kepler)
- GTX 10xx, 16xx
- RTX 20xx, 30xx, 40xx, 50xx
- Tesla/Quadro com suporte a CUDA

# ⚠️ Modelos não suportados (ou problemáticos)
GPUs muito antigas / china: GTX 400, 500, AMD HD 6000/7000, RX 400, 500, etc.
GPUs com OpenCL 1.0/1.1 apenas (muito limitadas)
iGPUs da Intel anteriores a 2021 → suporte fraco
GPUs em ambientes com drivers genéricos (ex: Windows sem driver dedicado)

# 📌 Detecção Automática (modo Auto)
No modo “Auto”, o Hirunlocker:
Tenta detectar CUDA primeiro (NVIDIA)
Se falhar, tenta OpenCL (Todos modelos de GPU)
Se nenhuma GPU estiver disponível ou suportada → fallback para CPU
