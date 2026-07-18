# krestiki-niliki1
def print_board(board):
    """
    Отображает текущее состояние поля в консоли.
    Использует форматированные строки для аккуратного вывода.
    """
    print("\nТекущее поле:")
    for row in board:
        print(" | ".join(row))
        print("-" * 9)  # разделитель между строками

def check_game_over(board):
    """
    Проверяет, завершилась ли игра.
    Возвращает:
    - 'X', если победили крестики
    - 'O', если победили нолики
    - 'Draw', если ничья
    - None, если игра еще продолжается
    """
    # Проверка победных условий (по линиям)
    lines = []

    # горизонтали
    lines.extend(board)
    # вертикали
    for col in range(3):
        lines.append([board[row][col] for row in range(3)])
    # диагонали
    lines.append([board[i][i] for i in range(3)])
    lines.append([board[i][2 - i] for i in range(3)])
    
    # Проверка победы
    for line in lines:
        if line == ['X', 'X', 'X']:
            return 'X'
        elif line == ['O', 'O', 'O']:
            return 'O'

    # Проверка ничьи
    if all(cell != ' ' for row in board for cell in row):
        return 'Draw'
    
    return None  # игра продолжается

def get_player_move(player, board):
    """
    Запрашивает у игрока координаты хода, проверяет их корректность.
    """
    while True:
        try:
            coords = input(f"Игрок {player}, введите координаты хода (строка и столбец через пробел 1-3): ")
            row_str, col_str = coords.strip().split()
            row, col = int(row_str) - 1, int(col_str) - 1  # приведение к индексам списка
            # Проверка границ
            if not (0 <= row < 3 and 0 <= col < 3):
                print("Координаты вне диапазона. Попробуйте снова.")
                continue
            # Проверка свободной ячейки
            if board[row][col] != ' ':
                print("Ячейка уже занята. Попробуйте другую.")
                continue
            return row, col
        except ValueError:
            print("Некорректный ввод. Введите два числа через пробел от 1 до 3.")

# Инициализация поля
board = [[' ' for _ in range(3)] for _ in range(3)]

current_player = 'X'  # крестики начинают
game_over = False

while not game_over:
    print_board(board)
    row, col = get_player_move(current_player, board)
    board[row][col] = current_player

    result = check_game_over(board)
    if result:
        print_board(board)
        if result == 'Draw':
            print("Ничья!")
        else:
            print(f"Игрок {result} выиграл!")
        game_over = True
    else:
        # Меняем игрока
        current_player = 'O' if current_player == 'X' else 'X'
