## Отчет по лабораторной работе № 2

#### № группы: `ПМ-2401`

#### Выполнил: `Гречков Максим Дмитриевич`

#### Вариант: `9`

### Cодержание:

- [Постановка задачи](#1-постановка-задачи)
- [Входные и выходные данные](#2-входные-и-выходные-данные)
- [Выбор структуры данных](#3-выбор-структуры-данных)
- [Алгоритм](#4-алгоритм)
- [Программа](#5-программа)
- [Анализ правильности решения](#6-анализ-правильности-решения)

### 1. Постановка задачи

Программа должна выполнить следующие действия с одномерным массивом строк:

1. Считать с консоли число N, затем N строк и заполнить массив размером N.
2. Упорядочить строки по количеству слов в них. Если количество слов одинаковое, сортировать в порядке возрастания числа гласных букв. Если и это одинаково, сортировать строки по длине.
3. Найти и вывести самую часто встречающуюся букву во всех строках (без учёта регистра). Если таких букв несколько, вывести все.
4. Вывести все палиндромы среди введённых строк, а также количество таких строк.
5. Соединить все строки в одну, разделяя их запятыми, если строка не заканчивается знаком препинания. Затем подсчитать количество слов в полученной строке и вывести результат.

### 2. Входные и выходные данные

#### Данные на вход

На вход программа получает:
1. Целое число N - количество строк
2. N строк произвольной длины

#### Данные на выход

Программа выводит:
1. Отсортированные строки
2. Самые часто встречающиеся буквы
3. Палиндромы и их количество
4. Итоговую объединенную строку и количество слов в ней

### 3. Выбор структуры данных

Для решения задачи используются следующие структуры данных:

1. Массив строк `String[]` для хранения введенных строк
2. Массив целых чисел `int[]` для подсчета частоты встречаемости символов
3. Примитивные типы данных для хранения промежуточных результатов и счетчиков

### 4. Алгоритм

Алгоритм программы можно разбить на следующие основные шаги:

1. **Ввод данных:**
   - Считывание числа N с консоли.
   - Создание массива строк размером N.
   - Заполнение массива строками, введенными с консоли.

2. **Сортировка строк:**
   - Реализация метода сравнения строк `compareStrings`, который учитывает:
     a) Количество слов в строке (метод `countWords`).
     b) Количество гласных букв (метод `countVowels`).
     c) Длину строки.
   - Использование алгоритма пузырьковой сортировки для упорядочивания массива строк.
   - Вывод отсортированного массива.

3. **Поиск самых часто встречающихся букв:**
   - Создание массива `charCount` размером 65536 для подсчета частоты каждого символа Unicode.
   - Перебор всех символов во всех строках и подсчет частоты букв (без учета регистра).
   - Определение максимальной частоты.
   - Вывод всех букв, частота которых равна максимальной.

4. **Поиск и вывод палиндромов:**
   - Реализация метода `isPalindrome` для проверки, является ли строка палиндромом:
     a) Удаление всех небуквенных и нецифровых символов.
     b) Приведение строки к нижнему регистру.
     c) Проверка, читается ли строка одинаково с начала и с конца.
   - Проверка каждой строки в массиве на палиндром.
   - Подсчет количества палиндромов.
   - Вывод найденных палиндромов и их общего количества.

5. **Объединение строк и подсчет слов:**
   - Реализация метода `endsWithPunctuation` для проверки, заканчивается ли строка знаком препинания.
   - Объединение всех строк в одну с добавлением запятой и пробела между строками, если предыдущая строка не заканчивается знаком препинания.
   - Подсчет слов в итоговой строке с помощью метода `countWords`.
   - Вывод итоговой строки и количества слов в ней.

Дополнительные методы:

- `countWords`: Подсчитывает количество слов в строке, считая слово как последовательность непробельных символов.
- `countVowels`: Подсчитывает количество гласных букв в строке (учитывает как латинские, так и русские гласные).
- `isPalindrome`: Проверяет, является ли строка палиндромом, игнорируя пробелы, знаки препинания и регистр букв.
- `endsWithPunctuation`: Проверяет, заканчивается ли строка знаком препинания (точка, восклицательный или вопросительный знак).

### 5. Программа

```java
import java.util.Scanner;

public class Main {
    private static int compareStrings(String s1, String s2) {
        int wordCount1 = countWords(s1);
        int wordCount2 = countWords(s2);

        if (wordCount1 != wordCount2) {
            return wordCount1 - wordCount2;
        }

        int vowelCount1 = countVowels(s1);
        int vowelCount2 = countVowels(s2);

        if (vowelCount1 != vowelCount2) {
            return vowelCount1 - vowelCount2;
        }

        return s1.length() - s2.length();
    }

    private static int countWords(String s) {
        return s.trim().split("\\s+").length;
    }

    private static int countVowels(String s) {
        int count = 0;
        String vowels = "aeiouаеёиоуыэюя";
        for (int i = 0; i < s.length(); i++) {
            if (vowels.indexOf(Character.toLowerCase(s.charAt(i))) != -1) {
                count++;
            }
        }
        return count;
    }

    private static boolean isPalindrome(String word) {
        String cleaned = word.replaceAll("[^a-zA-Zа-яА-Я0-9]", "").toLowerCase();
        int length = cleaned.length();
        for (int i = 0; i < length / 2; i++) {
            if (cleaned.charAt(i) != cleaned.charAt(length - 1 - i)) {
                return false;
            }
        }
        return length > 1; // Палиндром должен содержать хотя бы 2 символа
    }

    private static boolean endsWithPunctuation(String s) {
        if (s.length() == 0) {
            return false;
        }
        char lastChar = s.charAt(s.length() - 1);
        return lastChar == '.' || lastChar == '!' || lastChar == '?';
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // 1. Считывание строк и заполнение массива
        System.out.print("Введите количество строк N: ");
        int N = scanner.nextInt();
        scanner.nextLine(); // Очистка буфера

        String[] strings = new String[N];
        for (int i = 0; i < N; i++) {
            System.out.print("Введите строку " + (i + 1) + ": ");
            strings[i] = scanner.nextLine();
        }

        // 2. Сортировка строк
        for (int i = 0; i < N - 1; i++) {
            for (int j = 0; j < N - i - 1; j++) {
                if (compareStrings(strings[j], strings[j + 1]) > 0) {
                    String temp = strings[j];
                    strings[j] = strings[j + 1];
                    strings[j + 1] = temp;
                }
            }
        }

        System.out.println("\nОтсортированные строки:");
        for (int i = 0; i < N; i++) {
            System.out.println(strings[i]);
        }

        // 3. Поиск самой часто встречающейся буквы
        int[] charCount = new int[65536]; // Увеличиваем размер массива для Unicode символов
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < strings[i].length(); j++) {
                char c = Character.toLowerCase(strings[i].charAt(j));
                if (Character.isLetter(c)) {
                    charCount[c]++;
                }
            }
        }

        int maxCount = 0;
        for (int i = 0; i < 65536; i++) {
            if (charCount[i] > maxCount) {
                maxCount = charCount[i];
            }
        }

        System.out.println("\nСамые часто встречающиеся буквы:");
        for (int i = 0; i < 65536; i++) {
            if (charCount[i] == maxCount && Character.isLetter((char)i)) {
                System.out.print((char)i + " ");
            }
        }
        System.out.println();

        // 4. Поиск и вывод палиндромов
        int totalPalindromeCount = 0;
        System.out.println("\nПалиндромы:");
        for (int i = 0; i < N; i++) {
            String[] words = strings[i].split("\\s+");
            for (String word : words) {
                if (isPalindrome(word)) {
                    System.out.println(word);
                    totalPalindromeCount++;
                }
            }
        }
        System.out.println("Количество палиндромов: " + totalPalindromeCount);

        // 5. Соединение строк и подсчет слов
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < N; i++) {
            result.append(strings[i]);
            if (i < N - 1 && !endsWithPunctuation(strings[i])) {
                result.append(", ");
            }
        }

        String finalString = result.toString();
        int wordCount = countWords(finalString);

        System.out.println("\nИтоговая строка:");
        System.out.println(finalString);
        System.out.println("Количество слов в итоговой строке: " + wordCount);
    }
}
