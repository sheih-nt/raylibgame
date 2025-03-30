# 🕒 Мастер-класс: Анимация часов в Raylib (C++)

## **1. Введение**  
### 📌 Что будем делать?  
- Создадим аналоговые часы.  
- Рассчитаем углы поворота стрелок.  
- Добавим управление временем.  

### 👀 Важные концепции  
Перед тем как начать, важно понимать несколько базовых вещей:
1. **Координаты в Raylib**: (0,0) — верхний левый угол, центр экрана у нас будет в (300,300).
2. **Анимация в играх**: Каждые 1/60 секунды кадр обновляется (FPS = 60).
3. **Работа с углами (DEG2RAD)**: Raylib работает с радианами, а нам удобнее градусы.

---

## **2. Создаём окно и настраиваем Raylib**  
### 📌 Сначала создадим окно и напечатаем текст

```cpp
#include "raylib.h"

const int screenWidth = 600;
const int screenHeight = 600;

int main()
{
    InitWindow(screenWidth, screenHeight, "Часы в Raylib");
    SetTargetFPS(60);

    while (!WindowShouldClose()) // Основной цикл игры
    {
        BeginDrawing();
        ClearBackground(RAYWHITE);

        DrawText("Здесь будут часы!", 150, screenHeight / 2, 20, BLACK);

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```

✅ **Зачем это нужно?**  
- `InitWindow()` создаёт окно программы.  
- `SetTargetFPS(60)` устанавливает частоту обновления 60 кадров в секунду.  
- `BeginDrawing()` и `EndDrawing()` — блоки отрисовки, в которых будем рисовать часы.  

---

## **3. Рисуем циферблат**  
Теперь добавим основу наших часов — циферблат.

### 📌 Создаём функцию `DrawClockFace()`

```cpp
void DrawClockFace()
{
    Vector2 center = { screenWidth / 2.0f, screenHeight / 2.0f };

    // Рисуем внешний круг
    DrawCircleV(center, 250, DARKGRAY);
    DrawCircleV(center, 245, RAYWHITE);

    // Рисуем метки часов
    for (int i = 0; i < 12; i++)
    {
        float angle = i * 30 * DEG2RAD;
        Vector2 start = { center.x + cosf(angle) * 220, center.y + sinf(angle) * 220 };
        Vector2 end = { center.x + cosf(angle) * 240, center.y + sinf(angle) * 240 };
        DrawLineV(start, end, BLACK);
    }
}
```

📌 **Теперь вызываем эту функцию в `main()`**

```cpp
BeginDrawing();
ClearBackground(RAYWHITE);
DrawClockFace();
EndDrawing();
```

✅ **Зачем мы это сделали?**  
- `DrawCircleV()` рисует основу часов.  
- `for (int i = 0; i < 12; i++)` создаёт 12 меток, каждая с шагом 30° (360° / 12).  
- `cosf(angle)` и `sinf(angle)` рассчитывают координаты на окружности.  

---

## **4. Добавляем стрелки**  
Теперь, давайте добавим стрелки для часов, минут и секунд.

### 📌 Создаём функцию `DrawClockHands()`

```cpp
void DrawClockHands(float hourAngle, float minuteAngle, float secondAngle)
{
    Vector2 center = { screenWidth / 2.0f, screenHeight / 2.0f };

    Vector2 hourEnd = { center.x + cosf((hourAngle - 90) * DEG2RAD) * 120,
                        center.y + sinf((hourAngle - 90) * DEG2RAD) * 120 };
    DrawLineEx(center, hourEnd, 8, BLACK);

    Vector2 minuteEnd = { center.x + cosf((minuteAngle - 90) * DEG2RAD) * 180,
                          center.y + sinf((minuteAngle - 90) * DEG2RAD) * 180 };
    DrawLineEx(center, minuteEnd, 5, DARKGRAY);

    Vector2 secondEnd = { center.x + cosf((secondAngle - 90) * DEG2RAD) * 200,
                          center.y + sinf((secondAngle - 90) * DEG2RAD) * 200 };
    DrawLineEx(center, secondEnd, 2, RED);
}
```

📌 **Добавляем тестовый вызов в `main()`**

```cpp
DrawClockHands(90, 180, 270); // Пробное рисование стрелок
```

✅ **Почему `-90` в углах?**  
В Raylib 0° — это вправо, а мы хотим, чтобы стрелки начинались сверху (на 12 часов).  

---

## **5. Делаем стрелки динамическими**  
Теперь, вместо фиксированных углов, нам нужно получать реальное время.

### 📌 Создаём функцию `GetRotationAngles()`

```cpp
void GetRotationAngles(float &hourAngle, float &minuteAngle, float &secondAngle)
{
    time_t now = time(nullptr);
    struct tm ltm;
    localtime_s(&ltm, &now);

    int hours = ltm.tm_hour;
    int minutes = ltm.tm_min;
    int seconds = ltm.tm_sec;

    hourAngle = (hours % 12 + minutes / 60.0f) * 30.0f;
    minuteAngle = (minutes + seconds / 60.0f) * 6.0f;
    secondAngle = seconds * 6.0f;
}
```

📌 **Вызываем её в `main()`**

```cpp
float hourAngle, minuteAngle, secondAngle;
GetRotationAngles(hourAngle, minuteAngle, secondAngle);
DrawClockHands(hourAngle, minuteAngle, secondAngle);
```

✅ **Что изменилось?**  
- Теперь стрелки отображают текущее время!

---

## **6. Добавляем управление временем**  
Сейчас наши часы идут по реальному времени. Давайте добавим возможность изменять его!

### 📌 Добавляем переменные

```cpp
bool isPaused = false;
float timeSpeed = 1.0f;
double simulatedTime = 0.0;
```

### 📌 Функция для управления временем

Создадим функцию, которая будет отвечать за обработку нажатий клавиш для изменения времени.

```cpp
void UpdateTimeControl(bool &isPaused, float &timeSpeed, double &simulatedTime)
{
    if (IsKeyPressed(KEY_SPACE)) isPaused = !isPaused; // Пауза
    if (IsKeyPressed(KEY_UP)) timeSpeed *= 2.0f; // Ускорение времени
    if (IsKeyPressed(KEY_DOWN)) timeSpeed /= 2.0f; // Замедление времени
    if (IsKeyDown(KEY_RIGHT)) simulatedTime += 60; // Перемотка времени вперёд
    if (IsKeyDown(KEY_LEFT)) simulatedTime -= 60; // Перемотка времени назад
}
```

### 📌 Обновляем основную логику в `main()`

Теперь обновим цикл игры, чтобы использовать нашу функцию управления временем.

```cpp
while (!WindowShouldClose())
{
    // Обновление управления временем
    UpdateTimeControl(isPaused, timeSpeed, simulatedTime);

    // Вычисление углов для стрелок
    if (!isPaused)
    {
        simulatedTime += GetFrameTime() * timeSpeed;
    }

    float hourAngle, minuteAngle, secondAngle;
    GetRotationAngles(hourAngle, minuteAngle, secondAngle);

    // Рисование на экране
    BeginDrawing();
    ClearBackground(RAYWHITE);
    
    DrawClockFace(); // Рисуем циферблат
    DrawClockHands(hourAngle, minuteAngle, secondAngle); // Рисуем стрелки
    
    EndDrawing();
}
```

✅ **Почему так лучше?**  
- **Модульность**: Функция `UpdateTimeControl()` отвечает только за обработку клавиш и изменение параметров времени. Это облегчает чтение и поддержку кода.
- **Чистота кода**: Основной цикл в `main()` стал чище, и мы можем легче добавить дополнительные элементы управления, если это понадобится.

---

## **7. Финальный код в `main()`**

```cpp
while (!WindowShouldClose())
{
    // Обновление управления временем
    UpdateTimeControl(isPaused, timeSpeed, simulatedTime);
    
    float hourAngle, minuteAngle, secondAngle;
    if (!isPaused)
    {
        simulatedTime += GetFrameTime() * timeSpeed;
    }

    // Получаем углы для стрелок
    GetRotationAngles(hourAngle, minuteAngle, secondAngle);

    // Рисуем всё на экране
    BeginDrawing();
    ClearBackground(RAYWHITE);
    
    DrawClockFace();
    DrawClockHands(hourAngle, minuteAngle, secondAngle);
    
    EndDrawing();
}
```

---

# 🎯 Итоги  
✅ Нарисовали циферблат и стрелки.  
✅ Сделали динамическое время.  
✅ Добавили управление временем.

🚀 **Можно улучшить:**  
- Добавить анимацию плавного движения минутной и часовой стрелки.  
- Сделать цифровые часы.  
- Добавить текстуры и эффекты.
