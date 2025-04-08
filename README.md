# Многопоточный анализатор текстовых файлов

## Техническое задание
### Цели проекта
- Практика работы со структурами данных
- Изучение многопоточности и синхронизации потоков
- Реализация асинхронной обработки файлов

### Реализованные функции
1. Структуры данных
```go

// FileAnalysis хранит результаты анализа одного файла
type FileAnalysis struct {
    FileName  string // Имя файла
    WordCount int    // Количество слов
    CharCount int    // Количество символов
    Error     string // Ошибка обработки (если есть)
}

// GlobalResults хранит совокупные результаты
type GlobalResults struct {
    Files      map[int]FileAnalysis // Результаты по файлам
    TotalWords int                  // Общее количество слов
    TotalChars int                  // Общее количество символов
}
```
2. Многопоточная обработка
- Используется пул горутин для параллельной обработки
- Синхронизация через sync.Mutex
- Обработка ошибок чтения файлов

3. Асинхронная обработка
```go
Copy
// AnalyzeFiles запускает асинхронную обработку
func (a *Analyzer) AnalyzeFiles(filePaths []string) (*GlobalResults, error) {
    // ... 
    for i, filePath := range filePaths {
        wg.Add(1)
        go func(index int, path string) {
            defer wg.Done()
            a.analyzeFile(index, path, results)
        }(i, filePath)
    }
    // ...
}
```
4. Вывод результатов
Пример вывода:

```cmd
Результаты анализа:
1. file1.txt: 120 слов, 800 символов
2. file2.txt: ОШИБКА - файл не найден
3. file3.txt: 200 слов, 1250 символов


Итог: 320 слов, 2050 символов.
```
