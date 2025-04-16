import json
from collections import defaultdict

# Загрузка данных из файла
with open('orders.json', 'r', encoding='utf-8') as file:
    orders = json.load(file)

# 1. Самый дорогой заказ
most_expensive_order = max(orders, key=lambda x: x['total_price'])
most_expensive_order_id = most_expensive_order['order_id']

# 2. Заказ с наибольшим количеством товаров
most_items_order = max(orders, key=lambda x: len(x['items']))
most_items_order_id = most_items_order['order_id']

# 3. День с наибольшим количеством заказов
order_dates = [order['order_date'] for order in orders]
date_count = defaultdict(int)
for date in order_dates:
    date_count[date] += 1
busiest_day = max(date_count, key=lambda k: date_count[k])

# 4. Пользователь с наибольшим количеством заказов
user_orders = defaultdict(int)
for order in orders:
    user_orders[order['user_id']] += 1
most_orders_user = max(user_orders, key=lambda k: user_orders[k])

# 5. Пользователь с наибольшей суммарной стоимостью заказов
user_spending = defaultdict(float)
for order in orders:
    user_spending[order['user_id']] += order['total_price']
top_spending_user = max(user_spending, key=lambda k: user_spending[k])

# 6. Средняя стоимость заказа
average_order_price = sum(order['total_price'] for order in orders) / len(orders)

# 7. Средняя стоимость товаров
total_items_price = 0
total_items_count = 0
for order in orders:
    items = order['items']
    total_items_count += len(items)
    total_items_price += sum(item['price'] for item in items)
average_item_price = total_items_price / total_items_count if total_items_count > 0 else 0

# Вывод результатов
print(f"Самый дорогой заказ: {most_expensive_order_id}")
print(f"Заказ с наибольшим количеством товаров: {most_items_order_id}")
print(f"День с наибольшим количеством заказов: {busiest_day}")
print(f"Пользователь с наибольшим количеством заказов: {most_orders_user}")
print(f"Пользователь с наибольшей суммарной стоимостью заказов: {top_spending_user}")
print(f"Средняя стоимость заказа: {average_order_price:.2f}")
print(f"Средняя стоимость товаров: {average_item_price:.2f}")
