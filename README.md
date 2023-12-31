# AdaBoost-
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import AdaBoostRegressor
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import r2_score, mean_absolute_error
import matplotlib.pyplot as plt
df = pd.read_csv("C:/Users/XE/Downloads/datasetage_kz.csv")
df = pd.get_dummies(df, columns=['Gender', 'Married'], prefix=['Gender', 'Married'])
X = df.drop(columns=['Age', 'Income'])
y = df['Income']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
model = AdaBoostRegressor(base_estimator=DecisionTreeRegressor(max_depth=6), n_estimators=50, learning_rate=0.05)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
r2 = r2_score(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
print(f'R-квадрат: {r2}')
print(f'Средняя абсолютная ошибка (MAE): {mae}')
# График фактического дохода vs. предсказанного дохода
plt.figure(figsize=(8, 6))
plt.scatter(y_test, y_pred, alpha=0.5)
plt.xlabel('Фактический доход')
plt.ylabel('Предсказанный доход')
plt.title('Фактический доход vs. Предсказанный доход')
plt.show()
# Гистограмма остатков
residuals = y_test - y_pred
plt.figure(figsize=(8, 6))
plt.hist(residuals, bins=30)
plt.xlabel('Остатки')
plt.ylabel('Частота')
plt.title('Гистограмма остатков')
plt.show()
# Важность признаков
importance = model.feature_importances_
feature_names = X.columns
plt.figure(figsize=(8, 6))
plt.bar(range(len(feature_names)), importance, tick_label=feature_names)
plt.xlabel('Признаки')
plt.ylabel('Важность')
plt.title('Важность признаков')
plt.xticks(rotation=90)
plt.show()
# График заработка мужчин и женщин
plt.figure(figsize=(8, 6))
df_male = df[df['Gender_Male'] == 1]
df_female = df[df['Gender_Female'] == 1]
plt.scatter(df_male['Income'], df_male['Income'], label='Мужчины', alpha=0.5, color='blue')
plt.scatter(df_female['Income'], df_female['Income'], label='Женщины', alpha=0.5, color='pink')
plt.xlabel('Фактический доход')
plt.ylabel('Фактический доход')
plt.title('Заработок мужчин и женщин')
plt.legend()
plt.show()
