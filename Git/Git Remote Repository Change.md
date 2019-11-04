# Git Remote Repository Change

내가 생성한 `Repository`의 원격 저장소를 변경하거나,  
다른 `Repository`를 `Fork`해서 작업하던 `Repository`를 최신화 해 줄때 사용한다.

## Old `Repository` 최신화

저장소를 최신화하기 위해서는 먼저 저장소를 코드를 `Pull`(=Update) 하여 갱신을 하고, 내 로컬에서 작업하고 있는 코드를 모두 `Push`(=CheckOut) 해준다.

```shell
$ git pull
$ git add .
$ git commit -m "Local code commit all."
$ git push
```

## Old `Repository`의 Remote 정보 확인

저장소에 연결되어 있는 `Remote Repository`의 정보를 확인 할 수 있다.

```shell
$ git remote -v
```

## Old `Repository` Remote 제거

저장소에 연결되어 있는 `Remote Repository`를 제거한다.  
`Remote Repository`는 `origin`이라는 별칭으로 등록되어 있기에 해당 별칭을 사용하여 제거

```shell
$ git remote remove origin
```

## New `Repository` Reomote 추가

`origin`이라는 별칭으로 사용하던 `Remote Repository`를 삭제 하고 난 후,  
다시 `origin`이라는 별칭으로 `Remote Repository`를 새로 등록해 준다.

```shell
$ git remote add origin ${신규 저장소 주소 : https://github.com/계정/리포지토리.git}
```

새로운 `Remote Repository`를 연결했다면 `Pull`하여 최신화된 코드로 갱신하여 사용한다.