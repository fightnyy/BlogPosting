# FastAPI - gunicorn, uvicorn

* 개발환경에서 FastAPI 를 구동할 때는 보통 `Uvicorn`을 사용한다.
* 하지만 `uvicorn`은 `single process` 로만 동작하기 때문에 수많은 Request가 발생하는 Production 환경에서는 분명한 한계가 있다.
* 따라서 Multi Process 를 사용하고 관리할 수 있는 `WSGI` 서버인 `gunicorn` 을 이용하여 서버를 구동하는게 배포환경에서 가장 좋은 선택임

* `WSGI` 인 `gunicorn` 을 사용하면 빠른 속도의 장점인 `ASGI` 인 `uvicorn` 을 사용할 수 없다는 생각을 할 수 있는데 `uvicorn ` 에서 `gunicorn worker class` 를 제공하기 때문에 **`gunocorn` 의 장점인 Multi Process 관리와 `uvicorn` 의 장점인 강력한 성능 이점을 모두 활용하여 `ASGI` 서버를 구동할 수 있다. **



```shell
gunicorn -k uvicorn.workers.UvicornWorker --access-logfile ./gunicorn-access.log main:app --bind 0.0.0.0:8000 --workers 2 --daemon
```

* -k uvicorn.workers.UvicornWorker 
  * `uvicorn worker` 클래스를 사용한다.
* -access-logfile ./gunicorn-access.log: `gunicorn` 로그 파일을 기록한다.
* main:app 
  * main.py의 app을 실행한다.
* -workers 2
  * worker process의 개수를 설정한다. 통상 `CPU 코어 개수` * 2 로 설정한다
* --daemon 
  * `gunicorn` 을 __백그라운드 데몬상에서 구동__ 합니다.
* --bind 0.0.0.0:8000
  * __8000 포트에 서버를 연결__합니다. 예를 들어 8000 포트로 bind 사용자는 <서버주소>:8000 으로 서버에 접속이 가능하다.





# FastAPI - Dependency



## Dependency를 쓰는 경우

1. 같은 로직을 사용하는 경우
2. database connnections 을 공유할 때
3. 인증과, 인가, 보안등을 강요할 때
4. 등등

