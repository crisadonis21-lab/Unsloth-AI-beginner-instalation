# Unsloth-AI-beginner-instalation
"Para usar el entorno en VS Code Jupyter Notebooks, es indispensable instalar ipykernel dentro del entorno de Conda para que sea visible como Kernel seleccionable".

🛡️ El "Blindaje" (Solución a Errores de Atributos)

Este es el Pro-Tip de nuestra batalla. WSL2 a veces no registra tipos de datos experimentales de bits (int1, int3, etc.). Creamos un parche externo que Python lee automáticamente.

El Parche sitecustomize.py

Crea este archivo en la ruta de tus librerías para emular los bits faltantes y evitar el KeyError de Reinforcement Learning:

Python

# Ubicación: ~/miniconda3/envs/unsloth_env/lib/python3.11/site-packages/sitecustomize.py
import torch
import unsloth

# 1. Emular tipos de bits (int1 al int7) para evitar AttributeError

for i in range(1, 8):
    attr = f"int{i}"
    if not hasattr(torch, attr):
        setattr(torch, attr, torch.int8)

# 2. Corregir KeyError: 'sanitize_logprob' en módulos de RL

if hasattr(unsloth, "models") and hasattr(unsloth.models, "rl"):
    if not hasattr(unsloth.models.rl, "RL_REPLACEMENTS"):
        unsloth.models.rl.RL_REPLACEMENTS = {}
    if "sanitize_logprob" not in unsloth.models.rl.RL_REPLACEMENTS:
        unsloth.models.rl.RL_REPLACEMENTS["sanitize_logprob"] = None

✅ Verificación Final

Para confirmar que la RTX 3060 está lista para el ahorro de memoria de Unsloth, ejecuta:

Bash
python -c "import torch; from unsloth import FastLanguageModel; print(f'✅ GPU: {torch.cuda.get_device_name(0)}'); print('SISTEMA TOTALMENTE BLINDADO')"
