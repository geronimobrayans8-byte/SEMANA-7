# SEMANA-7
#EJERCICIO 1

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




#EJERCICIO 2

import numpy as np
import pandas as pd

# 1. Definir los datos de x
x_datos = np.array([1.5, 1.9, 2.1, 2.4, 2.6, 3.1])

# Asumimos que seguimos usando f(x) = e^x para rellenar la tabla.
# Si tu profesor te dio otros valores de f(x), simplemente reemplaza este array 
# con tus datos, por ejemplo: y_datos = np.array([4.48, 6.68, ...])
y_datos = np.exp(x_datos)

# Mostrar la tabla generada
tabla = pd.DataFrame({'x': x_datos, 'f(x)': y_datos})
print("TABLA DE DATOS GENERADA:")
print("-" * 35)
print(tabla.to_string(index=False))
print("\n")

# 2. El punto donde queremos calcular las derivadas
x_objetivo = 2.25

# 3. Encontrar los 3 puntos más cercanos a x = 2.25
# Calculamos la distancia de cada x al 2.25 y elegimos los 3 más cercanos
distancias = np.abs(x_datos - x_objetivo)
indices_cercanos = np.argsort(distancias)[:3]

# Extraemos esos 3 puntos (x y sus respectivos f(x))
x_cercanos = x_datos[indices_cercanos]
y_cercanos = y_datos[indices_cercanos]

print("PUNTOS SELECCIONADOS PARA INTERPOLACIÓN:")
print("-" * 35)
print(f"Valores x: {x_cercanos}")
print(f"Valores y: {y_cercanos}\n")

# 4. Ajustar un polinomio de grado 2 (parábola) a esos 3 puntos
# np.polyfit nos devuelve los coeficientes a, b y c de la ecuación ax^2 + bx + c
coeficientes = np.polyfit(x_cercanos, y_cercanos, 2)
a, b, c = coeficientes

# 5. Calcular las derivadas usando el polinomio ajustado
# Si P(x)  = ax^2 + bx + c
# Entonces P'(x) = 2ax + b
# Y P''(x) = 2a

f_prima_aprox = 2 * a * x_objetivo + b
f_biprima_aprox = 2 * a

print("RESULTADOS SOLICITADOS:")
print("-" * 35)
print(f"f'(2.25) aproximado  = {f_prima_aprox:.6f}")
print(f"f''(2.25) aproximado = {f_biprima_aprox:.6f}")

# Extra: Comparamos con el valor real para ver qué tan buena fue la aproximación
valor_real_exacto = np.exp(x_objetivo)
print(f"\n(Nota: Si f(x) = e^x, el valor analítico exacto de ambas derivadas en x=2.25 es {valor_real_exacto:.6f})")


