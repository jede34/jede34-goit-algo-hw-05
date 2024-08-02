import re
from typing import Callable

def generator_numbers(text: str):
    """
    Генератор, який ітерує всі дійсні числа у тексті.
    Числа повинні бути відокремлені пробілами з обох боків.
    """
    # Регулярний вираз для знаходження дійсних чисел
    number_pattern = r'\b\d+\.\d+\b'
    # Знаходимо всі відповідності
    matches = re.findall(number_pattern, text)
    # Генеруємо числа з знайдених відповідностей
    for match in matches:
        yield float(match)

def sum_profit(text: str, func: Callable):
    """
    Обчислює загальний прибуток, використовуючи генератор чисел.
    """
    total = sum(func(text))
    return total

# Приклад використання
text = "Загальний дохід працівника складається з декількох частин: 1000.01 як основний дохід, доповнений додатковими надходженнями 27.45 і 324.00 доларів."
total_income = sum_profit(text, generator_numbers)
print(f"Загальний дохід: {total_income:.2f}")
