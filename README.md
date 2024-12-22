# DrWeb-CTF2024-EverlastingReverse
Writeup

![image](https://github.com/user-attachments/assets/87dfe27d-01e8-46a0-b923-2f2686728d4a)
В архиве лежит exeшник и файл с расширением .pck
Запускаем приложение, видим, что перед нами программа, написанная на godot движке:
![godotConsole](https://github.com/user-attachments/assets/1eaa2b09-f5e9-41e5-8046-4ec6b53a1e58)


![image](https://github.com/user-attachments/assets/f28aff06-07cc-40de-b27f-9079a4318747)
Т.к. перед нами Godot, попробуем восстановить все файлы и сам проект из архива game.pck через GDRE Tools
Однако архив запаролен... Требуется найти 256 битный ключ(32 байта)

В описании задания есть подсказка на то, что мы имеем дело с опенсорсным проектом. Так оно и есть - исходный код движка godot есть на гитхабе:

Параллельно гуглим запросы по типу: "how to extract the project key from godot application"
1я ссылка наталкивает нас на статью о ненадежности функции open_and_parse, которая открывает наш защищенный .pck файл, используя ключ в открытом виде.
![image](https://github.com/user-attachments/assets/2fa85b5e-b177-4077-92eb-4af380d7e7fe)

Находим исходный код функции на гитхабе. Отсюда можно взять строки, по которым можно найти эту функцию в коде программы.
![image](https://github.com/user-attachments/assets/8f408308-5dc4-41c2-a7f7-ecf778669842)

Статический анализ.
Detect It Easy дает нам понять, что программа упакована(энтропия + нестандартное имя секции файла):
![image](https://github.com/user-attachments/assets/661053d0-0ec6-45d7-b165-c9f6d23f58a8)

Благо анти-дампа нет(установленно опытным путем), дампим прогу, к примеру, с помощью scylla-plugin.
После снятия дампа, находим по строкам искомую функцию и вычисляем ее оффсет относительно базы. Оффсет равен 0xb00ef0.
![image](https://github.com/user-attachments/assets/acc82f9f-c72f-44b0-b274-aadb44a1f62c)

Динамический анализ.
Попробуем протрассировать вызов API функций, используемых приложением.



