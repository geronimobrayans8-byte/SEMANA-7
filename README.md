# SEMANA-7

import numpy as np
import pandas as pd

# Valores iniciales
x0 = 2.0
h_values = [0.5, 0.1, 0.05, 0.01, 0.005, 0.001]

# Valor exacto: Si f(x) = e^x, entonces f'(2) = e^2 y f''(2) = e^2
valor_exacto = np.exp(x0)

# Listas para guardar los resultados y llenar la tabla después
f_prima_6_list = []
f_biprima_8_list = []
error_pct_6_list = []
error_pct_8_list = []

for h in h_values:
    # Evaluamos la función e^x limitando a 6 decimales (para la primera derivada)
    f_mas_6 = round(np.exp(x0 + h), 6)
    f_menos_6 = round(np.exp(x0 - h), 6)
    
    # Evaluamos la función e^x limitando a 8 decimales (para la segunda derivada)
    f_mas_8 = round(np.exp(x0 + h), 8)
    f_x0_8 = round(np.exp(x0), 8)
    f_menos_8 = round(np.exp(x0 - h), 8)
    
    # 1. Aproximación de f'(2) a 6 decimales (Diferencia Central)
    # f'(x) ≈ (f(x+h) - f(x-h)) / 2h
    f_prima_6 = (f_mas_6 - f_menos_6) / (2 * h)
    
    # 2. Aproximación de f''(2) a 8 decimales (Diferencia Central)
    # f''(x) ≈ (f(x+h) - 2f(x) + f(x-h)) / h^2
    f_biprima_8 = (f_mas_8 - 2 * f_x0_8 + f_menos_8) / (h**2)
    
    # 3. Cálculo de errores porcentuales
    error_pct_6 = abs((f_prima_6 - valor_exacto) / valor_exacto) * 100
    error_pct_8 = abs((f_biprima_8 - valor_exacto) / valor_exacto) * 100
    
    # Guardamos los resultados redondeando la salida final para mayor claridad (opcional)
    f_prima_6_list.append(round(f_prima_6, 6))
    f_biprima_8_list.append(round(f_biprima_8, 8))
    error_pct_6_list.append(error_pct_6)
    error_pct_8_list.append(error_pct_8)

# --- Creación de la Tabla con Pandas ---
tabla = pd.DataFrame({
    'h': h_values,
    "f'(2) 6decimales": f_prima_6_list,
    "f''(2) 8decimales": f_biprima_8_list,
    "error % 6 decimales": error_pct_6_list,
    "error % 8 decimales": error_pct_8_list
})

# Mostrar la tabla formateada
print("TABLA DE RESULTADOS Y ERRORES PORCENTUALES:")
print("-" * 80)
print(tabla.to_string(index=False))
