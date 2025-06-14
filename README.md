# lab004

# Проект: Система сборки и CI

## Описание

В рамках стажировки в компании "Formatter Inc." была реализована система сборки CMake и настроена система непрерывной интеграции (CI) с использованием GitHub Actions вместо TravisCI и AppVeyor.

## Автоматизация сборки с помощью GitHub Actions

### Сборка на Linux (Ubuntu) с использованием GCC и Clang

Для проверки сборки на Linux создан workflow .github/workflows/linux.yml. В нём проект автоматически собирается двумя компиляторами: GCC и Clang.

#### Используемые команды и их назначение:

| Команда | Описание |
|--------|-------------|
| runs-on: ubuntu-latest | Запуск сборки на последней версии Ubuntu. |
| strategy.matrix.compiler: [gcc, clang] | Задание матрицы сборки для двух компиляторов: GCC и Clang. |
| uses: actions/checkout@v4 | Клонирование исходного кода репозитория. |
| uses: lukka/get-cmake@latest | Установка CMake. |
| mkdir build | Создание отдельного каталога для сборки. |
| cd build | Переход в каталог сборки. |
| cmake -DCMAKE_C_COMPILER=${{ matrix.compiler }} -DCMAKE_CXX_COMPILER=${{ matrix.compiler }} .. | Конфигурация сборки для выбранного компилятора (GCC или Clang). |
| cmake --build . | Сборка проекта. |

#### Результат:
- Проект собирается двумя разными компиляторами.
- Проверка успешности сборки при каждом коммите или Pull Request.

---

### Сборка на Windows с использованием Microsoft Visual Studio (MSVC)

Для проверки сборки на Windows создан workflow .github/workflows/windows.yml. В нём проект собирается с помощью генератора Visual Studio.

#### Используемые команды и их назначение:

| Команда | Описание |
|--------|-------------|
| runs-on: windows-latest | Запуск сборки на последней версии Windows. |
| uses: actions/checkout@v4 | Клонирование репозитория. |
| uses: lukka/get-cmake@latest | Установка CMake. |
| mkdir build | Создание каталога сборки. |
| cd build | Переход в каталог сборки. |
| cmake -G "Visual Studio 17 2022" .. | Генерация файлов решения Visual Studio 2022. |
| cmake --build . --config Release | Сборка проекта в режиме Release. |

#### Результат:
- Проверка сборки проекта на Windows.
- Использование MSVC (Microsoft Visual C++ Compiler).

---

## Вывод

В результате настроена автоматическая сборка проекта на двух операционных системах (Linux и Windows) с использованием различных компиляторов.  
Это позволяет убедиться в переносимости и корректной работе проекта на разных платформах.

Настройка GitHub Actions заменяет требуемые TravisCI и AppVeyor из задания.
