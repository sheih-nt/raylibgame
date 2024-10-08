## Лекция 1: Введение в программирование и библиотеку raylib

### Цели лекции:
1. Познакомиться с основами программирования.
2. Установить и настроить среду разработки для работы с библиотекой **raylib**.
3. Написать и запустить первую простую программу с использованием **raylib**.

---

### 1. Что такое raylib?

**raylib** — это легкая библиотека для разработки игр и графических приложений на языке C. Она предназначена для упрощения процесса создания игр, предлагая удобные функции для работы с графикой, вводом с клавиатуры, мыши и другими устройствами. Основные преимущества raylib:
- Простота использования.
- Кроссплатформенность (Windows, macOS, Linux).
- Отлично подходит для новичков.

### 2. Установка и настройка среды разработки

#### 2.1. Установка компилятора
Для работы с **raylib** на языке C, вам понадобится компилятор. Вот несколько популярных вариантов:

- **Windows**: Установите **MinGW** с компилятором GCC.
- **macOS**: Установите **Xcode** через Mac App Store или через терминал с помощью команды `xcode-select --install`.
- **Linux**: Установите GCC через менеджер пакетов вашей системы (например, `sudo apt install build-essential` для Ubuntu).

#### 2.2. Установка библиотеки raylib
Скачайте последнюю версию raylib с официального сайта [raylib.com](https://www.raylib.com). Следуйте инструкциям по установке для вашей платформы, чтобы подключить библиотеку к проекту.

#### 2.3. Среда разработки
Рекомендуется использовать **Visual Studio Code** (VS Code) в качестве основной среды разработки. Установите расширение для работы с C/C++ и настройте проект для компиляции с использованием raylib.

### 3. Первая программа на raylib

Теперь, когда среда настроена, давайте создадим нашу первую программу. Она откроет окно с текстом "Hello, raylib!" и позволит изменять цвет фона с помощью клавиш.

#### Шаг 1: Создание окна

В raylib создание окна и работа с ним происходят с помощью функций `InitWindow` и `CloseWindow`. Весь процесс рендеринга (отрисовки) управляется циклом, который продолжается до тех пор, пока окно не будет закрыто пользователем.

```c
#include "raylib.h"

int main() {
    InitWindow(800, 600, "Hello, raylib!"); // Создание окна размером 800x600
    while (!WindowShouldClose()) {  // Цикл работы окна
        BeginDrawing();             // Начало отрисовки
        ClearBackground(RAYWHITE);  // Очистка фона (белый цвет)
        DrawText("Hello, raylib!", 190, 280, 20, LIGHTGRAY);  // Вывод текста
        EndDrawing();               // Конец отрисовки
    }
    CloseWindow();  // Закрытие окна после выхода из цикла
    return 0;
}
```

#### Шаг 2: Управление цветом фона

Теперь мы добавим обработку ввода с клавиатуры, чтобы изменять цвет фона при нажатии клавиш `R`, `G` и `B`.

```c
#include "raylib.h"

int main() {
    InitWindow(800, 600, "Change background color");

    Color bgColor = RAYWHITE;  // Начальный цвет фона

    while (!WindowShouldClose()) {
        // Обработка нажатий клавиш
        if (IsKeyPressed(KEY_R)) bgColor = RED;    // Меняем фон на красный
        if (IsKeyPressed(KEY_G)) bgColor = GREEN;  // Меняем фон на зеленый
        if (IsKeyPressed(KEY_B)) bgColor = BLUE;   // Меняем фон на синий

        BeginDrawing();
        ClearBackground(bgColor);  // Используем переменную цвета для фона
        DrawText("Press R, G, or B to change background color", 150, 280, 20, LIGHTGRAY);
        EndDrawing();
    }

    CloseWindow();  // Закрытие окна
    return 0;
}
```

### 4. Основные понятия программирования

Перед тем как углубляться в код, важно познакомиться с основными концепциями программирования:

- **Переменные** — это контейнеры для хранения данных. Например, переменная `bgColor` хранит текущий цвет фона.
- **Типы данных**: В C есть несколько типов данных, таких как:
  - `int` — целое число.
  - `float` — число с плавающей точкой.
  - `char` — символ.
  - `bool` — логическое значение (истина или ложь).
  
- **Условные операторы**: Позволяют выполнять действия на основе условий. В нашем примере мы используем функцию `IsKeyPressed` для проверки, была ли нажата клавиша.
  
- **Циклы**: В raylib основной игровой цикл основан на `while (!WindowShouldClose())`. Это цикл, который повторяется, пока окно не закрыто.

### 5. Практическое задание

Теперь, когда мы научились создавать окно и изменять цвет фона, попробуйте выполнить следующее задание:

1. **Измените программу так, чтобы добавились другие цвета фона.** Например, пусть при нажатии клавиш `A`, `S` или `D` фон будет становиться жёлтым, пурпурным или чёрным соответственно.
   
2. **Добавьте вывод разного текста в зависимости от нажатой клавиши.** Например, выводите на экран название выбранного цвета.

### Пример:

```c
#include "raylib.h"

int main() {
    InitWindow(800, 600, "Interactive background");

    Color bgColor = RAYWHITE;
    const char* colorText = "White";  // Текущий цвет в виде текста

    while (!WindowShouldClose()) {
        if (IsKeyPressed(KEY_R)) { bgColor = RED; colorText = "Red"; }
        if (IsKeyPressed(KEY_G)) { bgColor = GREEN; colorText = "Green"; }
        if (IsKeyPressed(KEY_B)) { bgColor = BLUE; colorText = "Blue"; }
        if (IsKeyPressed(KEY_Y)) { bgColor = YELLOW; colorText = "Yellow"; }

        BeginDrawing();
        ClearBackground(bgColor);
        DrawText(TextFormat("Background color: %s", colorText), 190, 280, 20, LIGHTGRAY);  // Вывод текста
        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```

### Заключение

Поздравляем! Вы только что написали свою первую программу с использованием raylib. В этом занятии вы узнали, как:
- Создавать окно и управлять его отрисовкой.
- Работать с текстом и цветами.
- Обрабатывать ввод с клавиатуры.

На следующем занятии мы углубимся в работу с графикой и научимся рисовать фигуры и управлять их движением по экрану!

---

### Домашнее задание

1. **Расширьте свою программу:** Добавьте больше цветов и возможностей для изменения фона.
2. **Экспериментируйте с текстом и шрифтами:** Попробуйте изменить размер и цвет текста в программе.
