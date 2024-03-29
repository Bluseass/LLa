# !Lab 4. Dockerfile.md

Dockerfile — это текстовый файл с инструкциями, необходимыми для создания образа контейнера. Эти инструкции включают идентификацию существующего образа, используемого в качестве основы, команды, выполняемые в процессе создания образа, и команду, которая будет выполняться при развертывании новых экземпляров этого образа контейнера.

```sh
# FROM
Задаёт родительский образ.

# LABEL
Описывает метаданные. Например — сведения о том, кто создал и поддерживает образ.

# ENV
Устанавливает постоянные переменные среды.

#RUN
Выполняет команду и создаёт слой образа. Используется для установки в контейнер пакетов.

#COPY
Копирует в контейнер файлы и папки.

# ADD
Копирует файлы и папки в контейнер, может распаковывать локальные .tar-файлы.

# CMD
Описывает команду с аргументами, которую нужно выполнить когда контейнер будет запущен. Аргументы могут
быть переопределены при запуске контейнера. В файле может присутствовать лишь одна инструкция CMD.

# WORKDIR
Задаёт рабочую директорию для следующей инструкции.

# ARG
Задаёт переменные для передачи Docker во время сборки образа.

# ENTRYPOINT
Предоставляет команду с аргументами для вызова во время выполнения контейнера. Аргументы не переопределяются.

# EXPOSE
Указывает на необходимость открыть порт.

# VOLUME
Создаёт точку монтирования для работы с постоянным хранилищем.
```

Вот наглядый пример создания полноценного Dockerfile
```sh
FROM python:3.7.2-alpine3.8
LABEL maintainer="nekrasov.d.a@outlook.com"
# Устанавливаем зависимости
RUN apk add --update git
# Задаём текущую рабочую директорию
WORKDIR /usr/src/my_app_directory
# Копируем код из локального контекста в рабочую директорию образа
COPY . .
# Задаём значение по умолчанию для переменной
ARG my_var=my_default_value
# Настраиваем команду, которая должна быть запущена в контейнере во время его выполнения
ENTRYPOINT ["python", "./app/my_script.py", "my_var"]
# Открываем порты
EXPOSE 8000
# Создаём том для хранения данных
VOLUME /my_volume
```

## Соберём образ
Наш образ будет состоять из nginx и index.html(Код в конце).

Для начала соберём наш образ
```sh
docker build . -t localhost:5000/nginx:0.0.1
```

Запустим наш образ
```sh
docker run -itd -p 8011:80 --name nginx-dock localhost:5000/nginx:0.0.1
```

Логи нашего контейнера
```sh
docker logs nginx-dock
```

Используя docker build можно автомотизировано собирать образы, которая сама будет последовательно выполнять команды.
```sh
docker build -t test/myapp.
```

## Veu.js

Установим Vite:
```sh
sudo apt install vite
```

Установим Node.js и Npm:
```sh
sudo apt install npm
```

Проиницилизируем проект на Vue.js:
```sh
npm init vite@latest testvue --template vue
# Выбираем vue ->vue
cd testvue
npm install
npm run dev
```

Cоздадим Dockerfile:
```sh
FROM node:lts-alpine as build-stage
RUN apk update && apk upgrade
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build


FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Соберём образ и получим следующее:
```sh
# Собираем образ:
docker build -t deploytest

# Запустим образ:
docker run -itd -p 8002:80 --name deploytest deploytest 
```

Результат будет следующим:
```sh
#Удаляем все лишнее
docker system prune -a

Deleted build cache objects:
sg7294tjxh6eo3g71llmylwm6
y46vthp6yizut25ulpwl988p3
x0z6015n2senlegh7s8612hsu
gm9k1zquzehuo3z9bjecyrkea
thtv0um1563eznvh760iaf2sy
g81kqnq49xmrtcl8dqu57ojsd
478w2jvxl6adh3g3deoe4hduo
qgj226cd0dzg946634cjzrubj
uxvbb3pfvgs3j4g0zs5obic2y
xexqloaf323ycj6h7yjjx5ijz
pmmopiyur3lzz3mfty0i0qeu1
mlde30gzciimv816yain9pjz1
ojc30pi5d2j5g67zi1ehwps50
8l0dvlsjb0vsc70zn0zvnxv30
qqwiiy24kdgw7bsa9jz7op3hx
ztgg7f7x49mfvbq57047tsb0w
wuseyqvsp1nbuh3cuav1ho596
7gugxrt210h39tknemy8gakcg
t8ivz1c0cc9h1yz9bcrm64m9x
fduzii0a96t8ybwelduebgnx6
so9izog9u2bwyz2tuyhmfhcfq
acpruu6xep4g9m8uvd6w9a16r
kuw5cy98150a1p77vrpsgtr0t
q6u6wdnzmnvaske8113od8p1q
yqtd1yay24a7jzwjg9lubxo69
ko5qoq8drc842wnucwir0juiv
zjpc71d9szkep9dk12cc0rw8h
vakysvbtmebnbasb4xe3r44m0
hhtb09auocfgrlo0xrr9mxkdk
svbwh8gn2xthen3gyharvh1i7
fg6bxa3b0lyuejpw7b7c6pvrx
j6c8pe2quqjd6umony8uu1rx3
mq0pzvmsbdiw23l632qtep35i
201mcmjiedzrl7z005y454xrz
6ifgixgv1h6vms46kt7i6k8mv
b6xpe8phk7rbuxim0opdtdr9r
g84cf8elfpgibl3loq8ra3lpn
qn8n9jl5hv9e13a5ffodpld3f
50z007lcqlj6rwc6d0w9o19s7
m7pihu7q085q2v7j86oarlm5r
h9umrvh4ppsmau3j0igtw6arc
ons6dxqwrp3x8qb9ssygmx75c
krdny7ahmlc2839lnp3efrbds
fs0rbxrrl8edevzrxnei9spu2
vfo01ugimaqhpswxkdiw5xde5
vub9lguhmr0hhbyh71zh0ucda
t64wvm56gso653xcgh4ipzf4l
njc9totybp27jajcop99gm2vu
16oumsh3lxrcm72bfeo4yfmul
1yi524d3lmah0nhfmtjpni6hi
vggcu7w4230aihe6fhy88eai0
uu4xd2wpa7z031agjrzibniak
3jq2ck1zcvkb3r41r655pyjhs
40d85df66ja5w89ia3ehpi539
lg0bwlf8y4tuwecmtlltdix5q
r2h93cnhv903ztlgqt8dk5oit
lhufhkmhyqvpermho3b1wd9dw
ll13bqbey1aguyndtahvd1nfg
lwdcgu4qje6vs1ucd172gwf7q
xk0ygdsixt04kjy8dhujprqpw
sxwaxz9h3ajwwa6zaiewz2t4p
hcamn76e14o7g9hla3s8b75n0
e62e6epy95es560tlmdfz3tfh
fbyhl372srokys83cvv9vpd18
jrkhp5tnn8roiv9p7g4cwj26w
hw930gq88unbwubbhdbxpbe89
k9zz7x2tdj2z72k44fbtt8glz
```