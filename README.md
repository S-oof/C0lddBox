# C0lddBox

sudo netdiscover -r 10.0.2.0/24
<img width="666" height="244" alt="image" src="https://github.com/user-attachments/assets/e0eaa15b-cee8-47d4-ad5e-097897db254c" />

10.0.2.2
10.0.2.4 - наш

sudo nmap -A --reason 10.0.2.4
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


<img width="1519" height="788" alt="image" src="https://github.com/user-attachments/assets/67c8d84a-48f3-4920-a166-5416245603b5" />


<img width="466" height="248" alt="image" src="https://github.com/user-attachments/assets/1a2fe4c8-623a-4b78-b323-587185af9f6d" />

<img width="950" height="212" alt="image" src="https://github.com/user-attachments/assets/43768ec2-5932-411c-aa61-e7364384472b" />









