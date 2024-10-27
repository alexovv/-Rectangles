# -Rectangles
def vector_product(x1, y1, x2, y2, x3, y3):
    """Вычисляем векторное произведение для ориентации"""
    return (x2 - x1) * (y3 - y1) - (y2 - y1) * (x3 - x1)

def is_point_in_triangle(px, py, ax, ay, bx, by, cx, cy):
    """Проверка, находится ли точка внутри треугольника, используя векторное произведение."""
    d1 = vector_product(ax, ay, bx, by, px, py)
    d2 = vector_product(bx, by, cx, cy, px, py)
    d3 = vector_product(cx, cy, ax, ay, px, py)
    # Проверяем, что все три векторных произведения одинакового знака
    return (d1 >= 0 and d2 >= 0 and d3 >= 0) or (d1 <= 0 and d2 <= 0 and d3 <= 0)

def do_segments_intersect(x1, y1, x2, y2, x3, y3, x4, y4):
    """Проверка пересечения отрезков [(x1, y1), (x2, y2)] и [(x3, y3), (x4, y4)]"""
    d1 = vector_product(x3, y3, x4, y4, x1, y1)
    d2 = vector_product(x3, y3, x4, y4, x2, y2)
    d3 = vector_product(x1, y1, x2, y2, x3, y3)
    d4 = vector_product(x1, y1, x2, y2, x4, y4)

    # Основное условие пересечения отрезков
    if d1 * d2 < 0 and d3 * d4 < 0:
        return True
    # Специальные случаи, когда точки лежат на одном отрезке
    if d1 == 0 and on_segment(x3, y3, x4, y4, x1, y1): return True
    if d2 == 0 and on_segment(x3, y3, x4, y4, x2, y2): return True
    if d3 == 0 and on_segment(x1, y1, x2, y2, x3, y3): return True
    if d4 == 0 and on_segment(x1, y1, x2, y2, x4, y4): return True

    return False

def on_segment(x1, y1, x2, y2, x, y):
    """Проверка, находится ли точка (x, y) на отрезке [(x1, y1), (x2, y2)]."""
    return min(x1, x2) <= x <= max(x1, x2) and min(y1, y2) <= y <= max(y1, y2)

def check_triangle_relationship(triangle1, triangle2):
    # Проверка пересечения сторон треугольников
    for i in range(3):
        for j in range(3):
            x1, y1 = triangle1[i]
            x2, y2 = triangle1[(i + 1) % 3]
            x3, y3 = triangle2[j]
            x4, y4 = triangle2[(j + 1) % 3]
            if do_segments_intersect(x1, y1, x2, y2, x3, y3, x4, y4):
                return "Треугольники пересекаются"

    
    if all(is_point_in_triangle(x, y, *triangle2[0], *triangle2[1], *triangle2[2]) for x, y in triangle1):
        return "Треугольник 1 находится внутри Треугольника 2"
    if all(is_point_in_triangle(x, y, *triangle1[0], *triangle1[1], *triangle1[2]) for x, y in triangle2):
        return "Треугольник 2 находится внутри Треугольника 1"

    return "Треугольники не пересекаются и не вложены друг в друга"

# Пример использования
print("введите координаты вершин первого треугольника: ")
a1, a2, b1, b2, c1, c2 = map(int, input().split())
triangle1 = [(a1, a2), (b1, b2), (c1, c2)]
print("введите координаты вершин второго треугольника: ")
a1, a2, b1, b2, c1, c2 = map(int, input().split())
triangle2 = [(a1, a2), (b1, b2), (c1, c2)]

result = check_triangle_relationship(triangle1, triangle2)
print(result)
