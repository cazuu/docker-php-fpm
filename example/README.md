# For Laravel
1. copy current files to local
2. copy .env.example to .env.docker
3. vi .env.docker
   ```
   DB_CONNECTION=mysql
   DB_HOST=mysql
   DB_PORT=3306
   DB_DATABASE=database
   DB_USERNAME=root
   DB_PASSWORD=password

   MAIL_DRIVER=smtp
   MAIL_HOST=smtp
   MAIL_PORT=1025
   ```
4. type `make setup`

