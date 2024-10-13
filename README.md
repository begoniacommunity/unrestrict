Потребуются ПК на Windows с установленными WinSCP, PuTTY или [Windows Terminal;](https://apps.microsoft.com/detail/9n0dx20hk701?hl=ru-ru&gl=NL) чистый VDS/VPS с Ubuntu.  
**Настоятельно рекомендуется** полностью ознакомиться с инструкцией прежде, чем будете делать изложенное. Есть два варианта действия – ручной и полуавтоматический.

## I. Настройка сервера 
### Ручной метод
1. Подключаемся к серверу через SSH в WinSCP. Запускаем программу, вводим:  
Имя хоста: IP-адрес вашего сервера, указанного в ЛК вашего хостера  
Порт: 22 (по умолчанию)  
Имя пользователя: root (или другой пользователь, согласно описанию приобретённой услуги в ЛК хостера)
Пароль: пароль от вашего сервера, который находится на странице услуги  
Нажимаем Войти. Во всплывающем окне с хэшем хостового ключа, ещё чем-либо – всё подтверждаем. Справа появится файловая система сервера.
2. Теперь, в верхнем тулбаре, выбираем кнопку Открыть сессию в PuTTY. Введите пароль от сервера, затем выполните `apt update && apt upgrade`.
3. Выполните `ufw status` – вывод должен быть inactive.
4. Придумайте или [сгенерируйте](https://www.random.org/) случайное число от 1 до 65535 – это будет ваш новый порт подключения по SSH. Выше – лучше.
5. Выполните `ufw allow [число порта для SSH]`.
6. В WinSCP перейдите по пути /etc/ssh/sshd_config, раскомментируйте строку `#Port 22` (т.е. удалите #) и замените число 22 на выбранное вами число.
7. Выполните команду `systemctl restart ssh && systemctl restart networking && ufw enable`. 
После этого сессия в WinSCP отвалится, потому что изменились данные подключения. Сессия в PuTTY останется живой. Теперь ваш порт подключения к серверу по SSH сменился на указанное вами число, соответственно, для повторных будущих подключений вписывайте новый порт (вместо стандартного 22).
8. Переавторизуемся в WinSCP и проходим по пути /etc/ufw и открываем Правкой (ПКМ → Правка) файл before.rules.
Листаем до раздела последнего комментария и перед строкой COMMIT добавляем:

<details>
<summary>Ограничение ГРЧЦ</summary>

```
# Restrict GRCHC
-A ufw-before-input -s 109.124.119.88/29 -j DROP
-A ufw-before-input -s 109.124.66.128/30 -j DROP
-A ufw-before-input -s 109.124.66.160/28 -j DROP
-A ufw-before-input -s 109.124.71.64/29 -j DROP
-A ufw-before-input -s 109.124.78.108/30 -j DROP
-A ufw-before-input -s 109.124.80.132/30 -j DROP
-A ufw-before-input -s 109.124.83.20/30 -j DROP
-A ufw-before-input -s 109.124.87.96/29 -j DROP
-A ufw-before-input -s 109.124.89.140/30 -j DROP
-A ufw-before-input -s 109.124.89.212/30 -j DROP
-A ufw-before-input -s 109.124.89.36/30 -j DROP
-A ufw-before-input -s 109.124.90.128/30 -j DROP
-A ufw-before-input -s 109.124.90.32/30 -j DROP
-A ufw-before-input -s 109.124.97.4/30 -j DROP
-A ufw-before-input -s 109.124.99.16/30 -j DROP
-A ufw-before-input -s 109.124.99.160/28 -j DROP
-A ufw-before-input -s 109.204.204.232/29 -j DROP
-A ufw-before-input -s 109.207.0.0/20 -j DROP
-A ufw-before-input -s 109.232.187.16/29 -j DROP
-A ufw-before-input -s 109.248.197.0/24 -j DROP
-A ufw-before-input -s 109.73.4.224/27 -j DROP
-A ufw-before-input -s 145.255.238.240/28 -j DROP
-A ufw-before-input -s 149.62.55.240/30 -j DROP
-A ufw-before-input -s 176.109.0.0/21 -j DROP
-A ufw-before-input -s 176.116.96.0/20 -j DROP
-A ufw-before-input -s 178.16.156.148/30 -j DROP
-A ufw-before-input -s 178.237.206.0/24 -j DROP
-A ufw-before-input -s 178.237.240.0/20 -j DROP
-A ufw-before-input -s 178.237.248.0/21 -j DROP
-A ufw-before-input -s 178.248.232.137/32 -j DROP
-A ufw-before-input -s 178.248.232.46/32 -j DROP
-A ufw-before-input -s 178.248.232.60/32 -j DROP
-A ufw-before-input -s 178.248.233.136/32 -j DROP
-A ufw-before-input -s 178.248.233.244/32 -j DROP
-A ufw-before-input -s 178.248.233.245/32 -j DROP
-A ufw-before-input -s 178.248.233.26/32 -j DROP
-A ufw-before-input -s 178.248.233.32/32 -j DROP
-A ufw-before-input -s 178.248.233.60/32 -j DROP
-A ufw-before-input -s 178.248.234.136/32 -j DROP
-A ufw-before-input -s 178.248.234.204/32 -j DROP
-A ufw-before-input -s 178.248.234.228/32 -j DROP
-A ufw-before-input -s 178.248.234.238/32 -j DROP
-A ufw-before-input -s 178.248.234.30/32 -j DROP
-A ufw-before-input -s 178.248.234.33/32 -j DROP
-A ufw-before-input -s 178.248.234.53/32 -j DROP
-A ufw-before-input -s 178.248.234.60/32 -j DROP
-A ufw-before-input -s 178.248.234.79/32 -j DROP
-A ufw-before-input -s 178.248.234.83/32 -j DROP
-A ufw-before-input -s 178.248.235.244/32 -j DROP
-A ufw-before-input -s 178.248.235.53/32 -j DROP
-A ufw-before-input -s 178.248.235.60/32 -j DROP
-A ufw-before-input -s 178.248.235.75/32 -j DROP
-A ufw-before-input -s 178.248.236.20/32 -j DROP
-A ufw-before-input -s 178.248.236.244/32 -j DROP
-A ufw-before-input -s 178.248.236.83/32 -j DROP
-A ufw-before-input -s 178.248.237.136/32 -j DROP
-A ufw-before-input -s 178.248.237.18/32 -j DROP
-A ufw-before-input -s 178.248.237.242/32 -j DROP
-A ufw-before-input -s 178.248.237.47/32 -j DROP
-A ufw-before-input -s 178.248.237.98/32 -j DROP
-A ufw-before-input -s 178.248.238.102/32 -j DROP
-A ufw-before-input -s 178.248.238.128/32 -j DROP
-A ufw-before-input -s 178.248.238.129/32 -j DROP
-A ufw-before-input -s 178.248.238.136/32 -j DROP
-A ufw-before-input -s 178.248.238.155/32 -j DROP
-A ufw-before-input -s 178.248.238.172/32 -j DROP
-A ufw-before-input -s 178.248.238.205/32 -j DROP
-A ufw-before-input -s 178.248.238.255/32 -j DROP
-A ufw-before-input -s 178.248.238.55/32 -j DROP
-A ufw-before-input -s 178.248.239.110/32 -j DROP
-A ufw-before-input -s 178.248.239.215/32 -j DROP
-A ufw-before-input -s 178.49.148.176/29 -j DROP
-A ufw-before-input -s 185.149.160.0/24 -j DROP
-A ufw-before-input -s 185.149.161.0/24 -j DROP
-A ufw-before-input -s 185.149.162.0/24 -j DROP
-A ufw-before-input -s 185.149.163.0/24 -j DROP
-A ufw-before-input -s 185.168.60.0/24 -j DROP
-A ufw-before-input -s 185.168.61.0/24 -j DROP
-A ufw-before-input -s 185.168.62.0/24 -j DROP
-A ufw-before-input -s 185.168.63.0/24 -j DROP
-A ufw-before-input -s 185.179.224.0/24 -j DROP
-A ufw-before-input -s 185.179.225.0/24 -j DROP
-A ufw-before-input -s 185.179.226.0/24 -j DROP
-A ufw-before-input -s 185.179.227.0/24 -j DROP
-A ufw-before-input -s 185.183.172.0/23 -j DROP
-A ufw-before-input -s 185.183.174.0/23 -j DROP
-A ufw-before-input -s 185.224.228.0/24 -j DROP
-A ufw-before-input -s 185.224.229.0/24 -j DROP
-A ufw-before-input -s 185.224.230.0/24 -j DROP
-A ufw-before-input -s 185.224.231.0/24 -j DROP
-A ufw-before-input -s 185.65.149.170/32 -j DROP
-A ufw-before-input -s 185.7.234.188/30 -j DROP
-A ufw-before-input -s 188.128.101.108/30 -j DROP
-A ufw-before-input -s 188.128.11.196/30 -j DROP
-A ufw-before-input -s 188.128.112.216/29 -j DROP
-A ufw-before-input -s 188.128.112.240/29 -j DROP
-A ufw-before-input -s 188.128.113.0/28 -j DROP
-A ufw-before-input -s 188.128.114.128/28 -j DROP
-A ufw-before-input -s 188.128.115.232/29 -j DROP
-A ufw-before-input -s 188.128.118.224/27 -j DROP
-A ufw-before-input -s 188.128.119.104/30 -j DROP
-A ufw-before-input -s 188.128.8.240/30 -j DROP
-A ufw-before-input -s 188.128.89.0/30 -j DROP
-A ufw-before-input -s 188.128.92.104/30 -j DROP
-A ufw-before-input -s 188.128.94.204/30 -j DROP
-A ufw-before-input -s 188.128.98.204/30 -j DROP
-A ufw-before-input -s 188.247.36.124/30 -j DROP
-A ufw-before-input -s 188.247.36.128/30 -j DROP
-A ufw-before-input -s 188.247.36.132/30 -j DROP
-A ufw-before-input -s 188.247.36.136/30 -j DROP
-A ufw-before-input -s 188.247.36.140/30 -j DROP
-A ufw-before-input -s 188.247.36.204/30 -j DROP
-A ufw-before-input -s 193.232.70.0/24 -j DROP
-A ufw-before-input -s 193.47.146.0/24 -j DROP
-A ufw-before-input -s 194.140.247.0/25 -j DROP
-A ufw-before-input -s 194.140.247.128/25 -j DROP
-A ufw-before-input -s 194.150.202.0/23 -j DROP
-A ufw-before-input -s 194.165.22.0/23 -j DROP
-A ufw-before-input -s 194.186.112.80/28 -j DROP
-A ufw-before-input -s 194.190.9.0/24 -j DROP
-A ufw-before-input -s 194.215.248.0/24 -j DROP
-A ufw-before-input -s 194.226.116.0/22 -j DROP
-A ufw-before-input -s 194.226.127.0/24 -j DROP
-A ufw-before-input -s 194.226.80.0/21 -j DROP
-A ufw-before-input -s 194.226.88.0/21 -j DROP
-A ufw-before-input -s 194.67.63.200/30 -j DROP
-A ufw-before-input -s 194.8.246.0/23 -j DROP
-A ufw-before-input -s 194.8.70.0/23 -j DROP
-A ufw-before-input -s 195.128.157.0/24 -j DROP
-A ufw-before-input -s 195.131.53.248/29 -j DROP
-A ufw-before-input -s 195.131.61.80/29 -j DROP
-A ufw-before-input -s 195.131.63.24/29 -j DROP
-A ufw-before-input -s 195.131.7.8/29 -j DROP
-A ufw-before-input -s 195.144.232.144/30 -j DROP
-A ufw-before-input -s 195.144.240.128/28 -j DROP
-A ufw-before-input -s 195.149.110.0/24 -j DROP
-A ufw-before-input -s 195.151.25.48/29 -j DROP
-A ufw-before-input -s 195.16.55.224/27 -j DROP
-A ufw-before-input -s 195.162.36.64/28 -j DROP
-A ufw-before-input -s 195.170.218.24/29 -j DROP
-A ufw-before-input -s 195.170.218.88/29 -j DROP
-A ufw-before-input -s 195.182.142.128/26 -j DROP
-A ufw-before-input -s 195.182.145.64/28 -j DROP
-A ufw-before-input -s 195.182.151.212/30 -j DROP
-A ufw-before-input -s 195.182.151.216/30 -j DROP
-A ufw-before-input -s 195.182.155.164/30 -j DROP
-A ufw-before-input -s 195.182.156.96/30 -j DROP
-A ufw-before-input -s 195.209.120.0/22 -j DROP
-A ufw-before-input -s 195.209.120.0/24 -j DROP
-A ufw-before-input -s 195.209.121.0/24 -j DROP
-A ufw-before-input -s 195.209.122.0/24 -j DROP
-A ufw-before-input -s 195.209.123.0/24 -j DROP
-A ufw-before-input -s 195.218.175.40/29 -j DROP
-A ufw-before-input -s 195.239.113.0/24 -j DROP
-A ufw-before-input -s 195.3.240.0/22 -j DROP
-A ufw-before-input -s 195.42.75.8/29 -j DROP
-A ufw-before-input -s 195.54.20.168/29 -j DROP
-A ufw-before-input -s 195.54.221.0/24 -j DROP
-A ufw-before-input -s 195.54.28.72/30 -j DROP
-A ufw-before-input -s 195.58.13.120/30 -j DROP
-A ufw-before-input -s 195.58.21.196/30 -j DROP
-A ufw-before-input -s 195.58.29.57/32 -j DROP
-A ufw-before-input -s 195.58.30.164/30 -j DROP
-A ufw-before-input -s 195.58.30.200/29 -j DROP
-A ufw-before-input -s 195.58.5.16/30 -j DROP
-A ufw-before-input -s 195.58.5.20/30 -j DROP
-A ufw-before-input -s 195.80.224.0/24 -j DROP
-A ufw-before-input -s 195.98.38.16/28 -j DROP
-A ufw-before-input -s 195.98.43.104/29 -j DROP
-A ufw-before-input -s 195.98.73.56/29 -j DROP
-A ufw-before-input -s 195.98.77.100/30 -j DROP
-A ufw-before-input -s 212.119.174.0/24 -j DROP
-A ufw-before-input -s 212.119.175.0/24 -j DROP
-A ufw-before-input -s 212.120.169.48/29 -j DROP
-A ufw-before-input -s 212.120.174.88/29 -j DROP
-A ufw-before-input -s 212.120.184.48/29 -j DROP
-A ufw-before-input -s 212.120.184.56/29 -j DROP
-A ufw-before-input -s 212.120.184.64/29 -j DROP
-A ufw-before-input -s 212.120.189.208/29 -j DROP
-A ufw-before-input -s 212.120.189.224/29 -j DROP
-A ufw-before-input -s 212.120.190.112/29 -j DROP
-A ufw-before-input -s 212.120.190.240/29 -j DROP
-A ufw-before-input -s 212.120.191.120/29 -j DROP
-A ufw-before-input -s 212.120.191.248/29 -j DROP
-A ufw-before-input -s 212.13.104.116/30 -j DROP
-A ufw-before-input -s 212.13.113.100/30 -j DROP
-A ufw-before-input -s 212.15.105.64/28 -j DROP
-A ufw-before-input -s 212.15.114.156/30 -j DROP
-A ufw-before-input -s 212.15.115.80/28 -j DROP
-A ufw-before-input -s 212.17.16.192/27 -j DROP
-A ufw-before-input -s 212.17.17.176/28 -j DROP
-A ufw-before-input -s 212.17.8.176/29 -j DROP
-A ufw-before-input -s 212.17.9.144/28 -j DROP
-A ufw-before-input -s 212.192.156.0/22 -j DROP
-A ufw-before-input -s 212.192.156.0/24 -j DROP
-A ufw-before-input -s 212.192.157.0/24 -j DROP
-A ufw-before-input -s 212.192.158.0/24 -j DROP
-A ufw-before-input -s 212.23.85.48/30 -j DROP
-A ufw-before-input -s 212.23.85.56/29 -j DROP
-A ufw-before-input -s 212.32.198.64/29 -j DROP
-A ufw-before-input -s 212.48.134.192/26 -j DROP
-A ufw-before-input -s 212.48.138.240/28 -j DROP
-A ufw-before-input -s 212.48.141.160/27 -j DROP
-A ufw-before-input -s 212.48.34.176/29 -j DROP
-A ufw-before-input -s 212.48.53.100/30 -j DROP
-A ufw-before-input -s 212.48.53.144/30 -j DROP
-A ufw-before-input -s 212.48.53.152/30 -j DROP
-A ufw-before-input -s 212.48.53.156/30 -j DROP
-A ufw-before-input -s 212.48.53.160/30 -j DROP
-A ufw-before-input -s 212.48.53.164/30 -j DROP
-A ufw-before-input -s 212.48.53.184/30 -j DROP
-A ufw-before-input -s 212.48.53.188/30 -j DROP
-A ufw-before-input -s 212.48.53.192/30 -j DROP
-A ufw-before-input -s 212.48.53.196/30 -j DROP
-A ufw-before-input -s 212.48.53.200/30 -j DROP
-A ufw-before-input -s 212.48.53.216/30 -j DROP
-A ufw-before-input -s 212.48.53.236/30 -j DROP
-A ufw-before-input -s 212.48.53.240/30 -j DROP
-A ufw-before-input -s 212.48.53.244/30 -j DROP
-A ufw-before-input -s 212.48.53.248/30 -j DROP
-A ufw-before-input -s 212.48.53.252/30 -j DROP
-A ufw-before-input -s 212.48.53.76/30 -j DROP
-A ufw-before-input -s 212.48.53.84/30 -j DROP
-A ufw-before-input -s 212.48.53.88/30 -j DROP
-A ufw-before-input -s 212.48.53.92/30 -j DROP
-A ufw-before-input -s 212.48.54.0/30 -j DROP
-A ufw-before-input -s 212.48.54.100/30 -j DROP
-A ufw-before-input -s 212.48.54.104/30 -j DROP
-A ufw-before-input -s 212.48.54.108/30 -j DROP
-A ufw-before-input -s 212.48.54.112/30 -j DROP
-A ufw-before-input -s 212.48.54.116/30 -j DROP
-A ufw-before-input -s 212.48.54.12/30 -j DROP
-A ufw-before-input -s 212.48.54.120/30 -j DROP
-A ufw-before-input -s 212.48.54.124/30 -j DROP
-A ufw-before-input -s 212.48.54.128/30 -j DROP
-A ufw-before-input -s 212.48.54.132/30 -j DROP
-A ufw-before-input -s 212.48.54.136/30 -j DROP
-A ufw-before-input -s 212.48.54.140/30 -j DROP
-A ufw-before-input -s 212.48.54.144/30 -j DROP
-A ufw-before-input -s 212.48.54.148/30 -j DROP
-A ufw-before-input -s 212.48.54.152/30 -j DROP
-A ufw-before-input -s 212.48.54.156/30 -j DROP
-A ufw-before-input -s 212.48.54.16/30 -j DROP
-A ufw-before-input -s 212.48.54.164/30 -j DROP
-A ufw-before-input -s 212.48.54.168/30 -j DROP
-A ufw-before-input -s 212.48.54.172/30 -j DROP
-A ufw-before-input -s 212.48.54.176/30 -j DROP
-A ufw-before-input -s 212.48.54.180/30 -j DROP
-A ufw-before-input -s 212.48.54.184/30 -j DROP
-A ufw-before-input -s 212.48.54.188/30 -j DROP
-A ufw-before-input -s 212.48.54.196/30 -j DROP
-A ufw-before-input -s 212.48.54.20/30 -j DROP
-A ufw-before-input -s 212.48.54.200/30 -j DROP
-A ufw-before-input -s 212.48.54.208/30 -j DROP
-A ufw-before-input -s 212.48.54.212/30 -j DROP
-A ufw-before-input -s 212.48.54.216/30 -j DROP
-A ufw-before-input -s 212.48.54.220/30 -j DROP
-A ufw-before-input -s 212.48.54.24/30 -j DROP
-A ufw-before-input -s 212.48.54.240/30 -j DROP
-A ufw-before-input -s 212.48.54.244/30 -j DROP
-A ufw-before-input -s 212.48.54.248/30 -j DROP
-A ufw-before-input -s 212.48.54.252/30 -j DROP
-A ufw-before-input -s 212.48.54.28/30 -j DROP
-A ufw-before-input -s 212.48.54.32/30 -j DROP
-A ufw-before-input -s 212.48.54.36/30 -j DROP
-A ufw-before-input -s 212.48.54.40/30 -j DROP
-A ufw-before-input -s 212.48.54.44/30 -j DROP
-A ufw-before-input -s 212.48.54.48/30 -j DROP
-A ufw-before-input -s 212.48.54.52/30 -j DROP
-A ufw-before-input -s 212.48.54.56/30 -j DROP
-A ufw-before-input -s 212.48.54.60/30 -j DROP
-A ufw-before-input -s 212.48.54.64/30 -j DROP
-A ufw-before-input -s 212.48.54.68/30 -j DROP
-A ufw-before-input -s 212.48.54.72/30 -j DROP
-A ufw-before-input -s 212.48.54.76/30 -j DROP
-A ufw-before-input -s 212.48.54.8/30 -j DROP
-A ufw-before-input -s 212.48.54.80/30 -j DROP
-A ufw-before-input -s 212.48.54.84/30 -j DROP
-A ufw-before-input -s 212.48.54.88/30 -j DROP
-A ufw-before-input -s 212.48.54.92/30 -j DROP
-A ufw-before-input -s 212.48.54.96/30 -j DROP
-A ufw-before-input -s 212.49.107.224/27 -j DROP
-A ufw-before-input -s 212.49.124.0/26 -j DROP
-A ufw-before-input -s 212.57.133.0/24 -j DROP
-A ufw-before-input -s 212.57.159.0/24 -j DROP
-A ufw-before-input -s 212.59.98.48/29 -j DROP
-A ufw-before-input -s 212.59.99.96/27 -j DROP
-A ufw-before-input -s 213.172.17.252/30 -j DROP
-A ufw-before-input -s 213.172.18.124/30 -j DROP
-A ufw-before-input -s 213.172.18.148/30 -j DROP
-A ufw-before-input -s 213.172.18.160/30 -j DROP
-A ufw-before-input -s 213.172.18.164/30 -j DROP
-A ufw-before-input -s 213.172.18.252/30 -j DROP
-A ufw-before-input -s 213.172.18.60/30 -j DROP
-A ufw-before-input -s 213.172.27.0/30 -j DROP
-A ufw-before-input -s 213.172.27.116/30 -j DROP
-A ufw-before-input -s 213.172.27.160/30 -j DROP
-A ufw-before-input -s 213.172.27.204/30 -j DROP
-A ufw-before-input -s 213.172.27.212/30 -j DROP
-A ufw-before-input -s 213.172.27.224/30 -j DROP
-A ufw-before-input -s 213.172.27.252/30 -j DROP
-A ufw-before-input -s 213.172.30.136/30 -j DROP
-A ufw-before-input -s 213.177.111.0/24 -j DROP
-A ufw-before-input -s 213.183.253.56/29 -j DROP
-A ufw-before-input -s 213.219.237.68/30 -j DROP
-A ufw-before-input -s 213.234.13.60/30 -j DROP
-A ufw-before-input -s 213.234.15.228/30 -j DROP
-A ufw-before-input -s 213.234.15.248/30 -j DROP
-A ufw-before-input -s 213.234.18.52/30 -j DROP
-A ufw-before-input -s 213.234.8.8/30 -j DROP
-A ufw-before-input -s 213.24.128.0/22 -j DROP
-A ufw-before-input -s 213.24.143.0/24 -j DROP
-A ufw-before-input -s 213.24.152.0/22 -j DROP
-A ufw-before-input -s 213.24.160.0/28 -j DROP
-A ufw-before-input -s 213.24.34.0/24 -j DROP
-A ufw-before-input -s 213.24.75.0/24 -j DROP
-A ufw-before-input -s 213.24.76.0/23 -j DROP
-A ufw-before-input -s 213.242.204.236/30 -j DROP
-A ufw-before-input -s 213.242.204.76/30 -j DROP
-A ufw-before-input -s 213.242.205.88/30 -j DROP
-A ufw-before-input -s 213.242.215.192/29 -j DROP
-A ufw-before-input -s 213.242.215.68/30 -j DROP
-A ufw-before-input -s 213.243.106.48/28 -j DROP
-A ufw-before-input -s 213.243.116.0/24 -j DROP
-A ufw-before-input -s 213.243.84.80/28 -j DROP
-A ufw-before-input -s 213.33.171.240/29 -j DROP
-A ufw-before-input -s 213.59.59.120/29 -j DROP
-A ufw-before-input -s 213.59.59.128/29 -j DROP
-A ufw-before-input -s 213.59.59.144/29 -j DROP
-A ufw-before-input -s 213.59.59.16/29 -j DROP
-A ufw-before-input -s 213.59.59.168/29 -j DROP
-A ufw-before-input -s 213.59.59.64/29 -j DROP
-A ufw-before-input -s 213.59.91.128/27 -j DROP
-A ufw-before-input -s 213.59.91.176/28 -j DROP
-A ufw-before-input -s 213.59.91.48/29 -j DROP
-A ufw-before-input -s 213.85.142.176/28 -j DROP
-A ufw-before-input -s 213.85.2.64/28 -j DROP
-A ufw-before-input -s 213.85.2.80/29 -j DROP
-A ufw-before-input -s 213.85.20.32/30 -j DROP
-A ufw-before-input -s 213.85.20.8/30 -j DROP
-A ufw-before-input -s 213.85.20.84/30 -j DROP
-A ufw-before-input -s 213.85.77.64/27 -j DROP
-A ufw-before-input -s 217.106.0.0/16 -j DROP
-A ufw-before-input -s 217.106.115.168/29 -j DROP
-A ufw-before-input -s 217.106.147.0/29 -j DROP
-A ufw-before-input -s 217.106.147.8/29 -j DROP
-A ufw-before-input -s 217.106.150.224/29 -j DROP
-A ufw-before-input -s 217.106.150.72/29 -j DROP
-A ufw-before-input -s 217.106.150.80/29 -j DROP
-A ufw-before-input -s 217.106.150.88/29 -j DROP
-A ufw-before-input -s 217.106.203.240/29 -j DROP
-A ufw-before-input -s 217.106.203.88/29 -j DROP
-A ufw-before-input -s 217.106.93.192/26 -j DROP
-A ufw-before-input -s 217.106.95.112/28 -j DROP
-A ufw-before-input -s 217.107.200.0/21 -j DROP
-A ufw-before-input -s 217.107.5.112/29 -j DROP
-A ufw-before-input -s 217.107.5.16/29 -j DROP
-A ufw-before-input -s 217.107.5.24/29 -j DROP
-A ufw-before-input -s 217.107.5.40/29 -j DROP
-A ufw-before-input -s 217.107.5.8/29 -j DROP
-A ufw-before-input -s 217.107.5.80/29 -j DROP
-A ufw-before-input -s 217.107.5.88/29 -j DROP
-A ufw-before-input -s 217.107.5.96/29 -j DROP
-A ufw-before-input -s 217.147.23.112/28 -j DROP
-A ufw-before-input -s 217.148.216.156/30 -j DROP
-A ufw-before-input -s 217.148.220.160/29 -j DROP
-A ufw-before-input -s 217.172.18.0/23 -j DROP
-A ufw-before-input -s 217.195.92.16/28 -j DROP
-A ufw-before-input -s 217.195.93.144/29 -j DROP
-A ufw-before-input -s 217.195.94.200/29 -j DROP
-A ufw-before-input -s 217.198.6.128/26 -j DROP
-A ufw-before-input -s 217.198.6.48/29 -j DROP
-A ufw-before-input -s 217.20.86.128/26 -j DROP
-A ufw-before-input -s 217.20.86.232/29 -j DROP
-A ufw-before-input -s 217.23.88.168/29 -j DROP
-A ufw-before-input -s 217.23.88.248/29 -j DROP
-A ufw-before-input -s 217.27.142.176/30 -j DROP
-A ufw-before-input -s 217.65.214.24/29 -j DROP
-A ufw-before-input -s 217.65.219.160/29 -j DROP
-A ufw-before-input -s 217.67.177.208/29 -j DROP
-A ufw-before-input -s 31.177.95.0/24 -j DROP
-A ufw-before-input -s 31.217.218.96/29 -j DROP
-A ufw-before-input -s 31.44.63.64/29 -j DROP
-A ufw-before-input -s 37.28.161.48/30 -j DROP
-A ufw-before-input -s 37.29.53.16/30 -j DROP
-A ufw-before-input -s 37.29.57.52/30 -j DROP
-A ufw-before-input -s 37.29.57.64/30 -j DROP
-A ufw-before-input -s 37.29.59.56/30 -j DROP
-A ufw-before-input -s 46.20.70.160/28 -j DROP
-A ufw-before-input -s 46.228.0.232/29 -j DROP
-A ufw-before-input -s 46.29.152.0/22 -j DROP
-A ufw-before-input -s 46.46.142.160/28 -j DROP
-A ufw-before-input -s 46.46.148.40/29 -j DROP
-A ufw-before-input -s 46.47.197.128/30 -j DROP
-A ufw-before-input -s 46.47.199.76/30 -j DROP
-A ufw-before-input -s 46.47.203.52/30 -j DROP
-A ufw-before-input -s 46.47.207.96/30 -j DROP
-A ufw-before-input -s 46.47.208.84/30 -j DROP
-A ufw-before-input -s 46.47.210.76/30 -j DROP
-A ufw-before-input -s 46.47.211.0/24 -j DROP
-A ufw-before-input -s 46.47.212.204/30 -j DROP
-A ufw-before-input -s 46.47.213.0/24 -j DROP
-A ufw-before-input -s 46.47.214.200/30 -j DROP
-A ufw-before-input -s 46.47.219.200/30 -j DROP
-A ufw-before-input -s 46.47.223.196/30 -j DROP
-A ufw-before-input -s 46.47.229.0/28 -j DROP
-A ufw-before-input -s 46.47.238.144/30 -j DROP
-A ufw-before-input -s 46.47.249.176/29 -j DROP
-A ufw-before-input -s 46.61.208.0/24 -j DROP
-A ufw-before-input -s 62.112.110.64/28 -j DROP
-A ufw-before-input -s 62.118.0.208/28 -j DROP
-A ufw-before-input -s 62.118.101.184/29 -j DROP
-A ufw-before-input -s 62.118.113.232/29 -j DROP
-A ufw-before-input -s 62.118.125.188/30 -j DROP
-A ufw-before-input -s 62.118.127.240/28 -j DROP
-A ufw-before-input -s 62.118.15.16/28 -j DROP
-A ufw-before-input -s 62.118.17.152/29 -j DROP
-A ufw-before-input -s 62.118.19.112/30 -j DROP
-A ufw-before-input -s 62.118.19.40/30 -j DROP
-A ufw-before-input -s 62.118.193.8/29 -j DROP
-A ufw-before-input -s 62.118.205.68/30 -j DROP
-A ufw-before-input -s 62.118.208.100/30 -j DROP
-A ufw-before-input -s 62.118.209.192/30 -j DROP
-A ufw-before-input -s 62.118.21.160/29 -j DROP
-A ufw-before-input -s 62.118.216.60/30 -j DROP
-A ufw-before-input -s 62.118.219.184/30 -j DROP
-A ufw-before-input -s 62.118.230.4/30 -j DROP
-A ufw-before-input -s 62.118.233.224/29 -j DROP
-A ufw-before-input -s 62.118.234.64/29 -j DROP
-A ufw-before-input -s 62.118.239.128/29 -j DROP
-A ufw-before-input -s 62.118.25.112/28 -j DROP
-A ufw-before-input -s 62.118.37.168/30 -j DROP
-A ufw-before-input -s 62.118.37.180/30 -j DROP
-A ufw-before-input -s 62.118.37.4/30 -j DROP
-A ufw-before-input -s 62.118.38.212/30 -j DROP
-A ufw-before-input -s 62.141.125.0/25 -j DROP
-A ufw-before-input -s 62.181.52.56/29 -j DROP
-A ufw-before-input -s 62.28.169.168/30 -j DROP
-A ufw-before-input -s 62.33.199.80/29 -j DROP
-A ufw-before-input -s 62.33.34.16/28 -j DROP
-A ufw-before-input -s 62.33.87.128/28 -j DROP
-A ufw-before-input -s 62.33.87.152/29 -j DROP
-A ufw-before-input -s 62.5.130.104/29 -j DROP
-A ufw-before-input -s 62.5.132.224/29 -j DROP
-A ufw-before-input -s 62.5.189.80/29 -j DROP
-A ufw-before-input -s 62.5.202.60/30 -j DROP
-A ufw-before-input -s 62.5.218.204/30 -j DROP
-A ufw-before-input -s 62.5.224.188/30 -j DROP
-A ufw-before-input -s 62.5.242.80/28 -j DROP
-A ufw-before-input -s 62.63.100.160/30 -j DROP
-A ufw-before-input -s 62.63.101.80/29 -j DROP
-A ufw-before-input -s 62.63.96.32/28 -j DROP
-A ufw-before-input -s 62.63.98.24/29 -j DROP
-A ufw-before-input -s 77.243.9.80/28 -j DROP
-A ufw-before-input -s 77.34.209.160/28 -j DROP
-A ufw-before-input -s 77.35.76.80/28 -j DROP
-A ufw-before-input -s 77.35.98.240/28 -j DROP
-A ufw-before-input -s 77.37.128.0/17 -j DROP
-A ufw-before-input -s 77.72.139.0/28 -j DROP
-A ufw-before-input -s 77.82.124.112/29 -j DROP
-A ufw-before-input -s 78.107.13.208/28 -j DROP
-A ufw-before-input -s 78.107.16.96/28 -j DROP
-A ufw-before-input -s 78.107.18.112/28 -j DROP
-A ufw-before-input -s 78.107.3.208/28 -j DROP
-A ufw-before-input -s 78.107.40.160/28 -j DROP
-A ufw-before-input -s 78.107.42.144/28 -j DROP
-A ufw-before-input -s 78.107.51.16/28 -j DROP
-A ufw-before-input -s 78.107.61.96/28 -j DROP
-A ufw-before-input -s 78.107.86.32/28 -j DROP
-A ufw-before-input -s 78.108.192.0/21 -j DROP
-A ufw-before-input -s 78.108.200.0/24 -j DROP
-A ufw-before-input -s 78.109.140.112/29 -j DROP
-A ufw-before-input -s 78.24.159.48/29 -j DROP
-A ufw-before-input -s 78.37.104.0/29 -j DROP
-A ufw-before-input -s 78.37.67.24/29 -j DROP
-A ufw-before-input -s 78.37.69.160/27 -j DROP
-A ufw-before-input -s 78.37.84.120/29 -j DROP
-A ufw-before-input -s 78.37.97.88/29 -j DROP
-A ufw-before-input -s 79.133.74.160/30 -j DROP
-A ufw-before-input -s 79.133.74.168/30 -j DROP
-A ufw-before-input -s 79.133.75.176/30 -j DROP
-A ufw-before-input -s 79.133.75.44/30 -j DROP
-A ufw-before-input -s 79.142.88.0/28 -j DROP
-A ufw-before-input -s 80.237.11.88/29 -j DROP
-A ufw-before-input -s 80.237.39.112/29 -j DROP
-A ufw-before-input -s 80.237.98.80/28 -j DROP
-A ufw-before-input -s 80.247.32.0/20 -j DROP
-A ufw-before-input -s 80.247.32.0/24 -j DROP
-A ufw-before-input -s 80.247.46.0/24 -j DROP
-A ufw-before-input -s 80.254.100.40/29 -j DROP
-A ufw-before-input -s 80.254.119.168/29 -j DROP
-A ufw-before-input -s 80.73.16.0/20 -j DROP
-A ufw-before-input -s 80.73.16.0/21 -j DROP
-A ufw-before-input -s 80.73.16.0/24 -j DROP
-A ufw-before-input -s 80.73.168.80/28 -j DROP
-A ufw-before-input -s 80.73.169.244/30 -j DROP
-A ufw-before-input -s 80.82.43.24/29 -j DROP
-A ufw-before-input -s 80.89.152.220/30 -j DROP
-A ufw-before-input -s 81.1.195.0/28 -j DROP
-A ufw-before-input -s 81.1.205.96/27 -j DROP
-A ufw-before-input -s 81.17.2.192/28 -j DROP
-A ufw-before-input -s 81.17.3.16/29 -j DROP
-A ufw-before-input -s 81.176.235.0/27 -j DROP
-A ufw-before-input -s 81.176.70.0/26 -j DROP
-A ufw-before-input -s 81.177.156.0/24 -j DROP
-A ufw-before-input -s 81.195.105.160/28 -j DROP
-A ufw-before-input -s 81.195.108.164/30 -j DROP
-A ufw-before-input -s 81.195.112.36/30 -j DROP
-A ufw-before-input -s 81.195.118.128/30 -j DROP
-A ufw-before-input -s 81.195.118.48/30 -j DROP
-A ufw-before-input -s 81.195.120.16/29 -j DROP
-A ufw-before-input -s 81.195.124.52/30 -j DROP
-A ufw-before-input -s 81.195.125.96/30 -j DROP
-A ufw-before-input -s 81.195.148.140/30 -j DROP
-A ufw-before-input -s 81.195.150.248/30 -j DROP
-A ufw-before-input -s 81.195.151.172/30 -j DROP
-A ufw-before-input -s 81.195.155.0/30 -j DROP
-A ufw-before-input -s 81.195.161.12/30 -j DROP
-A ufw-before-input -s 81.195.165.64/28 -j DROP
-A ufw-before-input -s 81.195.168.24/30 -j DROP
-A ufw-before-input -s 81.195.177.160/30 -j DROP
-A ufw-before-input -s 81.195.178.224/27 -j DROP
-A ufw-before-input -s 81.195.182.64/28 -j DROP
-A ufw-before-input -s 81.195.192.96/30 -j DROP
-A ufw-before-input -s 81.195.231.128/26 -j DROP
-A ufw-before-input -s 81.195.244.32/29 -j DROP
-A ufw-before-input -s 81.195.245.0/28 -j DROP
-A ufw-before-input -s 81.195.247.128/28 -j DROP
-A ufw-before-input -s 81.195.250.16/29 -j DROP
-A ufw-before-input -s 81.195.36.48/28 -j DROP
-A ufw-before-input -s 81.195.44.248/30 -j DROP
-A ufw-before-input -s 81.195.45.64/30 -j DROP
-A ufw-before-input -s 81.195.50.72/29 -j DROP
-A ufw-before-input -s 81.195.90.44/30 -j DROP
-A ufw-before-input -s 81.195.92.48/30 -j DROP
-A ufw-before-input -s 81.195.93.192/27 -j DROP
-A ufw-before-input -s 81.195.94.72/29 -j DROP
-A ufw-before-input -s 81.2.1.0/28 -j DROP
-A ufw-before-input -s 81.2.10.192/27 -j DROP
-A ufw-before-input -s 81.211.32.16/28 -j DROP
-A ufw-before-input -s 81.222.194.200/29 -j DROP
-A ufw-before-input -s 81.222.209.136/29 -j DROP
-A ufw-before-input -s 81.222.210.24/29 -j DROP
-A ufw-before-input -s 81.3.168.148/30 -j DROP
-A ufw-before-input -s 82.110.69.200/29 -j DROP
-A ufw-before-input -s 82.140.65.240/29 -j DROP
-A ufw-before-input -s 82.151.107.136/29 -j DROP
-A ufw-before-input -s 82.162.103.144/28 -j DROP
-A ufw-before-input -s 82.162.126.96/28 -j DROP
-A ufw-before-input -s 82.162.149.160/28 -j DROP
-A ufw-before-input -s 82.162.157.64/28 -j DROP
-A ufw-before-input -s 82.162.158.176/28 -j DROP
-A ufw-before-input -s 82.162.172.112/28 -j DROP
-A ufw-before-input -s 82.162.72.208/28 -j DROP
-A ufw-before-input -s 82.162.76.176/28 -j DROP
-A ufw-before-input -s 82.162.80.192/28 -j DROP
-A ufw-before-input -s 82.162.87.192/28 -j DROP
-A ufw-before-input -s 82.162.90.0/28 -j DROP
-A ufw-before-input -s 82.179.86.32/27 -j DROP
-A ufw-before-input -s 82.196.130.0/27 -j DROP
-A ufw-before-input -s 82.196.69.152/30 -j DROP
-A ufw-before-input -s 82.198.176.144/29 -j DROP
-A ufw-before-input -s 82.198.176.16/29 -j DROP
-A ufw-before-input -s 82.198.176.208/29 -j DROP
-A ufw-before-input -s 82.198.189.128/26 -j DROP
-A ufw-before-input -s 82.198.190.64/26 -j DROP
-A ufw-before-input -s 82.198.191.248/29 -j DROP
-A ufw-before-input -s 82.198.191.96/27 -j DROP
-A ufw-before-input -s 82.200.13.0/27 -j DROP
-A ufw-before-input -s 82.200.22.136/29 -j DROP
-A ufw-before-input -s 82.200.22.144/28 -j DROP
-A ufw-before-input -s 82.200.64.0/24 -j DROP
-A ufw-before-input -s 82.208.68.240/28 -j DROP
-A ufw-before-input -s 82.208.77.104/29 -j DROP
-A ufw-before-input -s 82.208.81.0/24 -j DROP
-A ufw-before-input -s 82.208.93.160/27 -j DROP
-A ufw-before-input -s 83.149.42.64/29 -j DROP
-A ufw-before-input -s 83.172.36.224/29 -j DROP
-A ufw-before-input -s 83.219.13.128/29 -j DROP
-A ufw-before-input -s 83.219.13.184/29 -j DROP
-A ufw-before-input -s 83.219.138.16/28 -j DROP
-A ufw-before-input -s 83.219.23.48/29 -j DROP
-A ufw-before-input -s 83.219.23.8/29 -j DROP
-A ufw-before-input -s 83.219.25.0/29 -j DROP
-A ufw-before-input -s 83.219.25.112/29 -j DROP
-A ufw-before-input -s 83.219.5.248/29 -j DROP
-A ufw-before-input -s 83.219.6.72/29 -j DROP
-A ufw-before-input -s 83.220.53.16/28 -j DROP
-A ufw-before-input -s 83.229.181.192/26 -j DROP
-A ufw-before-input -s 83.229.211.192/29 -j DROP
-A ufw-before-input -s 83.229.232.16/29 -j DROP
-A ufw-before-input -s 83.237.68.168/29 -j DROP
-A ufw-before-input -s 83.237.77.44/30 -j DROP
-A ufw-before-input -s 83.237.79.64/29 -j DROP
-A ufw-before-input -s 83.237.87.8/29 -j DROP
-A ufw-before-input -s 83.237.91.216/29 -j DROP
-A ufw-before-input -s 83.237.93.101/32 -j DROP
-A ufw-before-input -s 83.237.93.188/30 -j DROP
-A ufw-before-input -s 83.242.145.224/27 -j DROP
-A ufw-before-input -s 83.69.207.248/29 -j DROP
-A ufw-before-input -s 84.204.143.44/30 -j DROP
-A ufw-before-input -s 84.204.154.16/30 -j DROP
-A ufw-before-input -s 84.204.170.220/30 -j DROP
-A ufw-before-input -s 84.204.217.164/30 -j DROP
-A ufw-before-input -s 84.204.245.208/29 -j DROP
-A ufw-before-input -s 84.204.7.144/29 -j DROP
-A ufw-before-input -s 84.53.210.144/28 -j DROP
-A ufw-before-input -s 85.114.30.192/30 -j DROP
-A ufw-before-input -s 85.114.30.204/30 -j DROP
-A ufw-before-input -s 85.114.93.88/29 -j DROP
-A ufw-before-input -s 85.141.17.112/30 -j DROP
-A ufw-before-input -s 85.141.17.24/30 -j DROP
-A ufw-before-input -s 85.141.18.80/30 -j DROP
-A ufw-before-input -s 85.141.19.56/30 -j DROP
-A ufw-before-input -s 85.141.21.236/30 -j DROP
-A ufw-before-input -s 85.141.28.0/30 -j DROP
-A ufw-before-input -s 85.141.31.68/30 -j DROP
-A ufw-before-input -s 85.141.32.96/28 -j DROP
-A ufw-before-input -s 85.141.33.0/28 -j DROP
-A ufw-before-input -s 85.141.33.64/28 -j DROP
-A ufw-before-input -s 85.141.60.96/28 -j DROP
-A ufw-before-input -s 85.141.61.160/28 -j DROP
-A ufw-before-input -s 85.143.125.0/24 -j DROP
-A ufw-before-input -s 85.21.102.224/28 -j DROP
-A ufw-before-input -s 85.21.103.64/28 -j DROP
-A ufw-before-input -s 85.21.104.192/27 -j DROP
-A ufw-before-input -s 85.21.148.0/26 -j DROP
-A ufw-before-input -s 85.21.149.48/28 -j DROP
-A ufw-before-input -s 85.21.155.208/28 -j DROP
-A ufw-before-input -s 85.21.157.48/28 -j DROP
-A ufw-before-input -s 85.21.204.208/28 -j DROP
-A ufw-before-input -s 85.21.99.48/28 -j DROP
-A ufw-before-input -s 85.21.99.64/28 -j DROP
-A ufw-before-input -s 85.236.29.160/27 -j DROP
-A ufw-before-input -s 85.90.100.72/29 -j DROP
-A ufw-before-input -s 85.90.101.112/28 -j DROP
-A ufw-before-input -s 85.90.101.192/29 -j DROP
-A ufw-before-input -s 85.90.102.168/29 -j DROP
-A ufw-before-input -s 85.90.120.72/29 -j DROP
-A ufw-before-input -s 85.90.121.72/29 -j DROP
-A ufw-before-input -s 85.90.125.96/29 -j DROP
-A ufw-before-input -s 85.90.127.16/29 -j DROP
-A ufw-before-input -s 85.90.98.144/30 -j DROP
-A ufw-before-input -s 85.90.99.168/29 -j DROP
-A ufw-before-input -s 86.102.100.48/28 -j DROP
-A ufw-before-input -s 86.102.108.32/28 -j DROP
-A ufw-before-input -s 86.102.109.32/28 -j DROP
-A ufw-before-input -s 86.102.109.48/28 -j DROP
-A ufw-before-input -s 86.102.115.80/28 -j DROP
-A ufw-before-input -s 86.102.126.160/28 -j DROP
-A ufw-before-input -s 86.102.126.80/28 -j DROP
-A ufw-before-input -s 86.102.72.240/28 -j DROP
-A ufw-before-input -s 86.102.74.64/28 -j DROP
-A ufw-before-input -s 87.117.18.144/29 -j DROP
-A ufw-before-input -s 87.117.20.128/28 -j DROP
-A ufw-before-input -s 87.117.20.64/27 -j DROP
-A ufw-before-input -s 87.117.20.96/27 -j DROP
-A ufw-before-input -s 87.117.21.0/29 -j DROP
-A ufw-before-input -s 87.117.21.16/29 -j DROP
-A ufw-before-input -s 87.117.21.24/29 -j DROP
-A ufw-before-input -s 87.117.21.32/29 -j DROP
-A ufw-before-input -s 87.117.21.40/29 -j DROP
-A ufw-before-input -s 87.117.21.48/29 -j DROP
-A ufw-before-input -s 87.117.21.56/29 -j DROP
-A ufw-before-input -s 87.117.21.64/29 -j DROP
-A ufw-before-input -s 87.117.21.72/29 -j DROP
-A ufw-before-input -s 87.117.21.8/29 -j DROP
-A ufw-before-input -s 87.117.21.80/29 -j DROP
-A ufw-before-input -s 87.117.23.128/28 -j DROP
-A ufw-before-input -s 87.117.31.56/29 -j DROP
-A ufw-before-input -s 87.225.56.224/28 -j DROP
-A ufw-before-input -s 87.226.156.64/26 -j DROP
-A ufw-before-input -s 87.226.191.0/24 -j DROP
-A ufw-before-input -s 87.226.213.0/24 -j DROP
-A ufw-before-input -s 87.226.239.180/30 -j DROP
-A ufw-before-input -s 87.237.42.208/29 -j DROP
-A ufw-before-input -s 87.237.47.204/30 -j DROP
-A ufw-before-input -s 87.245.133.0/24 -j DROP
-A ufw-before-input -s 87.249.16.32/28 -j DROP
-A ufw-before-input -s 87.249.18.60/30 -j DROP
-A ufw-before-input -s 87.249.22.72/29 -j DROP
-A ufw-before-input -s 87.249.28.232/29 -j DROP
-A ufw-before-input -s 87.249.3.64/28 -j DROP
-A ufw-before-input -s 87.249.30.176/30 -j DROP
-A ufw-before-input -s 87.249.5.48/30 -j DROP
-A ufw-before-input -s 87.249.7.120/29 -j DROP
-A ufw-before-input -s 88.151.200.0/24 -j DROP
-A ufw-before-input -s 88.200.208.112/29 -j DROP
-A ufw-before-input -s 88.83.195.248/30 -j DROP
-A ufw-before-input -s 89.106.172.160/29 -j DROP
-A ufw-before-input -s 89.107.123.120/29 -j DROP
-A ufw-before-input -s 89.107.123.136/29 -j DROP
-A ufw-before-input -s 89.107.127.136/29 -j DROP
-A ufw-before-input -s 89.109.250.88/29 -j DROP
-A ufw-before-input -s 89.109.7.176/29 -j DROP
-A ufw-before-input -s 89.111.176.0/22 -j DROP
-A ufw-before-input -s 89.175.10.160/30 -j DROP
-A ufw-before-input -s 89.175.161.224/30 -j DROP
-A ufw-before-input -s 89.175.165.208/28 -j DROP
-A ufw-before-input -s 89.175.170.144/28 -j DROP
-A ufw-before-input -s 89.175.174.136/29 -j DROP
-A ufw-before-input -s 89.175.176.140/30 -j DROP
-A ufw-before-input -s 89.175.176.176/30 -j DROP
-A ufw-before-input -s 89.175.176.88/30 -j DROP
-A ufw-before-input -s 89.175.188.184/29 -j DROP
-A ufw-before-input -s 89.175.6.64/27 -j DROP
-A ufw-before-input -s 89.175.8.104/30 -j DROP
-A ufw-before-input -s 89.175.8.140/30 -j DROP
-A ufw-before-input -s 89.175.8.192/30 -j DROP
-A ufw-before-input -s 89.175.8.36/30 -j DROP
-A ufw-before-input -s 89.175.8.40/30 -j DROP
-A ufw-before-input -s 89.175.8.44/30 -j DROP
-A ufw-before-input -s 89.175.8.52/30 -j DROP
-A ufw-before-input -s 89.175.8.68/30 -j DROP
-A ufw-before-input -s 89.175.82.240/29 -j DROP
-A ufw-before-input -s 89.175.9.4/30 -j DROP
-A ufw-before-input -s 89.179.155.192/28 -j DROP
-A ufw-before-input -s 89.179.179.16/28 -j DROP
-A ufw-before-input -s 89.179.181.0/24 -j DROP
-A ufw-before-input -s 89.21.129.16/28 -j DROP
-A ufw-before-input -s 89.21.140.104/29 -j DROP
-A ufw-before-input -s 89.21.152.104/29 -j DROP
-A ufw-before-input -s 89.28.253.168/29 -j DROP
-A ufw-before-input -s 89.28.255.56/29 -j DROP
-A ufw-before-input -s 90.150.176.52/30 -j DROP
-A ufw-before-input -s 90.150.189.128/29 -j DROP
-A ufw-before-input -s 90.150.189.136/29 -j DROP
-A ufw-before-input -s 90.150.189.144/29 -j DROP
-A ufw-before-input -s 90.150.189.152/29 -j DROP
-A ufw-before-input -s 90.150.189.160/29 -j DROP
-A ufw-before-input -s 90.150.189.168/29 -j DROP
-A ufw-before-input -s 90.150.189.176/29 -j DROP
-A ufw-before-input -s 90.150.189.184/29 -j DROP
-A ufw-before-input -s 90.150.189.192/29 -j DROP
-A ufw-before-input -s 90.150.189.200/29 -j DROP
-A ufw-before-input -s 90.150.189.208/29 -j DROP
-A ufw-before-input -s 90.150.189.216/29 -j DROP
-A ufw-before-input -s 90.150.189.224/29 -j DROP
-A ufw-before-input -s 90.150.189.232/29 -j DROP
-A ufw-before-input -s 90.150.189.248/29 -j DROP
-A ufw-before-input -s 90.150.189.32/29 -j DROP
-A ufw-before-input -s 91.103.194.184/29 -j DROP
-A ufw-before-input -s 91.215.168.0/22 -j DROP
-A ufw-before-input -s 91.217.34.0/23 -j DROP
-A ufw-before-input -s 91.219.192.0/22 -j DROP
-A ufw-before-input -s 91.221.140.0/23 -j DROP
-A ufw-before-input -s 91.221.140.0/24 -j DROP
-A ufw-before-input -s 91.221.141.0/24 -j DROP
-A ufw-before-input -s 91.226.250.0/24 -j DROP
-A ufw-before-input -s 91.227.32.0/24 -j DROP
-A ufw-before-input -s 92.101.253.152/29 -j DROP
-A ufw-before-input -s 92.39.106.168/30 -j DROP
-A ufw-before-input -s 92.39.106.20/30 -j DROP
-A ufw-before-input -s 92.39.111.84/30 -j DROP
-A ufw-before-input -s 92.39.128.0/21 -j DROP
-A ufw-before-input -s 92.50.198.124/30 -j DROP
-A ufw-before-input -s 92.50.198.72/30 -j DROP
-A ufw-before-input -s 92.50.219.136/29 -j DROP
-A ufw-before-input -s 92.50.238.224/29 -j DROP
-A ufw-before-input -s 92.60.186.0/28 -j DROP
-A ufw-before-input -s 93.153.135.88/30 -j DROP
-A ufw-before-input -s 93.153.136.132/30 -j DROP
-A ufw-before-input -s 93.153.144.60/30 -j DROP
-A ufw-before-input -s 93.153.171.204/30 -j DROP
-A ufw-before-input -s 93.153.172.100/30 -j DROP
-A ufw-before-input -s 93.153.175.44/30 -j DROP
-A ufw-before-input -s 93.153.183.104/30 -j DROP
-A ufw-before-input -s 93.153.194.160/29 -j DROP
-A ufw-before-input -s 93.153.220.192/29 -j DROP
-A ufw-before-input -s 93.153.223.8/29 -j DROP
-A ufw-before-input -s 93.153.229.232/29 -j DROP
-A ufw-before-input -s 93.153.244.188/30 -j DROP
-A ufw-before-input -s 93.153.244.248/29 -j DROP
-A ufw-before-input -s 93.153.251.0/24 -j DROP
-A ufw-before-input -s 93.178.104.32/30 -j DROP
-A ufw-before-input -s 93.178.104.36/30 -j DROP
-A ufw-before-input -s 93.178.104.64/30 -j DROP
-A ufw-before-input -s 93.178.104.68/30 -j DROP
-A ufw-before-input -s 93.178.106.0/26 -j DROP
-A ufw-before-input -s 93.182.23.48/29 -j DROP
-A ufw-before-input -s 93.188.20.72/29 -j DROP
-A ufw-before-input -s 93.190.110.0/24 -j DROP
-A ufw-before-input -s 94.124.192.192/29 -j DROP
-A ufw-before-input -s 94.199.64.0/21 -j DROP
-A ufw-before-input -s 94.25.119.228/30 -j DROP
-A ufw-before-input -s 94.25.53.56/29 -j DROP
-A ufw-before-input -s 94.25.57.176/29 -j DROP
-A ufw-before-input -s 94.25.57.224/28 -j DROP
-A ufw-before-input -s 94.25.65.16/29 -j DROP
-A ufw-before-input -s 94.25.70.64/30 -j DROP
-A ufw-before-input -s 94.25.90.240/29 -j DROP
-A ufw-before-input -s 94.25.95.136/30 -j DROP
-A ufw-before-input -s 95.167.113.48/30 -j DROP
-A ufw-before-input -s 95.167.114.48/30 -j DROP
-A ufw-before-input -s 95.167.121.68/30 -j DROP
-A ufw-before-input -s 95.167.122.128/28 -j DROP
-A ufw-before-input -s 95.167.142.32/30 -j DROP
-A ufw-before-input -s 95.167.157.156/30 -j DROP
-A ufw-before-input -s 95.167.162.236/30 -j DROP
-A ufw-before-input -s 95.167.162.76/30 -j DROP
-A ufw-before-input -s 95.167.176.0/23 -j DROP
-A ufw-before-input -s 95.167.2.4/30 -j DROP
-A ufw-before-input -s 95.167.21.104/29 -j DROP
-A ufw-before-input -s 95.167.213.0/24 -j DROP
-A ufw-before-input -s 95.167.29.104/29 -j DROP
-A ufw-before-input -s 95.167.4.168/29 -j DROP
-A ufw-before-input -s 95.167.5.64/28 -j DROP
-A ufw-before-input -s 95.167.5.80/28 -j DROP
-A ufw-before-input -s 95.167.54.76/30 -j DROP
-A ufw-before-input -s 95.167.59.244/30 -j DROP
-A ufw-before-input -s 95.167.64.20/30 -j DROP
-A ufw-before-input -s 95.167.68.216/29 -j DROP
-A ufw-before-input -s 95.167.69.116/30 -j DROP
-A ufw-before-input -s 95.167.70.136/29 -j DROP
-A ufw-before-input -s 95.167.70.176/28 -j DROP
-A ufw-before-input -s 95.167.70.32/28 -j DROP
-A ufw-before-input -s 95.167.72.140/30 -j DROP
-A ufw-before-input -s 95.167.72.204/30 -j DROP
-A ufw-before-input -s 95.167.72.48/30 -j DROP
-A ufw-before-input -s 95.167.74.136/29 -j DROP
-A ufw-before-input -s 95.167.74.180/30 -j DROP
-A ufw-before-input -s 95.167.76.160/27 -j DROP
-A ufw-before-input -s 95.167.99.48/28 -j DROP
-A ufw-before-input -s 95.173.128.0/19 -j DROP
-A ufw-before-input -s 95.173.128.0/20 -j DROP
-A ufw-before-input -s 95.173.144.0/20 -j DROP
-A ufw-before-input -s 95.53.248.0/29 -j DROP
-A ufw-before-input -s 95.54.193.80/28 -j DROP
```  

</details>

Делаем тоже самое с файлом before6.rules:

```
# Restrict GRCHC
-A ufw6-before-input -s 2a0c:a9c7:156::/48 -j DROP
-A ufw6-before-input -s 2a0c:a9c7:157::/48 -j DROP
-A ufw6-before-input -s 2a0c:a9c7:158::/48 -j DROP
```

**Обязательно оставьте `COMMIT` последней строкой!** Сохраните файлы.  

9. Придумайте или [сгенерируйте](https://www.random.org/) **5 новых случайных чисел от 1 до 65535** – они потребуются в качестве портов для настраиваемых протоколов и веб-панели Marzban.

>
> [!NOTE]
> Итого, учитывая ранее добавленные порты, всего их должно быть 6 штук:
> - Для SSH
> - Для веб-панели
> - Для VLESS/TCP
> - Для VLESS/GPRC
> - Для AmneziaWG
> - Для OpenVPN-over-Cloak

10. Выполните `ufw allow [ваше число]` соответственно 5 раз.
Например:
```
ufw allow 41567
ufw allow 13854
ufw allow 29875 и т. п.
```  
11. После выполняем команду `ufw reload`.

<details>
<summary>(Опционально) Подключение к серверу через SSH-ключ для удобства и безопасности</summary>

1. В Терминале Windows выполняем команду `ssh-keygen -t ed25519`, задаем имя файла и *(опционально)* пароль для ключа.  
Будут созданы два файла: публичный ключ **(файл с расширением .pub)** и файл приватного ключа авторизации **(без расширения, только заданное вами название).** Они будут сохранены по пути C:/Users/[Профиль].
2. Авторизуемся на сервере любым удобным способом и переходим к консоли, затем выполняем команду `mkdir ~/.ssh && touch ~/.ssh/authorized_keys && chmod 644 -R ~/.ssh`.
3. Переходим к редактированию на сервере – используем WinSCP или [nano.](https://help.ubuntu.ru/wiki/nano)  
В файле `/etc/ssh/sshd_config:`   
Раскоментируем и меняем значение `PasswordAuthentication` на `no`  
Раскомментируем строчку `PubkeyAuthentication` и меняем значение на `yes`  
Добавляем новую строчку `AuthenticationMethods publickey`  
Раскомментируем строчку `AuthorizedKeysFile`, а из её значения убираем `.ssh/authorized_keys2`  
В результате должно быть так: `AuthorizedKeysFile      .ssh/authorized_keys`. Сохраняем файл.

Возвращаемся к ранее созданному файлу публичного ключа, открываем его текстовым редактором и копируем содержимое **БЕЗ указания пользователя@хостнейма.** То есть без `username@something` в конце строчки.  
В результате должно быть нечто подобное: `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN3CGQ2UqRlaqdxwIdyAMkvFWDkvnEAqBRIPCqbfXvYG`.  
Переходим к файлу `/root/.ssh/authorized_keys`, открываем текстовым редактором и вставляем в него скопированное содержимое. Сохраняем файл.

4. Выполняем команду `systemctl reload ssh` для применения изменений.
5. Подключаемся к серверу через Терминал в Windows: ssh root@IP-сервера -i *(путь до файла приватного ключа)* -p *(порт для SSH)*. Вводим заданный ранее пароль к ключу.

Настраиваем авторизацию для WinSCP:  
* Выбираем **Новое подключение**
* Вводим всё то же, что в начале инструкции, кроме пароля от root
* Переходим в меню "Ещё..." → SSH/Аутентификация → Файл закрытого ключа → [...] (выбор файла) → меняем фильтр расширения на Все файлы → выбираем файл приватного ключа. Соглашаемся на конвертацию в необходимый формат, вводим пароль, если задавали, и сохраняем файл в безопасном месте. Он будет автоматически выбран в этом поле. Нажимаем ОК.
* Возвращаемся и подключаемся к серверу кнопкой Войти. Для удобства сохраните пресет авторизации этой сессии через ПКМ по вкладке с сервером → Сохранить подключение. Для авторизации в консоли через PuTTY нужно будет вводить пароль от ключа (но зачем оно нужно, когда можно использовать Терминал Windows?). Готово.
</details>

### Полуавтоматический метод
<sup>(всё то же самое, но из обычного Терминала и в чуть более облегчённом виде)</sup>

1. Открываем терминал и подключаемся к VDS (были выше описаны данные для подключения):
`ssh username@ip_address`
2. Соглашаемся с хостовыми ключами VDS, прописав `yes`. 
3. Если пользователь был `root` – дальше от его лица будут производиться действия, иначе, в зависимости от выбранной ОС на VDS хостера, подключаемся выданным пользователем и сменяем пользователя и привилегии командой `sudo -i`.
4. Выполняем обновление системы: `apt update && apt upgrade`.
5. Выполняем скрипт настройки системы. Пишем `nano prepare.sh` и вставляем следующее содержимое (напоминаю, выполняется всё и вся здесь от прав суперпользователя):
```sh
#!/usr/bin bash

declare -a ports

while getopts ":u:p:" opt; do
  case $opt in
    u)
      IFS=","
      ports=($OPTARG)
      unset IFS
      ;;
    p)
      port_num=($OPTARG)
      ;;
    \?)
      echo "Неверная опция: -$OPTARG" >&2
      exit 1
      ;;
  esac
done

echo "Настройка файервола"
ufw default deny incoming && ufw default allow outgoing
ufw allow $port_num
echo "Добавляем порты в разрешённые в файерволе:"
for ports in "${ports[@]}"; do
  ufw allow $ports
done

echo "Устанавливаем порт для SSH"
sed -i "s/^#Port 22/Port $port_num/" /etc/ssh/sshd_config

echo "Блокировка сканирования ГРЧЦ"
cat >> /etc/ufw/before.rules <<EOF
# drop GRCHC
-A ufw-before-input -s 185.224.228.0/22 -j DROP
-A ufw-before-input -s 195.209.120.0/22 -j DROP
-A ufw-before-input -s 212.192.156.0/22 -j DROP
-A ufw-before-input -s 194.165.22.0/24 -j DROP
-A ufw-before-input -s 194.165.23.0/24 -j DROP
-A ufw-before-input -s 195.209.122.0/22 -j DROP
-A ufw-before-input -s 185.224.231.0/24 -j DROP
-A ufw-before-input -s 185.224.230.0/24 -j DROP
-A ufw-before-input -s 185.224.229.0/24 -j DROP
-A ufw-before-input -s 212.192.158.0/24 -j DROP
COMMIT
EOF

echo "Перезапускаем сервисы"
systemctl restart ssh && systemctl restart networking && ufw enable
```
6. Выполняем скрипт. Порты для файервола на открытие перечисляем через запятую без пробелов после опции `-u`, например: `-u 1488,1337,228`, для порта SSH просто указать число после опции `-p` (`-p 11488`).  
Исходя из описанного, **команда запуска скрипта** будет следующая: `bash ./prepare.sh -u 1488,1337,228,7839,9475 -p 11488`
7. PROFIT. Всё сделалось само, никуда лезть не надо :)

## II. Устанавливаем и настраиваем Marzban
>
> [!NOTE]
> Инструкция актуальна на момент последнего редактирования этого раздела (13.04.2024).  
Если вы обнаружили, что в новых версиях присутствуют какие-либо несоответствия с гайдом, просим сообщить о них, создав Issue или предложив правки через Pull Request, так как Marzban – активно развивающийся проект, изменения в котором не всегда получается отслеживать своевременно. 
1. Выполните команду:  
`sudo bash -c "$(curl -sL https://github.com/Gozargah/Marzban-scripts/raw/master/marzban.sh)" @ install`

После установки выведутся логи. Для остановки вывода логов воспользуйтесь комбинацией Ctrl+C.

2. Создаём аккаунт админа для Marzban: `sudo marzban cli admin create --sudo`  
Используйте логин из рандомных символов и пароль как можно сложнее.
3. Переходим в WinSCP, идём по пути /opt/marzban. Правкой открываем файл .env. (или в терминале от `root` открываем файл для редактирования командой `nano /opt/marzban/.env`)
4. Находим самых верхние строчки `UVICORN_IP=` и `UVICORN_PORT=`.
```
UVICORN_IP=xx.xxx.xxx.xx (меняем на IP-адрес вашего сервера)
UVICORN_PORT=XXXXXX (пишем порт, который вы создавали для веб-панели)
```

5. Переходим к разделу Telegram. Находим строчки `TELEGRAM_API_TOKEN=`, `TELEGRAM_ADMIN_ID=` и *(опционально)* `TELEGRAM_LOGGER_CHANNEL_ID=` – раскомментируем каждую из них.  
* Открываем ваш клиент Telegram и обращаемся к [@BotFather.](https://t.me/BotFather) Создаём нового бота с рандомным названием и юзернеймом. В "поздравительном" сообщении об успешном создании бота находим токен доступа к боту, копируем и вставляем его в строчку `TELEGRAM_API_TOKEN=`.  
* Если у вас кастомный клиент/включено отображение ID в экспериментальных настройках: заходим в Настройки/свой профиль и копируем ID вашего аккаунта; если у вас официальный клиент, напишите любое сообщение в чат и ответьте на него же командой /id.  
* *(Опционально)* Создаём частный канал и добавляем в него вашего бота. Копируем ID канала и вставляем в строчку `TELEGRAM_LOGGER_CHANNEL_ID=`, **добавив префикс -100 (!)**. Некоторые клиенты, выдающие ID каналов через Bot API, сразу добавляют -100 – в таком случае добавлять префикс вручную не нужно, в общем, перепроверьте.  
После добавления бота в ваш частный канал, отключите разрешение на его добавление в другие каналы. Делается это в [@BotFather](https://t.me/BotFather) командой `/setjoingroups`. Там выберите нужного бота и переведите статус в Disabled.
6. Сохраняем отредактированный файл. Возвращаемся в окно PuTTY с консолью и выполняем команду `marzban restart`.
7. Marzban должен запуститься без ошибок в логах, а вывод в логе обновиться – теперь там будет указан IP-адрес вашего сервера с портом. Переходим по адресу вашей веб-панели, добавив `/dashboard/` в URL *(например: 134.43.57.54:43842/dashboard/)*. Логинимся в созданную ранее учетную запись админа. 
8. В интерфейсе веб-панели открываем меню (три полоски) → Настройки хоста → **VLESS TCP REALITY** → разворачиваем Дополнительные опции → меняем порт на тот, который придумали ранее для этого протокола; аналогично с **VLESS GRPC**. Сохраняем.
9. Открываем Настройки (иконка шестерёнки) → находим в конфигурации нужные протоколы: VLESS TCP REALITY, VLESS GRPC; меняем значение "port" на указанные ранее соответствующе каждому протоколу. Сохраняем, нажимаем Перезагрузить ядро.
10. Создаем нового пользователя (новый профиль). Придумайте ему любое название. Оставляем активным только протокол Vless (т.е. выбран только он, что индицируется в интерфейсе синей подсветкой) без ограничений по трафику и времени. Пусть этот профиль будет для телефона. Через NekoBox добавляем конфиги TCP и GRPC, отсканировав QR-коды (так будет проще). Выбираем профиль VLESS/tcp и нажимаем на кнопку самолётика снизу. 
11. Аналогично создаём такой же профиль для ПК. В Nekoray включаем режим системного прокси и добавляем профиль ссылкой из буфера. Ссылка должна начинаться с vless://xxx..., после добавления ПКМ по профилю VLESS/tcp → Запустить.

Проверяем, что VPN функционирует корректно на обоих устройствах. Если всё нормально, возвращаемся в WinSCP, проходим по пути /opt/marzban, открываем Правкой файл .env и меняем `UVICORN_IP` на `127.0.0.1`. Этим мы закрываем веб-панель в локальной сети (т.е. закрываем доступ к админ-панели извне), а дальнейшее управление профилями будем по необходимости производить через Telegram-бота.  
P.S. Telegram-бот по неизвестной мне причине иногда отваливается после перезапуска Marzban. Это фиксится само по себе по истечении ~5 минут. 

## III. Устанавливаем и настраиваем Amnezia
1. Скачиваем клиент [AmneziaVPN](https://amnezia.org/en/downloads) на ПК. Запускаем и выбираем Настроить свой сервер → У меня есть данные подключения → указываем IP-адрес сервера:порт для авторизации через SSH *(например 134.43.57.54:43842)*, логин root и пароль от сервера (не путать с паролем от админки Marzban, нужен тот пароль, который вы использовали в WinSCP). На экране появится прогресс-бар, ждём некоторое время.  
> Если вы используете авторизацию через SSH-ключ, в поле ввода пароля вставьте содержимое файла приватного ключа (не забывая в том числе и строки `-----BEGIN OPENSSH PRIVATE KEY-----`, `-----END OPENSSH PRIVATE KEY-----` – их тоже нужно копировать).
2. Откроется окно выбора предустановленных профилей. Листаем вниз и выбираем вариант "Выбрать протокол самостоятельно".
3. Выбираем AmneziaWG, указываем порт, который создавали для этого протокола. Ждём установки. 
4. После установки первого протокола мы попадаем на основную страницу клиента AmneziaVPN. Переходим по иконке Настроек → Серверы → выбираем наш сервер (Server 1). Здесь можно его переименовать. В разделе Протоколы находим OpenVPN over Cloak и рядом нажимаем на кнопку загрузки. Указываем порт, который ранее добавляли для этого протокола. Ждём окончания установки.
5. Чтобы добавить ваш сервер с AmneziaVPN на телефон, снизу выбираем кнопку Поделиться и добавляем юзеров для каждого протокола отдельно. Протокол AmneziaWG можно добавить QR-кодом, если повезёт, то OpenVPN over Cloak тоже. Если нет – жмём в окне с постоянно меняющимся QR-кодом "Поделиться" и сохраняем конфиг файлом, далее добавляем его на телефон.  
Управлять пользователями можно в разделе Поделиться → Пользователи.
