# ���������� ���� kittygram

![workflow](https://github.com/PARTYNEXTDOORS/kittygram_final/actions/workflows/main.yml/badge.svg)

___

### ����

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=blue) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB) ![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white) ![Gunicorn](https://img.shields.io/badge/gunicorn-%298729.svg?style=for-the-badge&logo=gunicorn&logoColor=white) ![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)

___
### �������� ������� 
Kittygram - ������ ��� ��������� �������.

��� ����� ������:

- ���������, �������������, ������������� � ������� �������.
- ��������� ����� � ����������� ��� ������������ ����������. 
- ������������� ����� ����� � �� ����������.

___
## ��������� 

1. ���������� ����������� �� ���� ���������:

    ```bash
    git clone git@github.com:PARTYNEXTDOORS/kittygram_final.git
    ```
    ```bash
    cd kittygram
    ```
2. �������� ���� .env � ��������� ��� ������ �������. �������� ������ ������ � �������� ���������� ������� � ����� .env.example.

___
### �������� Docker-�������

1.  �������� username �� ��� ����� �� DockerHub:

    ```bash
    cd frontend
    docker build -t username/kittygram_frontend .
    cd ../backend
    docker build -t username/kittygram_backend .
    cd ../nginx
    docker build -t username/kittygram_gateway . 
    ```

2. ��������� ������ �� DockerHub:

    ```bash
    docker push username/kittygram_frontend
    docker push username/kittygram_backend
    docker push username/kittygram_gateway
    ```

___
### ������ �� �������

1. ������������ � ���������� �������

    ```bash
    ssh -i ����_��_�����_�_SSH_������/��������_�����_�_SSH_������ ���_������������@ip_�����_������� 
    ```

2. �������� �� ������� ���������� kittygram ����� ��������

    ```bash
    mkdir kittygram
    ```

3. ��������� docker compose �� ������:

    ```bash
    sudo apt update
    sudo apt install curl
    curl -fSL https://get.docker.com -o get-docker.sh
    sudo sh ./get-docker.sh
    sudo apt-get install docker-compose-plugin
    ```

4. � ���������� kittygram/ ���������� ����� docker-compose.production.yml � .env:

    ```bash
    scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml
    * ath_to_SSH � ���� � ����� � SSH-������;
    * SSH_name � ��� ����� � SSH-������ (��� ����������);
    * username � ���� ��� ������������ �� �������;
    * server_ip � IP ������ �������.
    ```

5. ��������� docker compose � ������ ������:

    ```bash
    sudo docker compose -f docker-compose.production.yml up -d
    ```

6. ��������� ��������, �������� ����������� ����� ������� � ���������� �� � /backend_static/static/:

    ```bash
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
    ```

7. �� ������� � ��������� nano �������� ������ Nginx:

    ```bash
    sudo nano /etc/nginx/sites-enabled/default
    ```

8. �������� ��������� location � ������ server:

    ```bash
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
    ```

9. ��������� ����������������� ������� Nginx:

    ```bash
    sudo nginx -t
    ```
    ���� ����� � ��������� �����, ������, ������ ���:
    ```bash
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
    ```

10. ������������� Nginx
    ```bash
    sudo service nginx reload
    ```

___
### ��������� CI/CD

1. ���� workflow ��� �������. �� ��������� � ����������

    ```bash
    kittygram/.github/workflows/main.yml
    ```

2. ��� ��������� ��� �� ����� ������� �������� ������� � GitHub Actions:

    ```bash
    DOCKER_USERNAME                # ��� ������������ � DockerHub
    DOCKER_PASSWORD                # ������ ������������ � DockerHub
    HOST                           # ip_address �������
    USER                           # ��� ������������
    SSH_KEY                        # ��������� ssh-���� (cat ~/.ssh/id_rsa)
    SSH_PASSPHRASE                 # ������� ����� (������) ��� ssh-�����

    TELEGRAM_TO                    # id ��������-�������� (����� ������ � @userinfobot, ������� /start)
    TELEGRAM_TOKEN                 # ����� ���� (�������� ����� ����� � @BotFather, /token, ��� ����)
    ```

___
###  ��� �������� � ������������ ���������� �������

#### ��� ����� �������

��������� ������ ������� Kittygram � ����������� � CI/CD � ������� GitHub Actions

#### ��� ��������� ������ � ������� ����������

� ����� ����������� �������� ���� tests.yml �� ��������� ����������:
```yaml
repo_owner: ���_�����_��_�������
kittygram_domain: ������ ������ (https://��������_���) �� ��� ������ Kittygram
taski_domain: ������ ������ (https://��������_���) �� ��� ������ Taski
dockerhub_username: ���_�����_��_���������
```

���������� ���������� ����� `.github/workflows/main.yml` � ���� `kittygram_workflow.yml` � �������� ���������� �������.

��� ���������� ������� ������ �������� ����������� ���������, ���������� � ���� ����������� �� backend/requirements.txt � ��������� � �������� ���������� ������� `pytest`.

#### ���-���� ��� �������� ����� ��������� �������

- ������ Taski �������� �� ��������� �����, ���������� � `tests.yml`.
- ������ Kittygram �������� �� ��������� �����, ���������� � `tests.yml`.
- ��� � ����� main ��������� ������������ � ������ Kittygram, � ����� ��������� ������ ��� �������� ��������� � ��������.
- � ����� ������� ���� ���� `kittygram_workflow.yml`.

�����: [������ �������](https://github.com/McCloudin21) :+1:
