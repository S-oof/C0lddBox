# C0lddBox
Отримання IP-адресу цільової машини:
sudo netdiscover -r 10.0.2.0/24
<img width="666" height="244" alt="image" src="https://github.com/user-attachments/assets/e0eaa15b-cee8-47d4-ad5e-097897db254c" />

10.0.2.2
10.0.2.4 - наш

Сканування відкритих портів цільової машини:
sudo nmap -A --reason 10.0.2.4
або
sudo nmap -sV --reason 10.0.2.4
<img width="861" height="612" alt="image" src="https://github.com/user-attachments/assets/bbd19263-c3fd-477c-8157-10466bbd05f3" />

У браузері відкриваємо http://10.0.2.4, отримуємо:
<img width="1919" height="848" alt="image" src="https://github.com/user-attachments/assets/70d900c4-1395-409c-ac9f-8f8d5a09224f" />

Скористуємось login та отримуємо типове запрошення від wordpress. Цей спосіб може використовуватись як один із варіантів входу у систему, але для цього потрібно мати логін та пароль.
<img width="681" height="638" alt="image" src="https://github.com/user-attachments/assets/94fd2089-b958-4e1f-a464-a9bf3eeb2783" />

Проведемо дослідження структури вебсайту за допомогою gobuster:
gobuster dir -u http://10.0.2.4/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
<img width="830" height="422" alt="image" src="https://github.com/user-attachments/assets/7f88e2ab-d78e-42c2-9b4e-aaac3f630ea4" />

Перевірка доступних посилань дає результат лише по останньому, то маємо такий результат:
<img width="1728" height="330" alt="image" src="https://github.com/user-attachments/assets/e415c859-bb41-4f5e-adf0-7b27da3e76b2" />

Це повідомлення дає змогу визначити нам трьох користувачів: C0ldd, Hugo, Phillip. Потрібно більш детально дослідити Wordpress на наявність додаткової інформації. Використаємо команду
wpscan --url http://10.0.2.4/ -e
<img width="793" height="327" alt="image" src="https://github.com/user-attachments/assets/4ccc3699-08b6-47aa-88da-edeb69142f2d" />

wpscan --url http://10.0.2.4/ -U c0ldd -P /usr/share/wordlists/rockyou.txt
<img width="379" height="65" alt="image" src="https://github.com/user-attachments/assets/8272537a-5f31-4e30-9bb9-2f346bad4818" />

Далі робимо логін в вордпрес та шукаємо можливості впровадження коду для створення реверс-шелу. Для цього переходимо в Apearence->Editor.
<img width="997" height="757" alt="image" src="https://github.com/user-attachments/assets/a4b9846a-9c5e-45bc-8a1d-52772f00a7b7" />

Наступним кроком маємо встановити, який з темплейтів ми зможемо модифікувти таким чином, щоб отримати реверс-шел. Для прикладу, скористаємось існуючим репозіторієм на ГітХабі: https://github.com/pentestmonkey/php-reverse-shell
<img width="1884" height="784" alt="image" src="https://github.com/user-attachments/assets/d01dd31a-0119-4681-9e82-c2a4dd38dab7" />

Зазвичай, код для реверс-шелу може бути доданий або в шаблон "Footer", або в "404". То як раз і почнемо з "Footer".
<img width="1519" height="788" alt="image" src="https://github.com/user-attachments/assets/67c8d84a-48f3-4920-a166-5416245603b5" />

Замінюємо код у "footer.php" на код з "php-reverse-shell.php", та додаємо кастомізацію.
<img width="466" height="248" alt="image" src="https://github.com/user-attachments/assets/1a2fe4c8-623a-4b78-b323-587185af9f6d" />

В терміналі Калі виконуємо наступну команду:
nc -lvnp 3421
Далі робимо оновлення основної сторінки вебсервера та маємо наступний результат:
<img width="950" height="212" alt="image" src="https://github.com/user-attachments/assets/43768ec2-5932-411c-aa61-e7364384472b" />

Зараз, коли ми знаходимося всередині сервера, давайте пошукаємо файли, які містять важливу інформацію. На вебсайті WordPress існує основний файл, що містить базові конфігураційні дані сайту, і він називається wp-config.php. Ми можемо знайти цей файл у /var/www/html
<img width="613" height="218" alt="image" src="https://github.com/user-attachments/assets/ee8e24b4-c392-4093-85b6-21a25f260fa1" />

Передивляємось зміст файлу wp-config.php за допомогою утіліти cat:
<img width="463" height="183" alt="image" src="https://github.com/user-attachments/assets/02d04820-d2a4-4867-96f6-8cb67507cbfc" />

Спробуємо зайти в сисетму під користовачем "c0dd" зі знайденим паролем:

su c0ldd

отримуємо результат:
<img width="461" height="160" alt="image" src="https://github.com/user-attachments/assets/bed476c0-c854-4d83-bcce-dbfd5682ea3e" />

перейдемо до домашнього каталогу користувача та дослідемо його зміст, та виведемо на термінал зміст файлу, що знайдено:
<img width="458" height="166" alt="image" src="https://github.com/user-attachments/assets/9922f4e5-0978-4dce-9c74-2d7a7e77b45a" />

Декодуємо за допомогою www.base64decode.org та отримуємо такий результат-привітання:
<img width="585" height="483" alt="image" src="https://github.com/user-attachments/assets/1d8f82a3-ac11-4507-bd9b-b7ab350a9e2e" />

Визначимо, які саме повноваження є за допомогою команди:
<img width="770" height="221" alt="image" src="https://github.com/user-attachments/assets/8e080548-7967-4808-b937-d16d0a30d3da" />
Ми можемо підвищити свої привілеї до root, використовуючи будь-яку з цих трьох команд.

#### Використання chmod
Небезпека полягає в тому, що якщо цьому бінарному файлу дозволено виконуватися від імені суперкористувача через sudo, він не скидає підвищені привілеї й може бути використаний для доступу до файлової системи, підвищення привілеїв або збереження привілейованого доступу.
Скористаємось методом, щоб змінити права доступу до файлу, який обмежений для користувача з низькими привілеями, і зробити його доступним для всіх користувачів. У нашому випадку файл, до якого потрібно отримати доступ, — це root-файл. Зробимо це за допомогою команди:
LFILE=root
Після цього виконаємо команду:
sudo chmod 6777 $LFILE
<img width="574" height="332" alt="image" src="https://github.com/user-attachments/assets/65cd0f9c-ad10-432c-9792-6d683be2e1ed" />

Декодуємо base64 за допомогою командного рядку:
<img width="498" height="528" alt="image" src="https://github.com/user-attachments/assets/feec5de3-7eb8-46b8-b5e9-a26c50af4b19" />

Завдання виконано.














