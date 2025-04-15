import json
from datetime import datetime

def process_orders(file_path):
    # Загрузка данных из файла
    with open(file_path, 'r', encoding='utf-8') as file:
        orders = json.load(file)

    max_cost_order = {'order_number': None, 'total_cost': 0}
    max_items_order = {'order_number': None, 'total_items': 0}
    daily_orders = {}
    user_order_counts = {}
    user_total_spent = {}
    total_cost_sum = 0
    total_items_sum = 0
    total_products_cost = 0
    total_products_count = 0

   
    for order in orders:
        # 1. Самый дорогой заказ
        if order['total_cost'] > max_cost_order['total_cost']:
            max_cost_order = {
                'order_number': order['order_number'],
                'total_cost': order['total_cost']
            }

        # 2. Заказ с наибольшим количеством товаров
        if order['total_items'] > max_items_order['total_items']:
            max_items_order = {
                'order_number': order['order_number'],
                'total_items': order['total_items']
            }

        # 3. Подсчет заказов по дням
        order_date = datetime.strptime(order['date'], '%Y-%m-%d')
        day = order_date.day
        daily_orders[day] = daily_orders.get(day, 0) + 1

        # 4. Подсчет заказов по пользователям
        user_order_counts[order['user_id']] = user_order_counts.get(order['user_id'], 0) + 1

        # 5. Суммарная стоимость по пользователям
        user_total_spent[order['user_id']] = user_total_spent.get(order['user_id'], 0) + order['total_cost']

        # 6-7. Данные для средних значений
        total_cost_sum += order['total_cost']
        total_items_sum += order['total_items']

        # Расчет стоимости всех товаров
        for item in order['items']:
            total_products_cost += item['price'] * item['quantity']
            total_products_count += item['quantity']

    # Результаты
    return {
        'most_expensive_order': max_cost_order['order_number'],
        'largest_items_order': max_items_order['order_number'],
        'busiest_day': max(daily_orders, key=daily_orders.get),
        'most_active_user': max(user_order_counts, key=user_order_counts.get),
        'top_spender': max(user_total_spent, key=user_total_spent.get),
        'average_order_value': total_cost_sum / len(orders),
        'average_item_value': total_products_cost / total_products_count
    }
