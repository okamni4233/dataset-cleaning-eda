# Limpieza y análisis exploratorio de ventas de una cafetería

> **English summary.** Data cleaning and exploratory analysis of a deliberately dirty café sales dataset (10,000 transactions, 2023). The cleaning notebook normalises columns, handles invalid markers ("ERROR", "UNKNOWN"), fixes types and dates, and rebuilds the transaction total. After cleaning, 9,540 transactions remain, with total revenue of 85,246.5 and an average ticket of 8.94. About a third of the transactions have no recorded payment method, which limits any conclusion about payment preferences.

## Qué hace

1. **`notebooks/01_data_cleaning.ipynb`** toma `data/dirty_cafe_sales.csv` y genera `data/cleaned_cafe_sales.csv`:
   - normaliza los nombres de columnas;
   - convierte a nulo los marcadores inválidos (`ERROR`, `UNKNOWN`);
   - convierte cantidades y precios a numérico, y las fechas (formato ISO `AAAA-MM-DD`) a `datetime`;
   - rellena las categorías faltantes con `Unknown`, y cantidad y precio faltantes con la mediana;
   - recalcula `total_spent = quantity × price_per_unit`;
   - descarta las transacciones sin fecha válida y agrega año, mes y día de la semana.
2. **`notebooks/02_eda.ipynb`** analiza el dataset limpio: ventas por producto, por mes, por método de pago y por ubicación.

## Resultados

Cifras obtenidas al ejecutar ambos notebooks de principio a fin:

| Indicador | Valor |
|---|---|
| Transacciones originales | 10.000 |
| Transacciones tras la limpieza | 9.540 (se descartan 460 sin fecha válida) |
| Ingreso total | 85.246,5 |
| Ticket medio | 8,94 (mediana 8,00; máximo 25,00) |

- **Productos.** Las unidades vendidas se reparten de forma pareja entre los ocho productos (entre 3.093 y 3.425 unidades cada uno), así que las diferencias de ingreso vienen del precio: Salad aporta el 19,0 % del ingreso, Sandwich el 15,1 % y Smoothie el 14,8 %. Coffee es el producto con más unidades, pero solo aporta el 8,2 %. Un 9,6 % del ingreso corresponde a transacciones sin producto identificado.
- **Meses.** No se observa estacionalidad: el ingreso mensual se mueve entre 6.673 (febrero) y 7.418 (junio), una diferencia de 11 % entre el mes más bajo y el más alto.
- **Métodos de pago.** No hay una preferencia clara: Digital Wallet 23,0 %, Credit Card 22,7 % y Cash 22,6 %. El 31,6 % de las transacciones no tiene método de pago registrado, más que cualquier método individual, por lo que no se puede concluir cuál es el más usado.
- **Ubicación.** In-store (30,1 %) y Takeaway (30,3 %) están parejos; el 39,6 % no tiene ubicación registrada.

## Limitaciones

- Cantidad y precio faltantes se imputan con la mediana y el total se recalcula a partir de ellos. Es una simplificación: cuando falta uno de los tres valores (cantidad, precio, total) se podría reconstruir de forma exacta con los otros dos, y el precio se podría deducir del producto. Queda como mejora pendiente.
- El origen del dataset no está documentado en este repositorio (pendiente).
- Los montos no tienen unidad monetaria declarada en el dataset.

## Cómo reproducirlo

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace notebooks/01_data_cleaning.ipynb
jupyter nbconvert --to notebook --execute --inplace notebooks/02_eda.ipynb
```

Probado con Python 3.13.

## Estructura

```
data/
  dirty_cafe_sales.csv      dataset original
  cleaned_cafe_sales.csv    salida del notebook 01
notebooks/
  01_data_cleaning.ipynb
  02_eda.ipynb
requirements.txt
```
