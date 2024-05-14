## Для тех, кто забывает про sudo
```bash
bind '"\C-k":"\e[A\C-asudo \C-e"'   ## вызов предыдущей команды с приставкой sudo ctrl+k
bind '"\C-h":"\C-asudo \C-e"'       ## подстановка sudo в начало строки
```
## Рандомная папка в tmp и cd в нее
```bash
mkcdtemp()          
{
    randompempdirname="/tmp/$RANDOM$RANDOM"
    mkdir -p $randompempdirname && cd $randompempdirname
}
```
## Погода
```bash
pogoda()
{
    if [[ -z $1  ]] ; then 
            place='Moskow'
    else
        place="$1"
    fi  
    curl -4 wttr.in/$place
}
```
## Перемещает тег в гит на текущий коммит
```bash
git_replace_tag_remote(){
    tag_name=$1
    [[ -z "$tag_name" ]] && echo fail && return
    git tag -d $tag_name && \
    git push origin --delete $tag_name && \
    git tag $tag_name && \
    git push --tags && \
    echo success
}
```
## Пингует сервер пока не ответит, потом логинится по ssh
```bash
pssh(){
    host=$1
    username=$2
    [[ ! -z "$username" ]] && username="$username@"
    while true ; do
        if ping -c 1 $host ; then
            ssh $username$host
            break
        fi
    done
}
```
## Добавить ip к единственному интерфейсу
```bash
addip()
{
    sudo ip a a $1/24 dev enp2s0
}
```

## Примонтировать девайс/образ в /tmp/$path
```bash
mounttmp()
{
    mnttemp_do_cd=""

    case "$1" in
        "-cd")
            mnttemp_do_cd="1"
            shift
            ;;
    esac

        list=("$(ls $@)")
        [[ $? -ne '0' ]] && return
        for ii in $list ; do
                mkdir -p /tmp/$ii && sudo mount $ii /tmp/$ii && echo "/tmp/$ii"
        done
    [[ "$mnttemp_do_cd" == "1" ]] && cd /tmp/$ii

}
```

## Сделать папку и cd в нее
```bash
mkcd ()
{
        mkdir -p -- "$1" && cd -P -- "$1"
}
```
## Найти строку в текущем каталоге
```bash
grephere() 
{
    grep -Hnr $@ ./  
}
```



