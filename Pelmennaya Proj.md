
# Привет Я [БОРИС](https://www.linkedin.com/in/boris-gorbunov-61a54320/) ![](https://github.com/blackcater/blackcater/raw/main/images/Hi.gif) 
### Разворачиваем Пельменную в Лучшем Облачном Сервисе в  РОССИИ 🇷🇺
# Pelmennaya Project Yandex Cloud

## Репозиторий CI/CD сборка приложения:
- https://gitlab.praktikum-services.ru/std-009-076/pelmennaya

## Репозиторий инфраструктуры: 
- https://gitlab.praktikum-services.ru/std-009-076/pelmennaya_infrastructure

## ArgoCD:
- https://argo.24momo.ru

## Grafana:
- https://gf.24momo.ru

## Prometheus:
- https://pm.24momo.ru

## Структура програмного репозитория branch:main

```bash
pelmennaya
├── backend               ## Код фронтенд с файлом сборки контейнера 
│   ├── Dockerfile  
│   ├── .gitlab-ci.yml
├── frontend              ## Код фронтенд с файлом сборки контейнера и конфигурацией NGINX
│   ├── Dockerfile
│   ├── .gitlab-ci.yml
│   ├── nginx-frontend.conf  
├── .gitignore
└── .gitlab-ci.yml 
```

--------------------------------

## Структура инфраструктурного репозитория branch:main



## Helm Charts ArgoCD and Image Updater 


```bash
├── argo
│   └── argo-ingress.yaml    ## Ingress Conrtroller для  Argocd 
├── argo-image-updater
│   ├── argocd-helm-charts   
│   │   ├── argocd-application-set.yaml  ## Создание приложения для ArgoCD
│   │   ├── argocd-project.yaml          ##  Создание проекта для ArgoCD
│   └── secrets  
│       └── json-secret-iu.yaml  # используется для подключения к Gitlab rRgistry
```

----------------------------
# Helm Charts приложения в GitLab
 - Helm Chart для деплоя приложения.
 - Используется AplicationSet манифестами для загрузки данных из репозиториев.
```bash
├── momo-store-helm-charts    
│   ├── .argocd-source-momo-store-helm-charts.yaml
│   ├── charts
│   │   ├── backend
│   │   │   ├── Chart.yaml
│   │   │   ├── templates
│   │   │   │   ├── deployment.yaml
│   │   │   │   ├── secrets.yaml
│   │   │   │   └── service.yaml
│   │   │   └── values.yaml
│   │   ├── frontend
│   │   │   ├── Chart.yaml
│   │   │   ├── templates
│   │   │   │   ├── deployment.yaml
│   │   │   │   ├── secrets.yaml
│   │   │   │   └── service.yaml
│   │   │   └── values.yaml
│   │   └── ingress
│   │       ├── Chart.yaml
│   │       └── templates
│   │           └── deployment.yaml
│   ├── Chart.yaml
│   ├── core-install.yaml
│   └── values.yaml
```

## Мониторинг
 -  Манифесты для развертывания приложение мониторинга GRAFANA


```bash
├── monitoring
│   ├── grafana
│   │   └── gf-helm-charts
│   │       ├── cert
│   │       │   └── gf-cert.yaml
│   │       ├── Chart.yaml
│   │       ├── .helmignore
│   │       └── templates
│   │           ├── deployment.yaml
│   │           ├── gf-cert.yaml
│   │           ├── _helpers.tpl
│   │           ├── ingress.yaml
│   │           ├── pvc.yaml
│   │           └── services.yaml
```

## Сбор Метрик
 -  Манифесты для развертывания приложение - PROMETHEUS
```bash
│   └── prometheus
│       ├── cert
│       │   └── pm-cert.yaml
│       ├── pm-helm-charts
│       │   ├── configmap.yaml
│       │   ├── deployment.yaml
│       │   ├── ingress.yaml
│       │   └── services.yaml
│       └── RBAC.yaml
```

## Certificate Manager и SecretStore
 -  Манифесты  SecretStore и сертификатов
```bash
Certificates
├── SecretStore.sh 
├── argocd-cert.sh
├── grafana-ext-secrets.sh
├── pelmennaya-store.sh
└── prometheus.sh
```

## Манифесты Кубернетес
 -  Манифесты развертывания кубернетес кластера
```bash
└── terraform
    ├kubernetes
      ├── kubernetes.tf
      └── var.tf

```

-----------------------------------

## Подготовка инфраструктуры:
![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)

## Resources

1. Скачайте и установите терраформ:

- https://releases.hashicorp.com/terraform/1.4.2/terraform_1.4.2_darwin_amd64.zip

Разархивируйте и перенесите бинарные файлы в нужный каталог:
```bash
sudo mv ~/Downloads/terraform /usr/local/bin/
```
настройте провайдера Terraform: 
 - https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#configure-terraform


2. Ключ сервисного аккаунта
 Установите Yandex CLI: https://cloud.yandex.ru/docs/cli/operations/install-cli  
Чтобы управлять инфраструктурой Yandex Cloud с помощью Terraform, используйте сервисный аккаунт!
Если у вас еще нет то создайте его :

```bash
yc iam service-account create --name sa-acc
```

Прверка:

```bash
yc iam service-account list
```

Результат:

|          ID          |       NAME       |   DESCRIPTION   |
| --------------------- | ------------------ | -----------------|
| aje6o61dvog2h6g9a33s | my-robot         | my description  |

Добавьте роль editor для расширенных прав.

```bash
yc resource-manager folder add-access-binding my-folder \
  --role editor \
  --subject serviceAccount:aje6o61dvog2h6g9a33s
```

Где
- aje6o61dvog2h6g9a33s - ID сервисного аккаунта
-  editor - роль сервисного аккаунта

## Авторизованный ключ для созданного сервисного аккаунта
4. Создайте авторизованный ключ для созданного сервисного аккаунта. Сохраните его в файл. Он понадобится для работы с Терраформ.

```bash
yc iam key create \
  --service-account-name sa-acc \
  --output key.json
```
где 
- sa-acc имя вашего сервисного аккаунта.



## 4. Деплой Kubernetes

Репозиторий:  https://gitlab.praktikum-services.ru/std-009-076/pelmennaya_infrastructure 

- Файлы Terraform для деплоя Kubernetes кластера

Манифесты 
- Манифест для развёртывания Кубернетес (аннотации в файле)

Путь: pelmennaya_infrastructure/terraform/kubernetes/kubernetes.tf

- Переменные для кластера Кубернетес

Путь: pelmennaya_infrastructure/terraform/kubernetes/var.tf

## ЗАПУСКАЕМ ДЕПЛОЙ КУБЕРНЕТЕС
[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=%2336BCF7&lines=terraform+init)](https://git.io/typing-svg)
_Эта команда выполняет несколько различных шагов инициализации, чтобы подготовить текущий рабочий каталог для использования с Terraform._

[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=%2336BCF7&lines=terraform+plan)](https://git.io/typing-svg)
_Команда создает план выполнения, который позволяет предварительно просмотреть изменения_

     Считывает текущее состояние любых уже существующих удаленных объектов, чтобы убедиться, что состояние Terraform актуально.
     Сравнивает текущую конфигурацию с предыдущим состоянием и отмечает любые различия.
     Предлагает набор действий по изменению, применение которых должно привести удаленные объекты в соответствие с конфигурацией.

[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=%2336BCF7&lines=terraform+apply)](https://git.io/typing-svg)
_Команда terraform apply выполняет действия, предложенные в плане Terraform._


---------------------------

## Добавление DNS  зоны

 - Добавьте небходимые DNS зоны для вашего домена 

```bash
yc dns zone create --name momo-dnszone  --zone www.24momo.ru. --public-visibility
```

_Результат:_

```bash
id: aet2q4fn8i8icfug97p0
folder_id: aoerb349v3h4bupphtaf
created_at: "2021-03-03T19:07:08.685Z"
name: momo-dnszone 
zone: www.24momo.ru.
public_visibility: {}
```
_Вывод команды_

```bash
yc dns zone list
```

|          ID          |                    NAME                    |       ZONE       |          VISIBILITY          | 
|----------------------|--------------------------------------------|------------------|------------------------------|
| dns103fe510re3b5tn6f | momo-dnszone                               | 24momo.ru.       | PUBLIC                       | 

--------------------------------



## Добавление сертификатов домена:

 Ключ сервисного аккаунта

Используйте авторизованный ключ для ранее созданного сервисного аккаунта (k8s-bgorbunov) или создайте новый.  Сохраните его в файл. Он нам понадобится для создания "ExternalSecret*"

_*External Secrets Operator — это оператор Kubernetes, который интегрирует внешние системы управления секретами._

```bash
yc iam key create \
  --service-account-name k8s-bgorbunov \
  --output authorized-key.json
```


2. Назначение права SA account на управление сертификатами
 ```bash
yc cm certificate add-access-binding \
  --id fpqtea62d0ljseluiae8 \
  --service-account-name k8s-bgorbunov \
  --role certificate-manager.certificates.downloader
```

3. Добавление запроса на создание сертификатов

Пример:

```bash
yc certificate-manager certificate request \
  --name momo-cert \
  --domains 24momo.ru
```
Где:
- name — имя сертификата.
- domains — домены сертификатов.

Добавьте все небходимые сертификаты для доменов
- 24momo.ru
- argo.24momo.ru
- pm.24momo.ru
- gf.24momo.ru


## Вывод списка сертификатов

```bash
yc cm certificate list
```

| ID | NAME | DOMAINS| NOT AFTER | TYPE | STATUS |
| ------ | ------ | ------ | ------ | ------ | ------ |
| fpq0i5ulhtblldahkp6b | momo-cert      | 24momo.ru      | 2023-06-02 08:25:35 | MANAGED | ISSUED |
| fpqd425u24lfrmemo2gi | argo-24momo-ru | argo.24momo.ru | 2023-06-10 20:53:46 | MANAGED | ISSUED |
| fpqd991d3h8vhqqgt66p | pm-24momo-ru   | pm.24momo.ru   | 2023-06-12 07:51:43 | MANAGED | ISSUED |
| fpqtea62d0ljseluiae8 | gf-24momo-ru   | gf.24momo.ru   | 2023-06-12 07:48:30 | MANAGED | ISSUED |


## Проверьте, что права сервисному аккаунту назначены:
 
 ```bash
 yc cm certificate list-access-bindings --id  "ID сертификата"
```
 - Пример вывода

| ROLE ID |  SUBJECT TYPE | SUBJECT ID  |
| ------- |------------------------------- | ---------------------- |
| certificate-manager.certificates.downloader | serviceAccount | ajehu3kpdgs1763jee58 |


--------------------

## External Secrets

1. Добавьте Helm-репозиторий external-secrets:

```bash
helm repo add external-secrets https://charts.external-secrets.io
```
2. Установите External Secrets Operator в кластер Kubernetes:
```bash
helm install external-secrets \
  external-secrets/external-secrets \
  --namespace external-secrets \
  --create-namespace
```

## Создайте секрет yc-auth, содержащий в себе ключ сервисного аккаунта k8s-bgorbunov:
```bash
kubectl --namespace "Namespace_Name" create secret generic yc-auth \
  --from-file=authorized-key=authorized-key.json
```
Где
 - authorized-key.json - ваш ключь
 - "Namespace_Name" - неймспейс системы управления секретами

## Создайте хранилище секретов (SecretStore) secret-store, содержащее секрет yc-auth:
```bash
sudo ./pelmennaya_infrastructure/Certificates/secretstore.sh 
```

## Создайте объекты ExternalSecret external-secret, указывающий на сертификат из Certificate Manager:

External Secret для сайта магазина 24momo.ru 
 ```bash
sudo ./pelmennaya_infrastructure/Certificates/pelmennaya-store.sh
 ```
External Secret для сайта Argo CD — декларативный GitOps-инструмент непрерывной доставки (continuous delivery) для Kubernetes
 ```bash
sudo ./pelmennaya_infrastructure/Certificates/argocd-cert.sh 
 ```
External Secret для сайта мониторинга Grafana — свободная программная система визуализации данных, ориентированная на данные систем ИТ-мониторинга.
 ```bash
sudo ./pelmennaya_infrastructure/Certificates/grafana-ext-secrets.sh
 ```
External Secret для сайта Prometheus - записывает метрики в реальном времени в базу данных временных рядов.
 ```bash
sudo ./pelmennaya_infrastructure/Certificates/argocd-cert.sh
 ```


## kubectl get es -n argocd

| NAME |                   STORE    |      REFRESH INTERVAL |  STATUS   |      READY|
| --- |                   ---    |    --- |  ---   |     --- |
|external-secret   |     secret-store |  1h       |          SecretSynced  | True|
momo-external-secret  | secret-store  | 1h            |     SecretSynced  | True|

##  kubectl get es -n monitoring
| NAME |                   STORE    |      REFRESH INTERVAL |  STATUS   |      READY|
| --- |                   ---    |    --- |  ---   |     --- |
|gf-external-secret |  secret-store |  1h   |              SecretSynced |  True | 
|pf-external-secret |  secret-store  | 1h    |             SecretSynced |  True|




## Ingress-контроллер NGINX NLB
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx && \
helm repo update && \
helm install ingress-nginx ingress-nginx/ingress-nginx
# Нзначение ранее зарезервированного статического loadBalancerIP
kubectl patch svc ingress-nginx-lb -p '{"spec": {"loadBalancerIP": "Ранне созданный IP адресс"}}'
```



## Argocd Image Updater установка.


Argocd Image Updater — инструмент для автоматического обновления образов контейнеров рабочих нагрузок Kubernetes, которыми управляет Argo CD.

Добавьте репозиторий, содержащий дистрибутив argocd iu:

   ```bash
   helm repo add argo https://argoproj.github.io/argo-helm
   helm repo update
   ```

Установите argocd-Image Updater с предварительно настроенными значениями, 
/argo-image-updater/helm-manifests-argocd-image-updater/


   ```bash
   helm install --namespace argocd
     -f argocd-iu-values.yaml \
     argocd-image-updater argo/argocd-image-updater \
   ```
Добавить предварительно Pull Secret для работы с Registry GitLab:

/pelmennaya_infrastructure/argo-image-updater/secrets/json-secret-iu.yaml



Ingress Controller Manifest:
Certificate Manifest: argo-ingress.yaml   






- Import a HTML file and watch it magically convert to Markdown
- Drag and drop images (requires your Dropbox account be linked)
- Import and save files from GitHub, Dropbox, Google Drive and One Drive
- Drag and drop markdown and HTML files into Dillinger
- Export documents as Markdown, HTML and PDF















Markdown is a lightweight markup language based on the formatting conventions
that people naturally use in email.
As [John Gruber] writes on the [Markdown site][df1]

> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text you see here is *actually- written in Markdown! To get a feel
for Markdown's syntax, type some text into the left window and
watch the results in the right.

## Tech

Dillinger uses a number of open source projects to work properly:

- [AngularJS] - HTML enhanced for web apps!
- [Ace Editor] - awesome web-based text editor
- [markdown-it] - Markdown parser done right. Fast and easy to extend.
- [Twitter Bootstrap] - great UI boilerplate for modern web apps
- [node.js] - evented I/O for the backend
- [Express] - fast node.js network app framework [@tjholowaychuk]
- [Gulp] - the streaming build system
- [Breakdance](https://breakdance.github.io/breakdance/) - HTML
to Markdown converter
- [jQuery] - duh

And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

## Installation

Dillinger requires [Node.js](https://nodejs.org/) v10+ to run.

Install the dependencies and devDependencies and start the server.

```sh
cd dillinger
npm i
node app
```

For production environments...

```sh
npm install --production
NODE_ENV=production node app
```

## Plugins

Dillinger is currently extended with the following plugins.
Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantaneously see your updates!

Open your favorite Terminal and run these commands.

First Tab:

```sh
node app
```

Second Tab:

```sh
gulp watch
```

(optional) Third:

```sh
karma test
```

#### Building for source

For production release:

```sh
gulp build --prod
```

Generating pre-built zip archives for distribution:

```sh
gulp build dist --prod
```

## Docker

Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 8080, so change this within the
Dockerfile if necessary. When ready, simply use the Dockerfile to
build the image.

```sh
cd dillinger
docker build -t <youruser>/dillinger:${package.json.version} .
```

This will create the dillinger image and pull in the necessary dependencies.
Be sure to swap out `${package.json.version}` with the actual
version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on
your host. In this example, we simply map port 8000 of the host to
port 8080 of the Docker (or whatever port was exposed in the Dockerfile):

```sh
docker run -d -p 8000:8080 --restart=always --cap-add=SYS_ADMIN --name=dillinger <youruser>/dillinger:${package.json.version}
```

> Note: `--capt-add=SYS-ADMIN` is required for PDF rendering.

Verify the deployment by navigating to your server address in
your preferred browser.

```sh
127.0.0.1:8000
```

## License

MIT

**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
   
[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)
   
   
   
