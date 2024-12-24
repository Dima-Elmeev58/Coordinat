# Coordinat
class Program 
{
    static void Main() 
    {
        try // Начинаем блок try для обработки возможных исключений.
        {
            // Создаем объект StreamReader для чтения файла с координатами.
            using (StreamReader sr = new StreamReader("C:\\Users\\revno\\Doc\\coord.txt"))
            {
                int n = int.Parse(sr.ReadLine()); // Считываем первое значение из файла, которое представляет количество занятых мест, и преобразуем его в целое число.
                List<Tuple<int, int>> coordinates = new List<Tuple<int, int>>(); // Создаем список для хранения координат занятых мест.

                // Считываем координаты занятых мест
                for (int i = 0; i < n; i++) // Цикл для считывания n координат.
                {
                    string[] inputs = sr.ReadLine().Split(); // Считываем строку из файла и разбиваем ее на части по пробелам.
                    int x = int.Parse(inputs[0]); // Преобразуем первую часть (координата x) в целое число.
                    int y = int.Parse(inputs[1]); // Преобразуем вторую часть (координата y) в целое число.
                    coordinates.Add(new Tuple<int, int>(x, y)); // Добавляем координаты в список в виде кортежа.
                }

                // Сортируем координаты по рядам и местам
                coordinates.Sort((a, b) => a.Item1 != b.Item1 ? a.Item1.CompareTo(b.Item1) : a.Item2.CompareTo(b.Item2)); // Сортируем список координат: сначала по x, затем по y.

                int[] ans = { 0, 0 }; // Инициализируем массив для хранения результата (ряд и место).

                // Проходим по всем координатам занятых мест
                for (int i = 0; i < n - 1; i++) // Цикл для перебора всех координат занятых мест, кроме последней.
                {
                    // Проверяем, совпадает ли ряд и есть ли 2 свободных места между ними
                    if (coordinates[i].Item1 == coordinates[i + 1].Item1 && coordinates[i].Item2 + 3 == coordinates[i + 1].Item2) // Если ряд совпадает и между местами есть 2 свободных места.
                    {
                        int row = coordinates[i].Item1; // Сохраняем номер ряда.
                        int seat = coordinates[i].Item2 + 1; // Вычисляем минимальное свободное место между двумя занятыми местами.
                        if (row > ans[0]) // Если текущий ряд больше, чем уже найденный ряд в ans.
                        {
                            ans[0] = row; // Обновляем ряд в ans.
                            ans[1] = seat; // Обновляем место в ans.
                        }
                    }
                }

                // Выводим результат
                Console.WriteLine($"{ans[0]} {ans[1]}"); // Выводим номер ряда и места, которые можно занять.
            }
        }
        catch (FileNotFoundException ex) // Обработка исключения, если файл не найден.
        {
            Console.WriteLine("Ошибка: Файл не найден. Проверьте путь к файлу."); // Выводим сообщение об ошибке.
        }
        catch (IOException ex) // Обработка исключения ввода-вывода.
        {
            Console.WriteLine("Ошибка ввода-вывода: " + ex.Message); // Выводим сообщение об ошибке.
        }
        catch (FormatException ex) // Обработка исключения формата.
        {
            Console.WriteLine("Ошибка формата: Проверьте, что данные в файле корректны."); // Выводим сообщение об ошибке.
        }
        catch (Exception ex) // Обработка всех других исключений.
        {
            Console.WriteLine("Произошла ошибка: " + ex.Message); // Выводим сообщение об ошибке.
        }
    }
}
