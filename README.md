# C0lddBox

sudo netdiscover -r 10.0.2.0/24
<img width="666" height="244" alt="image" src="https://github.com/user-attachments/assets/e0eaa15b-cee8-47d4-ad5e-097897db254c" />

10.0.2.2
10.0.2.4 - наш

sudo nmap -A --reason 10.0.2.4
sudo nmap -sV --reason 10.0.2.12
<img width="861" height="612" alt="image" src="https://github.com/user-attachments/assets/bbd19263-c3fd-477c-8157-10466bbd05f3" />

У браузері відкриваємо http://10.0.2.4, отримуємо:
<img width="1919" height="848" alt="image" src="https://github.com/user-attachments/assets/70d900c4-1395-409c-ac9f-8f8d5a09224f" />

Скористуємось login та отримуємо типове запрошення від wordpress. Цей спосіб може використовуватись як один із варіантів входу у систему, але для цього потрібно мати логін та пароль.
<img width="681" height="638" alt="image" src="https://github.com/user-attachments/assets/94fd2089-b958-4e1f-a464-a9bf3eeb2783" />

Проведемо дослідження структури вебсайту за допомогою gobuster:
gobuster dir -u http://10.0.2.4/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
<img width="830" height="422" alt="image" src="https://github.com/user-attachments/assets/7f88e2ab-d78e-42c2-9b4e-aaac3f630ea4" />

