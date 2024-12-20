# Инструкция по запуску проекта

## Запуск заданий 1–4
Чтобы запустить задания с 1 по 5:
1. Выберите файл, который вы хотите запустить. 
2. В параметрах запуска укажите "Current file".
3. Запустите проект.

## Запуск заданий 5, 6, 7 и тестов
Для запуска заданий 5, 6, 7 и тестов необходимо:
1. Открыть файл `main.js`.
2. Указать путь к нужному файлу `App.vue` в следующей строке:
   ```javascript
   import App from './tests/App.vue'; // Укажите нужную папку
   ```
3. Сохраните изменения в файле.
4. Запустите проект.

## Общие требования
Убедитесь, что у вас установлены:
- Node.js
- Vue CLI

Запуск осуществляется через команду:
```bash
npm run dev