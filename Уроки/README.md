Лекция 2: Рисование примитивов и работа с графикой в raylib
Цель занятия
Научиться работать с графическими примитивами (линии, круги, квадраты) и динамически изменять их параметры. На практике вы создадите простую анимацию, познакомитесь с функциями raylib для рисования и работы с цветами.

Теория
1. Примитивы в raylib
Raylib предоставляет функции для работы с основными графическими примитивами:

Линии: рисуются с помощью функции DrawLine(int startX, int startY, int endX, int endY, Color color).

Пример:

cpp
Копировать код
DrawLine(100, 100, 200, 200, RED);
Прямоугольники: используются функции для отрисовки закрашенных прямоугольников и их контуров:

DrawRectangle(int posX, int posY, int width, int height, Color color) для закрашенного прямоугольника.
DrawRectangleLines(int posX, int posY, int width, int height, Color color) для контура.
Пример:

cpp
Копировать код
DrawRectangle(300, 100, 150, 100, BLUE);
DrawRectangleLines(300, 100, 150, 100, BLACK);
Круги: рисуются с помощью DrawCircle(int centerX, int centerY, float radius, Color color) для закрашенного круга. Контур круга можно рисовать с DrawCircleLines.

Пример:

cpp
Копировать код
DrawCircle(500, 200, 50, GREEN);
Треугольники: задаются тремя вершинами, рисуются с помощью DrawTriangle(Vector2 v1, Vector2 v2, Vector2 v3, Color color).

Пример:

cpp
Копировать код
DrawTriangle((Vector2){600, 400}, (Vector2){550, 500}, (Vector2){650, 500}, PURPLE);
2. Работа с цветами
В raylib доступен набор встроенных цветов (например, RED, BLUE, GREEN, WHITE, BLACK). Также можно создавать пользовательские цвета с помощью структуры Color, задавая значения каналов RGB и альфа-канала.

Пример создания цвета:

cpp
Копировать код
Color customColor = (Color){ 255, 100, 150, 255 };  // Красный, зеленый, синий, альфа-канал
Пример кода: Рисование примитивов
Пример программы, которая рисует различные фигуры на экране:

cpp
Копировать код
#include "raylib.h"

int main() {
    // Инициализация окна
    InitWindow(800, 600, "Рисование примитивов в raylib");

    // Основной цикл программы
    while (!WindowShouldClose()) {
        BeginDrawing();
        ClearBackground(RAYWHITE); // Задаем белый фон

        // Рисуем различные примитивы
        DrawLine(100, 100, 200, 200, RED);  // Линия
        DrawRectangle(300, 100, 150, 100, BLUE); // Прямоугольник
        DrawCircle(500, 200, 50, GREEN);    // Круг
        DrawTriangle((Vector2){600, 400}, (Vector2){550, 500}, (Vector2){650, 500}, PURPLE); // Треугольник

        EndDrawing();
    }

    // Закрытие окна
    CloseWindow();
    return 0;
}
Практическая часть
Задание 1: Анимация примитивов
Модифицируйте пример программы таким образом, чтобы примитивы двигались или меняли свои размеры со временем.
Задание 2: Движение фигур с клавиатуры
Создайте программу, которая позволяет управлять фигурой с помощью клавиш на клавиатуре. Пример для движения прямоугольника:

cpp
Копировать код
#include "raylib.h"

int main() {
    InitWindow(800, 600, "Движение фигуры");

    int rectX = 300; // Начальные координаты прямоугольника
    int rectY = 200;
    int rectWidth = 100;
    int rectHeight = 50;

    while (!WindowShouldClose()) {
        // Обработка ввода
        if (IsKeyDown(KEY_RIGHT)) rectX += 5;
        if (IsKeyDown(KEY_LEFT)) rectX -= 5;
        if (IsKeyDown(KEY_UP)) rectY -= 5;
        if (IsKeyDown(KEY_DOWN)) rectY += 5;

        BeginDrawing();
        ClearBackground(RAYWHITE);

        DrawRectangle(rectX, rectY, rectWidth, rectHeight, BLUE);

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
Домашнее задание
Добавьте новые фигуры и анимации в вашу программу. Попробуйте сделать, чтобы они изменяли размеры или цвет со временем.
Реализуйте возможность увеличения или уменьшения размеров фигур с помощью клавиш + и
